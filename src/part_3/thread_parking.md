# Efficient Thread Parking

Efficient thread management is crucial for optimizing performance in concurrent applications. One strategy to minimize latency and CPU usage is thread parking.

In Rust, thread parking involves suspending a thread until it's needed again, allowing it to consume minimal CPU resources while remaining available for quick activation.

Consider the following example:

```rust
fn read_line() -> String {
    let mut input = String::new();
    std::io::stdin()
        .read_line(&mut input)
        .expect("Failed to read line");
    input.trim().to_string()
}

fn parkable_thread(n: u32) {
    loop {
        std::thread::park(); // Suspends the thread until awakened
        println!("Thread {} is awake - briefly!", n);
    }
}

fn main() {
    let mut threads = Vec::new();

    // Spawning 10 threads to demonstrate parking
    for i in 0..10 {
        let thread = std::thread::spawn(move || parkable_thread(i));
        threads.push(thread);
    }

    loop {
        println!("Enter a thread number to awaken, or 'q' to quit:");
        let input = read_line();

        if input == "q" {
            break;
        }

        if let Ok(number) = input.parse::<u32>() {
            if number < threads.len() as u32 {
                threads[number as usize].thread().unpark(); // Awakens the chosen thread
            }
        }
    }
}
```

In this example, threads are parked and awakened based on user input. The `std::thread::park()` function suspends the thread until it receives a wakeup signal, making it an efficient way to manage background tasks with minimal overhead.

However, thread parking has limitations, particularly when it comes to passing data between threads. We'll explore more advanced thread communication techniques in the next section.

By judiciously parking and awakening threads, you can achieve efficient background task execution while maintaining responsiveness in your applications. This approach is particularly useful in scenarios where threads need to remain dormant until triggered by specific events, such as user input or system events.
