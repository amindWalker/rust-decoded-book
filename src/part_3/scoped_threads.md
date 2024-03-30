# Sharing Data with Scoped Threads

# Dividing Workloads with Threads

In Rust, leveraging threads to distribute a workload efficiently is a powerful technique. Let's explore how we can achieve this.

Example:
```rust
use std::thread;

fn main() {
    const N_THREADS: usize = 8;

    let to_add: Vec<u32> = (0..5000).collect();
    let chunks = to_add.chunks(N_THREADS);
    let sum = thread::scope(|s| {
        let mut thread_handles = Vec::new();

        for chunk in chunks {
            let thread_handle = s.spawn(move || {
                let mut sum = 0;
                for i in chunk {
                    sum += i;
                }
                sum
            });
            thread_handles.push(thread_handle);
        }

        thread_handles
            .into_iter()
            .map(|handle| handle.join().unwrap())
            .sum::<u32>()
    });
    println!("Sum is {sum}");
}
```

## Defining Thread Count

We start by defining the number of threads we want to utilize using a constant. This provides flexibility for adjusting the thread count as needed in the future.

```rust
const N_THREADS: usize = 8;
```

## Preparing Data

Next, we create a vector of numbers to process. Using the `collect` function, we efficiently build the vector from a range.

```rust
let to_add: Vec<u32> = (0..5000).collect();
```

## Partitioning the Workload

To divide the workload evenly among threads, we utilize the `chunks` function, which partitions the vector into manageable chunks.

```rust
let chunks = to_add.chunks(N_THREADS);
```

## Handling Ownership and Threads

We encounter a challenge when passing data to threads due to ownership constraints. To overcome this, we create owned copies of each chunk using `to_owned`.

```rust
let my_chunk = chunk.to_owned();
```

## Concurrent Computation

Each thread independently calculates the sum of its assigned chunk. We spawn threads and store their handles for later synchronization.

```rust
let handle = std::thread::spawn(move || {
    // Thread computation
});
thread_handles.push(handle);
```

## Synchronization and Result Aggregation

We synchronize threads and aggregate their results to obtain the final sum.

```rust
let mut sum = 0;
for handle in thread_handles {
    sum += handle.join().unwrap();
}
```

# Scoped Threads for Simplified Management
## Benefits of Scoped Threads

Scoped threads (the use of `scope`) function similarly to regular threads but offer a crucial advantage: termination guarantees within the scope. This ensures that borrowed data from the parent scope remains safe, mitigating common bugs and issues prevalent in other languages.

## Simplified Workload Distribution

Scoped threads excel in scenarios where distributing a workload among calculation threads is necessary. By providing a structured approach to thread management, they streamline the process of combining results efficiently.

With scoped threads, Rust empowers developers to manage thread lifetimes effectively, fostering safer and more efficient concurrent programming practices.

```rust
let sum = thread::scope(|s| {
    // Thread spawning within scope
});
```

## Safe Borrowing with Thread Scopes

Scoped threads guarantee termination within the scope, allowing safe borrowing of data from the parent scope.

```rust
let sum = thread::scope(|&s| {
    // Thread computation with borrowed data
});
```
