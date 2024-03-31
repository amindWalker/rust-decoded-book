# Dealing with Deadlocks and Errors

# Deadlocks

Deadlocks are a critical issue that can arise when locking data in concurrent programming. One particularly nasty consequence of locking data is a deadlock. In Rust, deadlocks occur when a thread blocks indefinitely, waiting for itself to release a lock. Rust provides mechanisms to help prevent deadlocks, but they must be used correctly as Rust cannot completely prevent them.

## Deadlocking Example

Consider the following simple example that demonstrates a deadlock:

```rust
use std::sync::Mutex;

fn main() {
    let my_shared = Mutex::new(0);

    let lock = my_shared.lock().unwrap();
    let lock = my_shared.lock().unwrap();
}
```

In this code, the program never stops due to attempting to acquire the Mutex twice, causing a deadlock. It waits indefinitely for itself to release the lock.

## Avoiding Deadlocks with Simple Error Handling

To avoid deadlocks, one approach is to use `try_lock` instead of `lock`:

```rust
use std::sync::Mutex;

static MY_SHARED: Mutex<u32> = Mutex::new(0);

fn main() {
    if let Ok(_lock) = MY_SHARED.try_lock() {
        println!("I got the lock!");

        if let Ok(_lock) = MY_SHARED.try_lock() {
            println!("I got the lock!");
        } else {
            println!("I couldn't get the lock!");
        }
    } else {
        println!("I couldn't get the lock!");
    }
}
```

However, `try_lock` does not wait and may fail, requiring careful handling to avoid busy loops.

## Explicitly Dropping Locks

Another way to manage locks is by explicitly dropping them:

```rust
let lock = MY_SHARED.lock().unwrap();
std::mem::drop(lock);
let lock = MY_SHARED.lock().unwrap();
```

Though it may seem cumbersome, dropping locks manually can be useful, especially when holding locks for extended periods.

## Cleaning Up Locks with Scopes

Using scopes is a more elegant solution to drop locks:

```rust
{
    let _lock = MY_SHARED.lock().unwrap();
    println!("I got the lock!");
}
let _lock = MY_SHARED.lock().unwrap();
println!("I got the lock again!");
```

This ensures that locks are automatically released when they go out of scope.

## Ownership of Mutex Guards

Consider the following example illustrating mutex guard ownership:

```rust
use std::sync::Mutex;

fn main() {
    let data = Mutex::new(vec![1, 2, 3]);

    {
        let mut guard = data.lock().unwrap();
        guard.push(4);
    } // Guard goes out of scope, unlocking the Mutex

    // Attempting to modify the data outside the guard’s scope would result in a compilation error
    // let mut guard = data.lock().unwrap(); // This line would fail to compile
    println!("Data: {:?}", data.lock().unwrap());
}
```
Mutex guards enforce Rust's ownership rules, ensuring thread safety and preventing data races. They guarantee that only one thread can modify the data at a time.

## Mutex Poisoning

If a thread crashes while holding a lock, the lock becomes poisoned. Rust's safety features mark the lock as poisoned to prevent data corruption.

```rust
use std::sync::{Mutex, PoisonError};

static DATA: Mutex<u32> = Mutex::new(0);

fn poisoner() {
    let mut lock = DATA.lock().unwrap();
    *lock += 1;
    panic!("And poisoner crashed horribly");
}

fn main() {
    let handle = std::thread::spawn(poisoner);
    println!("Trying to return from the thread:");
    println!("{:?}", handle.join());
    println!("Locking the Mutex after the crash:");
    let lock = DATA.lock();
    println!("{lock:?}");
}
```

Recovering from poisoning involves cautious handling:

```rust
let recovered_data = lock.unwrap_or_else(|poisoned| {
    println!("Mutex was poisoned, recovering data...");
    poisoned.into_inner()
});
println!("Recovered data: {recovered_data:?}");
```

## Effectively Handling Poisoned Mutexes

This example demonstrates a more complex handling of the poisoned mutexes:

```rust
use std::sync::{Mutex, Arc};
use std::thread;

fn main() {
    // Creating a Mutex-protected data structure wrapped in an Arc for shared ownership
    let data = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    // Spawning threads to work with the mutex-protected data
    for _ in 0..5 {
        let data = Arc::clone(&data);
        let handle = thread::spawn(move || {
            // Locking the mutex for exclusive access
            let mut guard = data.lock().unwrap();
            // Incrementing the counter and handling potential panics
            if *guard == 3 {
                println!("Thread reached 3, stopping.");
                return;
            }
            *guard += 1;
        });
        handles.push(handle);
    }

    // Waiting for all threads to complete
    for handle in handles {
        handle.join().unwrap();
    }

    // Attempting to lock the mutex again to print the final counter value
    let result = data.lock();
    match result {
        Ok(guard) => println!("Final Counter Value: {:?}", *guard),
        Err(poisoned) => {
            // Handling the poisoned state by recovering the data
            let guard = poisoned.into_inner();
            println!("Mutex is poisoned. Recovered Counter Value: {:?}", *guard);
        }
    }
}
```

## Overview:

- A Mutex-protected data structure wrapped in an Arc is created for shared ownership among threads.
- Threads are spawned to manipulate the mutex-protected data, each thread obtaining its own clone of the Arc and Mutex.
- Mutex locking ensures exclusive access to the data, with counter incrementation and panic handling implemented inside each thread.
- Once all threads complete their tasks, the final counter value is printed after attempting to lock the mutex again.
- In case of a poisoned mutex, the data is recovered and appropriately handled.

## Output:

```plaintext
Thread reached 3, stopping.
Thread reached 3, stopping.
Final Counter Value: 3
```

This illustrates the behavior of a program when dealing with both normal and poisoned states of the mutex.
