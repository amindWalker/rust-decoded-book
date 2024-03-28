# Part I: First Steps
- [Introduction to Rust Programming](./part_1/introduction.md)
- [Setting Up Rust](./part_1/setting_up_rust.md)
- [IDE Configuration for Rust Development](./part_1/ide_configuration.md)
- [Understanding Workspaces and Cargo](./part_1/understanding_workspaces_and_cargo.md)
- [Exploring Essential Rust Concepts](./part_1/essential_rust_concepts.md)
- [Handling Console Input](./part_1/handling_inputs.md)

# Part II: Essential Rust Building and Data Structures
## Rust Building Tools
- [API Guidelines](./part_2/api_guidelines.md)
- [Design Patterns](./part_2/design_patterns.md)
- [Utilizing Macros for Repetitive Tasks](./part_2/macros.md)
- [Testing and Test Driven Development](./part_2/tests_tdd.md)

## Applying Concepts using Data Structures
- [Building Your Own Rust Library](./part_2/building_a_lib.md)
- [Understanding and Implementing Enumerations](./part_2/enums.md)
- [Working with Structures](./part_2/structs.md)
- [Managing Data with Vectors](./part_2/vectors.md)
- [Utilizing HashMaps for Key-Value Data Storage](./part_2/hashmaps.md)
- [Serialization and Deserialization](./part_2/serde.md)
- [Secure Cipher Methods](./part_2/cipher_hashing.md)
- [Making a Command-Line Interface (CLI) Application](./part_2/cli_app.md)
- [Making a User Administrative Roles Application](./part_2/user_admin_app.md)

# Part III: Understanding Operating Systems and Concurrent Programming (Multitasking)
- [Introduction to Concurrent Programming](./part_3/concurrent_intro.md)
- [Fundamentals of System Threads](./part_3/system_threads.md)
- [Creating and Managing Threads](./part_3/managing_threads.md)

## Thread Communication with CPU Synchronization
- [Understanding Closures and Move Synchronization](./part_3/thread_sync.md)
- [Sharing Data with Scoped Threads](./part_3/scoped_threads.md)
- [Sharing Data with Atomics](./part_3/atomics.md)
- [Protecting Shared Data with Mutexes](./part_3/mutex.md)
- [Implementing Read/Write Locks](./part_3/rw_locks.md)
- [Dealing with Deadlocks and Errors](./part_3/deadlocks_and_error_handling.md)

# Solving Concurrent Locks in Data Structures
- [Lock and Lock-Free Structures](./part_3/lock_free.md)
- [Efficient Thread Parking](./part_3/thread_parking.md)

# Thread Communication via Channels
- [Introduction to Channels](./part_3/channels.md)
- [Understanding Channels and Ownership](./part_3/channels_ownership.md)
- [Sending Functions Across Threads](./part_3/function_threads.md)

# Advanced Thread Management
- [Managing CPU/Core Affinity](./part_3/core_affinity.md)
- [Setting Thread Priorities](./part_3/thread_priority.md)
- [Building a Work Queue with Thread Pools](./part_3/thread_pools.md)
- [Simplifying Concurrent Workloads with Rayon](./part_3/concurrent_rayon.md)
- [Leveraging Scopes and Thread Pools with Rayon](./part_3/rayon_scopes_and_pools.md)

# Part IV: Advanced Concepts, Memory and Pointers
## Introduction
- [Why Memory Management Matters](./part_4/memory_mgmt.md)

## Memory Management Basics
- [Low-Level Memory Management](./part_4/low_lvl_memory.md)
- [The Use of `unsafe` in Rust](./part_4/rust_unsafe.md)
- [The Drop Trait and RAII (Resource Acquisition is Initialization)](./part_4/drop_and_raii.md)

## Advanced Concepts
- [Memory Efficiency, Allocators, and Arenas](./part_4/memory_concepts.md)
- [Manipulation of Bits and Bytes](./part_4/bit_manipulation.md)

## Reference Counting and Lifetimes
- [Understanding Reference Counting](./part_4/references.md)
- [The Lifetimes Concept](./part_4/lifetimes.md)

## Traits and Generics
- [Shared Behavior with Traits](./part_4/traits.md)
- [Don't Repeat Yourself with Generics](./part_4/generics.md)

## Iteration on Collections
- [Iterators: Beyond the Basics](./part_4/iterators.md)
- [Applying Lifetimes and Pointers in Algorithms](./part_4/applying_lifetimes.md)
- [Calling Other Languages with Rust](./part_4/ffi.md)

# Part V: Asynchronous Programming with Rust Async/Await
## First Steps with Async/Await
- [Introduction to Async/Await](./part_5/async_await_overview.md)
- [Executors and Async Runtimes](./part_5/executors_and_async_runtimes.md)

## Deep Dive in the Tokio Library
- [Tokio Overview](./part_5/tokio_overview.md)
- [Tokio Futures](./part_5/tokio_futures.md)
- [Understanding Blocking](./part_5/blocking_tasks.md)
- [Tokio Unit Tests](./part_5/tokio_testing.md)

## Advanced Concepts
- [Selecting Futures](./part_5/selecting_futures.md)
- [Pinning](./part_5/pinning.md)
- [Tokio Tracing](./part_5/tokio_tracing.md)
- [Handling Errors](./part_5/handling_errors.md)

## Input/Output Operations
- [File I/O](./part_5/file_io.md)
- [Basic Network I/O](./part_5/basic_network_io.md)
- [Async Channels (Tokio)](./part_5/async_channels.md)
- [Shared State (Tokio)](./part_5/shared_state.md)

## Working with External Tools
- [Working with Databases](./part_5/working_with_databases.md)
- [Axum - The Web Framework from the Tokio Group](./part_5/axum_web_framework.md)

## Let's Build a Meme Server
- [Building a Meme Server](./part_5/meme_server.md)






