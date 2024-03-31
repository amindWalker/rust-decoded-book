# Exploring Essential Rust Concepts.
Let's dive into the basics of Rust programming.

## Integer Types
- Rust uses `i32` integers by default if you didn't specify one.

| Length | Signed | Unsigned |
| --- | --- | --- |
| 8 bits | i8 | u8 |
| 16 bits | i16 | u16 |
| 32 bits | i32 | u32 |
| 64 bits | i64 | u64 |
| 128 bits | i128 | u128 |
| pointer-sized | isize | usize |

## Floating Point
- `f32`: single precision (32-bit)
- `f64`: double precision (64-bit)
- `f128`: 128-bit
```rust
fn main() {
    let x = 2.0;      // f64
    let y = 1.0f32;   // f32
}

```

## Numerical Operations

```rust
fn main() {
    let sum = 5 + 10;
    let difference = 10 - 3;
    let mult = 2 * 8;
    let div = 2.4 / 3.5;
    let int_div = 10 / 3;   // 3
    let remainder = 20 % 3;
}

```

- Overflow/underflow checked in debug builds
- Wrapping occurs in release builds for efficiency
- Mixing types not allowed

```rust
fn main() {
    let invalid_div = 2.4 / 5;         // Error!
    let invalid_add = 20u32 + 40u64;   // Error!
}

```

## Type Annotation
- Rust enforces strong and strict typing.
- Type inference often eliminates the need for explicit type declaration.
- Explicit types can be useful in certain contexts (like stating clear intention of using certai type).

```rust
fn main() {
    let x: i32 = 20;
    //     ^^^ Type annotation
}
```
## Booleans and Operations
```rust
fn main() {
    let yes: bool = true;
    let no: bool = false;
    let not = !no; # Inverted to `true` (negation)
    let and = yes && no;
    let or = yes || no;
    let xor = yes ^ no;
}
```

## Comparison Operators

```rust
fn main() {
    let x = 10;
    let y = 20;
    x < y;   // true
    x > y;   // false
    x <= y;  // true
    x >= y;  // false
    x == y;  // false
    x != y;  // true
}

```

- You can't compare different types
```rust
fn main() {
    3.0 < 20;       // Invalid
    30u64 > 20i32;  // Invalid
}
```

## Characters
In Rust, characters are valid `UTF-8` Unicode values. Which means you can use a wide range of symbols.

```rust
fn main() {
    let ch: char = 'c'; # Single quotes are used for defined single characters
    let cat = '😻'; # This is a valid value in Rust
}
```

## Strings

- `String`s are `UTF-8` encoded because they are a collection of many single `characters`.
- `Strings` are Heap-allocated (we'll see what this means later).

```rust
let s1 = String::from("Hello, 🌍!");
//       ^^^^^^ Owned, heap-allocated string

```

## Tuples

- Group multiple values into a compound type.
- It has a fixed size.
- Multiple different types can be stored per Tuple.

```rust
fn main() {
    let tup: (i32, f32, char) = (1, 2.0, 'a');
}

```
```rust
fn main() {
    let tup = (1, 2.0, 'Z');
    let (a, b, c) = tup; # Destructuring a tuple. Each value on the left will be assigned with respective values on the right.
    println!("({}, {}, {})", a, b, c);

    let another_tuple = (true, 42);
    println!("{}", another_tuple.1);
}
```

**Arrays**
- Fixed length at compile time
- Collection of same-type values
- Access elements using square brackets: `[]`
- Rust checks array bounds

```rust
fn main() {
    let arr: [i32; 3] = [1, 2, 3];
    println!("{}", arr[0]);
    let [a, b, c] = arr;
    println!("[{}, {}, {}]", a, b, c);
}
```

---

**Control Flow**
In Rust, you can use control flow conditionals inside variables and a special keyword `loop`, for infinite loops.
```rust
fn main() {
    let is_true = true;
    let conditional_result = if is_true { return "Good!"; } else { return "Bad!"; };
    let mut x = 0;
    loop {
        if x < 5 {
            println!("x: {}", x);
            x += 1;
        } else {
            break;
        }
    }

    let mut y = 5;
    while y > 0 {
        y -= 1;
        println!("y: {}", x);
    }

    for i in [1, 2, 3, 4, 5] {
        println!("i: {}", i);
    }
}
```

## Variables and Mutability
Variables in Rust are declared using `let`. By default, they are `immutable`, meaning once assigned, their value cannot be changed. However, you can make them mutable by adding `mut` before the variable name or use `shadowing`.

```rust
fn main() {
    let mut n = 5; // mutable variable
    n += 1;
    println!("{n}");
}
```

In Rust, when you re-declare a variable with the same name that's called `shadowing`. This effectively replace the previous value for a given scope.

```rust
fn main() {
    let n = 5;
    let n = n + 1; // shadowing
    println!("{n}");
}
```

## Scope and Return
Rust uses scopes defined by `{}`. Variables declared inside a scope (the `{}`) are **only accessible within that scope**.

```rust
fn main() {
    let n = 5;
    {
        let n = 6; // inner scope
        println!("{n}");
    }
    println!("{n}");
}
```

The last line of a block or function is the return value. Even if a block doesn't explicitly `return` anything, it implicitly returns the **unit type**: `()`.

```rust
fn main() {
    let n = {
        let mut accumulator = 0;
        for i in 1..10 {
            accumulator += i;
        }
        accumulator // return value
    };
    println!("{n}");
}
```
## Functions
Functions in Rust can take parameters and return values. You can use explicit `return` or let the last expression be the return value.

- Function boundaries are annotated with types.
- Function returning nothing has type *unit* (`()`).
- Function body comprises statements ending with an expression.

## Statements

- Instructions that perform actions without returning a value.
- Includes definitions and `let` statements.
- Almost everything else is an expression.
**Example statements**

```rust
fn my_fun() {
    println!("{}", 5);
}

let x = 10;

return 42;

let x = (let y = 10); // Invalid

```

```rust
fn double(n: i32) -> i32 {
    n * 2 # Implicit return
}

fn triple(n: i32) -> i32 {
    return n * 3; # Explicit return
}


fn main() {
    let n = double(5);
    println!("Double: {n}");
    let n = triple(5); # Shadowing a previous assignment
    println!("Triple: {n}");
}
```

**Expressions**

- Evaluate to a resulting value
- Predominant in Rust code
- Includes control flow and scoping braces
- Semicolon turns expression into statement

```rust
fn main() {
    let y = {
        let x = 3;
        x + 1
    };
    println!("{}", y); // 4
}
```

## Expressions - Control Flow
- Control flow expressions as statements don't need semicolons if they return *unit* (`()`).
- Block/function endings require expressions with correct types.

```rust
fn main() {
    let y = 11;
    // if as an expression
    let x = if y < 10 {
        42
    } else {
        24
    };

    // if as a statement

    if x == 42 {
        println!("Foo");
    } else {
        println!("Bar");
    }
}

```

## Move Semantics
Rust has a special `move` keyword that moves values by default. When you pass a variable to a function, it's **moved**, meaning you can't use it afterwards unless the function returns it (or passes it by `reference`, as we'll see).

```rust
fn greet(s: String) -> String {
    println!("Hello, {s}");
    s
}

fn main() {
    let mut name = "Hello".to_string();
    name = greet(name);
    name = name + " World"; // valid
}
```

## Borrowing and References

To pass a variable to a function without moving it, you **borrow** it using `&`.

```rust
fn greet(s: &String) {
    println!("Hello, {s}");
}

fn main() {
    let mut name = "Hello".to_string();
    greet(&name);
    name = name + " World";
}
```
> [!TIP]
> **If you want to learn more in-depth about Rust Types and how Rust manages memory take a look at: [Rust Memory Layout and Types](https://github.com/amindWalker/Rust-Layout-and-Types)**
