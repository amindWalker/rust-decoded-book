# Dealing with Deadlocks and Errors

Deadlocks are a significant concern when dealing with locked data in Rust. A Mutex or Read/Write lock can lead to deadlocks if not managed properly. Even attempting to lock a mutex twice within the same thread can result in a deadlock, where the thread waits indefinitely for itself to release the lock.

Rust provides mechanisms to help avoid deadlocks, but it cannot entirely prevent them. It's essential to use these mechanisms correctly to mitigate the risk.

## Deadlocking Example

Consider this straightforward scenario where the program becomes entirely locked up due to attempting to acquire a Mutex twice:

```rust
use std::sync::Mutex;

fn main() {
    let my_shared = Mutex::new(0);

    let lock = my_shared.lock().unwrap();
    let lock = my_shared.lock().unwrap();
}
```

The program hangs indefinitely as it tries to obtain the Mutex twice within the same thread, resulting in a deadlock.

## Avoiding Deadlocks with Simple Error Handling

To prevent deadlocks, consider using `try_lock` instead of `lock`:

```rust
use std::sync::Mutex;

static MY_SHARED : Mutex<u32> = Mutex::new(0);

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

While `try_lock` doesn't wait and either succeeds or fails immediately, it's essential to avoid busy loops that could lead to excessive CPU usage.

## Explicitly Dropping Locks

Locks are automatically released when they go out of scope due to their implementation of `Drop`. However, you can also explicitly drop locks to release them early if needed:

```rust
let lock = MY_SHARED.lock().unwrap();
std::mem::drop(lock);
let lock = MY_SHARED.lock().unwrap();
```

Alternatively, you can use scopes to manage lock lifetimes more cleanly:

```rust
{
    let _lock = MY_SHARED.lock().unwrap();
    println!("I got the lock!");
}
let _lock = MY_SHARED.lock().unwrap();
println!("I got the lock again!");
```

## Mutex Poisoning

If a thread crashes while holding a lock, the lock becomes poisoned, preventing further access attempts. It's crucial to handle this situation gracefully:

```rust
use std::sync::Mutex;

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

In this example, the Mutex becomes **poisoned** after the thread crashes while holding the lock, leading to subsequent lock attempts failing.

## Recovering from Poisoning

While crashing might be the safest option in many cases, you can attempt to recover data from a poisoned Mutex:

```rust
let lock = DATA.lock().unwrap();

let recovered_data = lock.unwrap_or_else(|poisoned| {
    println!("Mutex was poisoned, recovering data...");
    poisoned.into_inner()
});
println!("Recovered data: {recovered_data:?}");
```

However, be cautious as there's no guarantee of data integrity after a thread crashes.

# Timed Locking

Timed locking with `RwLock` provides a mechanism to enforce time limits on read or write operations, preventing indefinite waits:

```rust
use std::sync::{RwLock, Arc};
use std::thread;
use std::time::{Duration, Instant};
use rand::Rng;

fn main() {
    let data = Arc::new(RwLock::new(0));
    let mut handles = vec![];

    for i in 0..5 {
        let data = Arc::clone(&data);
        let handle = thread::spawn(move || {
            let timeout_duration = Duration::from_secs(1);
            let start_time = Instant::now();
            let should_timeout = rand::thread_rng().gen::<bool>();

            loop {
                thread::sleep(Duration::from_millis(100));

                if start_time.elapsed() >= timeout_duration {
                    if should_timeout {
                        println!("Thread {} timed out.", i);
                        break;
                    } else {
                        if let Ok(data_guard) = data.read() {
                            println!("Thread {} read {} successfully.", i, *data_guard);
                        }
                    }
                }

                if !should_timeout && start_time.elapsed().as_secs() >= 2 {
                    if let Ok(_data_guard) = data.read() {
                        println!("Thread {} acquired the lock.", i);
                        break;
                    }
                }
            }
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }
}
```

## Here's a breakdown of how timed locking works:

- **Setting a Timeout Duration**: Before attempting to acquire a lock, a timeout duration is defined. This duration represents the maximum amount of time a thread is willing to wait to acquire the lock.
- **Acquiring the Lock with a Timeout**: The thread attempts to acquire the lock on the shared resource using methods like `try_lock` for `Mutex` or `try_read`/`try_write` for `RwLock`. These methods differ from their blocking counterparts (`lock`, `read`, `write`) in that they return immediately, either successfully acquiring the lock or indicating failure if the lock couldn't be obtained within the specified timeout.
- **Checking Timeout**: After attempting to acquire the lock, the thread checks if the elapsed time has exceeded the timeout duration. If the timeout is reached and the lock acquisition failed, appropriate actions can be taken, such as retrying later, logging an error, or taking alternative paths in the code.
- **Handling Lock Acquisition**: If the lock is successfully acquired within the timeout duration, the thread proceeds with the desired read or write operation on the shared resource. If the lock is not obtained due to a timeout, the thread can gracefully handle the situation based on the application's requirements.
- **Release the Lock**: Once the thread finishes its operations on the shared resource, it releases the lock to allow other threads to access it.

# Deadlock Avoidance

Deadlock avoidance is critical in concurrent programming, and `RwLock` provides tools to achieve it.

This example demonstrates a more complex handling of the poisoned mutexes:

```rust
use std::sync::{RwLock, Arc};
use std::thread;

fn main() {
    let data1 = Arc::new(RwLock::new(0));
    let data2 = Arc::new(RwLock::new(0));
    let mut handles = vec![];

    for _ in 0..5 {
        let data1 = Arc::clone(&data1);
        let data2 = Arc::clone(&data2);
        let handle = thread::spawn(move || {
            let mut data_guard1 = data1.write().unwrap();
            let mut data_guard2 = data2.write().unwrap();
            // Perform operations on data1 and data2
            *data_guard1 += 1;
            *data_guard2 += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
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

By ensuring a consistent order of lock acquisition this code effectively avoids deadlocks in concurrent scenarios.

# Implementing Resource Pooling with `RwLock`

Resource pooling in concurrent programming efficiently manages and shares a limited set of resources among multiple threads. This Rust example demonstrates how to implement resource pooling using `RwLock`, ensuring safe and synchronized access.

## Code Example

```rust
use std::sync::{RwLock, Arc};
use std::thread;

struct Resource {
    id: u32,
}

fn main() {
    const NUM_RESOURCES: u32 = 3;
    let resources: Vec<Arc<RwLock<Resource>>> = (0..NUM_RESOURCES)
        .map(|id| Arc::new(RwLock::new(Resource { id })))
        .collect();

    let mut handles = vec![];

    for i in 0..10 {
        let resources_clone: Vec<Arc<RwLock<Resource>>> = resources.clone();

        let handle = thread::spawn(move || {
            let idx = (i % NUM_RESOURCES) as usize;
            let resource = &resources_clone[idx];
            let data_guard = resource.read().unwrap();
            println!("Thread {} accessed Resource {}", i, data_guard.id);
        });

        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }
}
```

## Explanation

In this example, an array of resources is created, each represented by a `Resource` struct and wrapped in an `Arc<RwLock<Resource>>` to allow shared ownership across threads. The number of resources is defined by `NUM_RESOURCES`, and a loop initializes these resources with unique IDs.

The program then spawns ten threads, and each thread attempts to access a resource from the pool. The use of `RwLock` ensures that multiple threads can read a resource concurrently without issues, but only one thread can hold a write lock for exclusive modification. Thus, the resource pool is effectively utilized by the threads, preventing data races and ensuring safe access.

To distribute thread access evenly among the available resources, modulo arithmetic `(i % NUM_RESOURCES)` is used, preventing conflicts on the same resource by multiple threads. This ensures efficient resource sharing, with the program producing an output displaying the resource IDs accessed by each thread.
