# Protecting Shared Data with Mutexes

# Mutex

When working with multi-threaded Rust programs, it’s crucial to protect shared data from concurrent access to avoid data races and unpredictable behavior. Rust offers the `Mutex` smart pointer for achieving this, providing exclusive, synchronized access to shared data, ensuring thread safety and data integrity.

## Example: Bank Simulation

Consider a bank simulation where multiple accounts are updated concurrently:

```rust
use std::sync::{Mutex, Arc};
use std::{thread, time::Duration};
use std::collections::HashMap;

struct Bank {
    accounts: Mutex<HashMap<String, f64>>,
}

impl Bank {
    fn new() -> Self {
        Bank {
            accounts: Mutex::new(HashMap::new()),
        }
    }

    fn deposit(&self, account: &str, amount: f64) {
        let mut accounts = self.accounts.lock().unwrap();
        let balance = accounts.entry(account.to_string()).or_insert(0.0);
        *balance += amount;
    }

    fn withdraw(&self, account: &str, amount: f64) {
        let mut accounts = self.accounts.lock().unwrap();
        let balance = accounts.entry(account.to_string()).or_insert(0.0);
        if *balance >= amount {
            *balance -= amount;
        } else {
            println!("Insufficient funds for account: {}", account);
        }
    }

    fn check_balance(&self, account: &str) -> f64 {
        let accounts = self.accounts.lock().unwrap();
        *accounts.get(account).unwrap_or(&0.0)
    }
}

fn main() {
    let bank = Arc::new(Bank::new());
    let mut handles = vec![];

    for i in 0..5 {
        let bank_clone = Arc::clone(&bank);
        let handle = thread::spawn(move || {
            let account = format!("Account{}", i);
            bank_clone.deposit(&account, 100.0);
            thread::sleep(Duration::from_millis(100)); // Simulate other work
            bank_clone.withdraw(&account, 30.0);
            let balance = bank_clone.check_balance(&account);
            println!("{} - Balance: {:.2}", account, balance);
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }
}
```

### Output:

```
Account0 - Balance: 70.00
Account1 - Balance: 70.00
Account2 - Balance: 70.00
Account3 - Balance: 70.00
Account4 - Balance: 70.00
```

## Explanation

In this example, a `Bank` struct contains a `Mutex<HashMap<String, f64>>` to store account balances. Multiple threads simulate bank transactions concurrently, ensuring data consistency and accuracy.

## Mutex Usage

Locking and unlocking a `Mutex` is crucial for ensuring exclusive access. Threads gain exclusive control over protected data when they lock a `Mutex`, automatically unlocking it once they finish their operation.

## Considerations

While `Mutex` guarantees exclusive modification access, excessive contention can impact performance. For read-heavy operations, consider using `RwLock<T>` to improve concurrency.
