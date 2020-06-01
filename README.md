# Rust Notes

Rust language programming notes and references.

## Web Frameworks

* [State of Rust Web Frameworks (Server, DB) ](https://dev.to/readredready/state-of-rust-web-frameworks-server-3g42)
* [Web Development Frameworks](https://www.arewewebyet.org/topics/frameworks/)

## Crates of Interest

* [SQLx](https://github.com/launchbadge/sqlx) - Database driver.
* [Actix](https://actix.rs/) - Rust's powerful actor system and most fun web framework.
* [Tide](https://blog.yoshuawuyts.com/tide/) - Fast and friendly HTTP server framework for async Rust.
* [Warp](https://github.com/seanmonstar/warp) - A super-easy, composable, web server framework for warp speeds.
* [Tower-Web](https://github.com/carllerche/tower-web) - A fast, boilerplate free, web framework for Rust.
* [Rocket](https://rocket.rs/) - Rocket is a web framework for Rust.
* [Rouille](https://github.com/tomaka/rouille) - Rust web micro-framework.

## The Rust Programming Language

The following notes have been taken from the excellent resource called [The Rust Programming Language](https://doc.rust-lang.org/book/title-page.html).

### Installing

Uses a command called `rustup`:

```bash
curl https://sh.rustup.rs -sSf | sh
```

To upgrade:

```bash
rustup update
```

To compile a source code file:

```bash
rustc main.rs
```

To create a new project:

```bash
cargo new project_name
```
To configure a project:

*Cargo.toml*
```toml
[package]
name = "hello_cargo"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]
edition = "2018"

[dependencies]
rand = "0.5.5"
```

To build a project:

```bash
cargo build
```

To run a project:

```bash
cargo run
```

To see if your code will compile:

```bash
cargo check
```

To build a release version of your application:

```bash
cargo build --release
```

### Rust Files

Following is the guessing game from the [The Rust Programming Language](https://doc.rust-lang.org/book/title-page.html)
for reference:

```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1, 101);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {}", guess);

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

Bring types into scope:

```rust
use std::io;
use std::cmp::Ordering;
use rand::Rng;
```

Rust requires a `main` entry point:

```rust
fn main() {

}
```

Using an external type:

```rust
use rand::Rng;

fn main() {
    let random_number = rand::thread_rng().gen_range(1, 101);
    println!("{}", random_number)
}
```

### Variables

By default variables are immutable.

Declare variables with either `let` or `const`

```rust
let x = 4;
const MAX_SIZE: u32 = 100_000;
```

Constants or `const` variables:

* Are always immutable.
* Must be annotated.
* Can't use `mut` with them (see next section).
* Can be declared in any scope including global.
* Can only be set by a constant expression (not computed at runtime).

You can make variables mutable by adding `mut` in front of the variable name.

```rust
let mut x = 5;
x = 6;
```

You can shadow variables by re-using the variable name with the let keyword.
Shadowing enables changing the type assigned to the variable name.

```rust
let x = 5;
let x = format!("{}", 5);
```

### Data Types

The compiler can usually infer what type we want to use based on the value and how we use it.

#### Scalar Types

A scalar type represents a single value. Rust supports four primary scalar types.

##### Integers

Rust performs two’s complement wrapping. Values greater than the maximum value the type can hold “wrap around”
to the minimum of the values the type can hold.

```rust
let x: i32 = -1000;
```

Sizes:

|Length|Signed|Unsigned|
|-|-|-|
|8-bit|i8|u8|
|16-bit|i16|u16|
|32-bit|i32|u32|
|64-bit|i64|u64|
|128-bit|i128|u128|
|CPU Arch|isize|usize|

Notation:

|Number Literals|Example|
|-|-|
|Decimal|56_989|
|Hex|0xff|
|Octal|0o77|
|Binary|0b1111_0000|
|Byte (u8 only)|b'A'|

##### Floating-point Numbers

Rust has two primitive types for floating-point numbers:

* `f64` which is the default.
* `f32`

```rust
let x: f32 = 199.99;
```

##### Boolean

Boolean `true` or `false` specified with the keyword `bool`.

```rust
let x: bool = true;
```

##### Character

The character type is four bytes in size and represents a Unicode Scalar Value.

Character literals are specified with single quotes.

```rust
let x = 'X';
```

#### Compound Types

Compound types can group multiple values into one type. Rust has two primitive compound types.

##### Tuple

A tuple is a general way of grouping together a number of values with a variety of types into one compound type.

Tuples are created by writing a comma-separated list of values inside parentheses.

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);
// Pattern matching
let (x, y, z) = tup;
// Period access
let x = tup.0;
let y = tup.1;
let z = tup.2;
```

##### Array

Every element of an array must have the same type and they are of a fixed length.

```rust
let a = [1,2,3,4,5];
// With type and length
let a: [i32; 5] = [1,2,3,4,5];
// Initialize array with the number three and a length of five
let a = [3, 5];
```

Array elements can be accessed using indexing.

```rust
let a = [1,2,3,4,5];
let first = a[0];
let second = a[1];
```

If you access an element in an array past its length, Rust will panic.

### Operators

The following numeric operators are supported:

|Operation|Symbol|Example|
|-|-|-|
|Addition|`+`|`5 + 10`|
|Subtraction|`-`|`95.5 - 4.3`|
|Multiplication|`*`|`4 * 30`|
|Division|`/`|`56.7 / 32.2`|
|Remainder|`%`|`43 % 5`|
