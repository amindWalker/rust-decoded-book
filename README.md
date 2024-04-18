![Rust-Decoded-Cover](https://github.com/amindWalker/rust-decoded-book/assets/66398400/7ca9d4d5-3f4e-46f2-9a8d-bf1e6a40c9bf)

<div align='center'>
  
  "**[The Book](https://doc.rust-lang.org/book/)**" from the `Rust` documentation decoded with concise explanations of advanced **Systems Development** concepts. Updated to Rust 2024 Edition and includes many exercises with a deep dive into **Web Development and WebAssembly**.

</div>

# Part I: First Steps
- [Introduction to Rust Programming](src/part_1/introduction.md)
- [Setting Up Rust](src/part_1/setting_up_rust.md)
- [IDE Configuration for Rust Development](src/part_1/ide_configuration.md)
- [Understanding Workspaces and Cargo](src/part_1/understanding_workspaces_and_cargo.md)
- [Exploring Essential Rust Concepts](src/part_1/essential_rust_concepts.md)
- [Handling Console Input](src/part_1/handling_inputs.md)

# Part II: Essential Rust Building and Data Structures
## Rust Building Tools
- [API Guidelines](src/part_2/api_guidelines.md)
- [Design Patterns](src/part_2/design_patterns.md)
- [Utilizing Macros for Repetitive Tasks](src/part_2/macros.md)
- [Testing and Test Driven Development](src/part_2/tests_tdd.md)

## Applying Concepts using Data Structures
- [Building Your Own Rust Library](src/part_2/building_a_lib.md)
- [Understanding and Implementing Enumerations](src/part_2/enums.md)
- [Working with Structures](src/part_2/structs.md)
- [Managing Data with Vectors](src/part_2/vectors.md)
- [Utilizing HashMaps for Key-Value Data Storage](src/part_2/hashmaps.md)
- [Serialization and Deserialization](src/part_2/serde.md)
- [Secure Cipher Methods](src/part_2/cipher_hashing.md)
- [Making a Command-Line Interface (CLI) Application](src/part_2/cli_app.md)
- [Making a User Administrative Roles Application](src/part_2/user_admin_app.md)

# Part III: Understanding Operating Systems and Concurrent Programming (Multitasking)
- [Introduction to Concurrent Programming](src/part_3/concurrent_intro.md)
- [Fundamentals of System Threads](src/part_3/system_threads.md)

## Thread Communication with CPU Synchronization
- [Understanding Closures and Move Synchronization](src/part_3/thread_sync.md)
- [Sharing Data with Scoped Threads](src/part_3/scoped_threads.md)
- [Sharing Data with Atomics](src/part_3/atomics.md)
- [Protecting Shared Data with Mutexes](src/part_3/mutex.md)
- [Implementing Read/Write Locks](src/part_3/rw_locks.md)
- [Dealing with Deadlocks and Errors](src/part_3/deadlocks_and_error_handling.md)

# Solving Concurrent Locks in Data Structures
- [Lock and Lock-Free Structures](src/part_3/lock_free.md)
- [Efficient Thread Parking](src/part_3/thread_parking.md)

# Thread Communication via Channels
- [Introduction to Channels](src/part_3/channels.md)
- [Understanding Channels and Ownership](src/part_3/channels_ownership.md)
- [Sending Functions Across Threads](src/part_3/function_threads.md)

# Advanced Thread Management
- [Setting Thread Priorities](src/part_3/thread_priority.md)
- [Building a Work Queue with Thread Pools](src/part_3/thread_pools.md)
- [Simplifying Concurrent Workloads with Rayon](src/part_3/concurrent_rayon.md)
- [Leveraging Scopes and Thread Pools with Rayon](src/part_3/rayon_scopes_and_pools.md)

# Part IV: Advanced Concepts, Memory and Pointers
## Introduction
- [Why Memory Management Matters](src/part_4/memory_mgmt.md)

## Memory Management Basics
- [Low-Level Memory Management](src/part_4/low_lvl_memory.md)
- [The Use of `unsafe` in Rust](src/part_4/rust_unsafe.md)
- [The Drop Trait and RAII (Resource Acquisition is Initialization)](src/part_4/drop_and_raii.md)

## Advanced Concepts
- [Memory Efficiency, Allocators, and Arenas](src/part_4/memory_concepts.md)
- [Manipulation of Bits and Bytes](src/part_4/bit_manipulation.md)

## Reference Counting and Lifetimes
- [Understanding Reference Counting](src/part_4/references.md)
- [The Lifetimes Concept](src/part_4/lifetimes.md)

## Traits and Generics
- [Shared Behavior with Traits](src/part_4/traits.md)
- [Don't Repeat Yourself with Generics](src/part_4/generics.md)

## Iteration on Collections
- [Iterators: Beyond the Basics](src/part_4/iterators.md)
- [Applying Lifetimes and Pointers in Algorithms](src/part_4/applying_lifetimes.md)
- [Calling Other Languages with Rust](src/part_4/ffi.md)

# Part V: Asynchronous Programming with Rust Async/Await
## First Steps with Async/Await
- [Introduction to Async/Await](src/part_5/async_await_overview.md)
- [Executors and Async Runtimes](src/part_5/executors_and_async_runtimes.md)

## Deep Dive in the Tokio Library
- [Tokio Overview](src/part_5/tokio_overview.md)
- [Tokio Futures](src/part_5/tokio_futures.md)
- [Understanding Blocking](src/part_5/blocking_tasks.md)
- [Tokio Unit Tests](src/part_5/tokio_testing.md)

## Advanced Concepts
- [Selecting Futures](src/part_5/selecting_futures.md)
- [Pinning](src/part_5/pinning.md)
- [Tokio Tracing](src/part_5/tokio_tracing.md)
- [Handling Errors](src/part_5/handling_errors.md)

## Input/Output Operations
- [File I/O](src/part_5/file_io.md)
- [Basic Network I/O](src/part_5/basic_network_io.md)
- [Async Channels (Tokio)](src/part_5/async_channels.md)
- [Shared State (Tokio)](src/part_5/shared_state.md)

## Working with External Tools
- [Working with Databases](src/part_5/working_with_databases.md)
- [Axum - The Web Framework from the Tokio Group](src/part_5/axum_web_framework.md)

## Let's Build a Meme Server
- [Building a Meme Server](src/part_5/meme_server.md)
