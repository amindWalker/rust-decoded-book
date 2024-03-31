# Understanding Channels and Ownership

`Channels` provide a convenient means of transmitting data between threads, but managing **ownership** is crucial.

Attempting to pass a **reference** through a channel raises ownership concerns. If the calling thread may **outlive** the data, passing a reference is problematic. The borrow checker, particularly its "lifetime checker" component, will raise objections.

The recommended approach is to `move` the data. In this model, the sending thread transfers ownership of the data to the channel. This eliminates ambiguity regarding ownership: the channel exclusively possesses the data and can transfer it to another thread as needed.

Let's consider an example to illustrate this:

```rust
use std::sync::mpsc;

// Define a struct not copyable or clone-able
struct MyData {
    data: String,
    n: u32,
}

pub fn read_line() -> String {
    let mut input = String::new();
    std::io::stdin()
        .read_line(&mut input)
        .expect("Failed to read line");
    input.trim().to_string()
}

fn main() {
    let (tx, rx) = mpsc::channel::<MyData>();

    std::thread::spawn(move || {
        while let Ok(data) = rx.recv() {
            println!("--- IN THE THREAD ---");
            println!("Message number {}", data.n);
            println!("Received: {}", data.data);
        }
    });

    let mut n = 0;
    loop {
        println!("Enter a string");
        let input = read_line();
        let data_to_move = MyData {
            data: input,
            n,
        };
        n += 1;

        tx.send(data_to_move).unwrap();
    }
}
```

Moving data also offers performance benefits. Although it involves a `memcpy` operation internally, the optimizer typically eliminates this overhead.

Let's run a benchmark:

```rust
use std::sync::mpsc;

// Define a struct not copyable or clone-able
struct MyData {
    start: std::time::Instant,
}

pub fn read_line() -> String {
    let mut input = String::new();
    std::io::stdin()
        .read_line(&mut input)
        .expect("Failed to read line");
    input.trim().to_string()
}

fn main() {
    let (tx, rx) = mpsc::channel::<MyData>();

    std::thread::spawn(move || {
        while let Ok(data) = rx.recv() {
            let elapsed = data.start.elapsed();
            println!("--- IN THE THREAD ---");
            println!("Message passed in {} us", elapsed.as_micros());
        }
    });

    loop {
        println!("Enter a string");
        let _ = read_line();
        let data_to_move = MyData {
            start: std::time::Instant::now(),
        };

        tx.send(data_to_move).unwrap();
    }
}
```

The average time per message shoulbe be approximately less than 60 microseconds on any modern machine, which is sufficiently fast for most applications. This performance allows for comfortable data movement, even in resource-intensive scenarios.
