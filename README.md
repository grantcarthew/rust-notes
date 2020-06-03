# Rust Notes

Rust language programming notes and references.

Table of Contents:

* [Frameworks and Libraries](#frameworks-and-libraries)
* [The Rust Programming Language](#the-rust-programming-language)
  * [Overview](#overview)
  * [Installing](#installing)
  * [Styles](#styles)
  * [Rust Files](#rust-files)
  * [Variables](#variables)
  * [Data Types](#data-types)
  * [Operators](#operators)
  * [Functions](#functions)
  * [Comments](#comments)
  * [Control Flow](#control-flow)
  * [Ownership](#ownership)
  * [Struct](#struct)

## Frameworks and Libraries

### Web Frameworks

* [State of Rust Web Frameworks (Server, DB)](https://dev.to/readredready/state-of-rust-web-frameworks-server-3g42)
* [Web Development Frameworks](https://www.arewewebyet.org/topics/frameworks/)

### Crates of Interest

* [SQLx](https://github.com/launchbadge/sqlx) - Database driver.
* [Actix](https://actix.rs/) - Rust's powerful actor system and most fun web framework.
* [Tide](https://blog.yoshuawuyts.com/tide/) - Fast and friendly HTTP server framework for async Rust.
* [Warp](https://github.com/seanmonstar/warp) - A super-easy, composable, web server framework for warp speeds.
* [Tower-Web](https://github.com/carllerche/tower-web) - A fast, boilerplate free, web framework for Rust.
* [Rocket](https://rocket.rs/) - Rocket is a web framework for Rust.
* [Rouille](https://github.com/tomaka/rouille) - Rust web micro-framework.

## The Rust Programming Language

The following notes have been taken from the excellent resource called [The Rust Programming Language](https://doc.rust-lang.org/book/title-page.html).

### Overview

Rust is an expression-based language:

* Statements are instructions that perform some action and do not return a value.
* Expressions evaluate to a resulting value and do not include ending semicolons.
* If you add a semicolon to the end of an expression, you turn it into a statement.

```rust
// Statement
let x = 5;
// Expression (no semicolon)
x + 1
// Statement with an expression
let y = x + 2;
```

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

Following is an example of a __Cargo.toml__ configuration file:

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

### Styles

Rust uses the following style points:

* Snake case for function and variable names (an_example_of_snake_case).
* Four spaces for tabs.
* Use `rustfmt` to standardize on code style.

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

Declare variables with either `let` or `const`:

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

You can make variables mutable by adding `mut` in front of the variable name:

```rust
let mut x = 5;
x = 6;
```

You can shadow variables by re-using the variable name with the let keyword.
Shadowing enables changing the type assigned to the variable name:

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
to the minimum of the values the type can hold:

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

Boolean `true` or `false` specified with the keyword `bool`:

```rust
let x: bool = true;
```

##### Character

The character type is four bytes in size and represents a Unicode Scalar Value.

Character literals are specified with single quotes:

```rust
let x = 'X';
```

#### Compound Types

Compound types can group multiple values into one type. Rust has two primitive compound types.

##### Tuple

A tuple is a general way of grouping together a number of values with a variety of types into one compound type.

Tuples are created by writing a comma-separated list of values inside parentheses:

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

Every element of an array must have the same type and they are of a fixed length:

```rust
let a = [1,2,3,4,5];
// With type and length
let a: [i32; 5] = [1,2,3,4,5];
// Initialize array with the number three and a length of five
let a = [3, 5];
```

Array elements can be accessed using indexing:

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

### Functions

Function definitions in Rust start with fn and have a set of parentheses after the function name.

Parameter or argument types are defined in the function signature.

Rust doesn’t care where you define your functions, only that they’re defined somewhere.

Function return types are declared after an arrow `->`.

The return value is the value of the final expression.

The `return` keyword can also return a value and exit the function early.

```rust
fn main() {
    let x = five()
    let y = plus_one(x)
    print_values(x, y);
}

fn print_values(x: i32, y: i32) {
    println!("The value of x is: {}", x);
    println!("The value of y is: {}", y);
}

fn five() -> i32 {
    5
}

fn plus_one(x: i32) -> i32 {
    x + 1
}
```

### Comments

```rust
// Comment with two back slashes.
// There is no block comment, just use multiple line comments.
Let x = 5; // Comments can be on the same line as statements or expressions.
```

### Control Flow

#### `if` Expressions

Condition in `if` expressions must evaluate to a boolean.

Blocks of code associated with the conditions in `if` expressions are sometimes called arms.

```rust
fn main() {
    let number = 6;

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
```

Because `if` is an expression it will return a value.

The arms of the `if` expression must return the same type.

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {}", number);

    // Error: if and else have incompatible types
    let unknown = if condition { 5 } else { "string" }
}
```

#### Loop

A continuous loop:

```rust
fn main() {
    loop {
        println!("again!");
    }
}
```

Returning values from loops:

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {}", result);
}
```

An example of a `while` loop:

```rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{}!", number);

        number -= 1;
    }

    println!("LIFTOFF!!!");
}
```

An example of a `for` loop:

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a.iter() {
        println!("the value is: {}", element);
    }
}
```

An example using a [Range](https://doc.rust-lang.org/std/ops/struct.Range.html) in a for loop:

```rust
fn main() {
    for number in (1..4).rev() {
        println!("{}!", number);
    }
    println!("LIFTOFF!!!");
}
```

### Ownership

Ownership enables Rust to make memory safety guarantees without needing a garbage collector.

Ownership rules:

* Each value in Rust has a variable that's called its __owner__.
* There can only be one owner at a time.
* When the owner goes out of scope, the value will be dropped.

Scope example:

```rust
// String pushed onto the stack
{                      // s is not valid here, it’s not yet declared
    let s = "hello";   // s is valid from this point forward

    // do stuff with s
}                      // this scope is now over, and s is no longer valid

// String allocated on the heap
{
    let s = String::from("hello"); // s is valid from this point forward

    // do stuff with s
}                                  // this scope is now over, and s is no
                                   // longer valid and "drop" is called
```

Simple values are stored on the stack and can easily be copied:

```rust
let x = 5;
let y = x;
```

Complex values are stored on the heap and, following rule number 2 above, can only have one owner:

```rust
let s1 = String::from("hello");
let s2 = s1; // This is a move, not a copy
// s1 is no longer valid and cannot be used
```

Complex types can be cloned:

```rust
let s1 = String::from("hello");
let s2 = s1.clone();
// s1 is still valid
```

As a general rule, any group of simple scalar values can be Copied.

* All the integer types, such as `u32`.
* The Boolean type, bool, with values `true` and `false`.
* All the floating point types, such as `f64`.
* The character type, `char`.
* Tuples, if they only contain types that are also Copy. For example, `(i32, i32)` is Copy, but `(i32, String)` is not.

Passing a variable to a function will move or copy, just as assignment does:

```rust
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // x would move into the function,
                                    // but i32 is Copy, so it’s okay to still
                                    // use x afterward

} // Here, x goes out of scope, then s. But because s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{}", some_string);
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{}", some_integer);
} // Here, some_integer goes out of scope. Nothing special happens.
```

Returning values from a function can also transfer ownership:

```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership moves its return
                                        // value into s1

    let s2 = String::from("hello");     // s2 comes into scope

    let s3 = takes_and_gives_back(s2);  // s2 is moved into
                                        // takes_and_gives_back, which also
                                        // moves its return value into s3
} // Here, s3 goes out of scope and is dropped. s2 goes out of scope but was
  // moved, so nothing happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String {             // gives_ownership will move its
                                             // return value into the function
                                             // that calls it

    let some_string = String::from("hello"); // some_string comes into scope

    some_string                              // some_string is returned and
                                             // moves out to the calling
                                             // function
}

// takes_and_gives_back will take a String and return one
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into
                                                      // scope

    a_string  // a_string is returned and moves out to the calling function
}
```

#### References and Borrowing

References allow you to refer to some value without taking ownership of it.

References are immutable by default.

Rules of references:

* At any given time, you can have either one mutable reference or any number of immutable references.
* References must always be valid.

The ampersands in the code below define references:

```rust
fn main() {
    let s1 = String::from("hello");

    // The `&s1` syntax lets us create a reference that refers to the value of s1 but does not own it.
    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize { // s is a reference to a String
    s.len()
} // Here, s goes out of scope. But because it does not have ownership of what
  // it refers to, nothing happens.
```

Using references as function parameters is called borrowing.

References can be changed to be mutable:

```rust
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

You can have only one mutable reference to a particular piece of data in a particular scope.

This restriction allows for mutation but in a very controlled fashion.

The benefit of having this restriction is that Rust can prevent data races at compile time. A data race is similar to a race condition and happens when these three behaviors occur:

* Two or more pointers access the same data at the same time.
* At least one of the pointers is being used to write to the data.
* There’s no mechanism being used to synchronize access to the data.

You can work around this restriction using curly brackets to create a new scope:

```rust
let mut s = String::from("hello");

{
    let r1 = &mut s;
} // r1 goes out of scope here, so we can make a new reference with no problems.

let r2 = &mut s;
```

You cannot have a mutable reference while we have an immutable one:

```rust
let mut s = String::from("hello");

let r1 = &s; // Immutable reference: no problem
let r2 = &s; // Immutable reference: no problem
let r3 = &mut s; // Mutable reference: BIG PROBLEM

println!("{}, {}, and {}", r1, r2, r3); // Mutable with immutable!!!
```

The following example works with a mix of immutable references and
a mutable reference:

```rust
let mut s = String::from("hello");

let r1 = &s; // no problem
let r2 = &s; // no problem
println!("{} and {}", r1, r2);
// r1 and r2 are no longer used after this point

let r3 = &mut s; // no problem
println!("{}", r3);
```

In Rust the compiler guarantees that references will never be dangling references:

```rust
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String { // dangle returns a reference to a String

    let s = String::from("hello"); // s is a new String

    &s // we return a reference to the String, s
} // Here, s goes out of scope, and is dropped. Its memory goes away.
  // Danger! Rust wont let you do this.
```

#### Slice Type

Slices let you reference a contiguous sequence of elements in a collection rather than the whole collection.

A string slice is a reference to part of a String, and it looks like this:

```rust
let s = String::from("hello world");

let hello = &s[..5]; // Range supports dropping the first index if starting at 0
let world = &s[6..11];
let remainder = &s[12..]; // Range supports dropping the last index if it is the length
let full = &s[..] // This is a slice of the entire string
```

The type that signifies “string slice” is written as `&str`:

```rust
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}
```

Better still, write the signature above using the following signature
because it allows us to use the same function on both &String values and &str values:

```rust
fn main() {
    let my_string = String::from("hello world");

    // first_word works on slices of `String`s
    let word = first_word(&my_string[..]);

    let my_string_literal = "hello world";

    // first_word works on slices of string literals
    let word = first_word(&my_string_literal[..]);

    // Because string literals *are* string slices already,
    // this works too, without the slice syntax!
    let word = first_word(my_string_literal);
}

// Note the change from &String to &str
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}
```

Array slices work the same way:

```rust
let a = [1, 2, 3, 4, 5];

let slice = &a[1..3];
```

### Struct

A struct, or structure, is a custom data type that lets you name and package together multiple related values that make up a meaningful group.

An example to define and use a struct:

```rust
// Define the User struct
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}

// Create an instance of the User struct
// Note that the entire instance must be mutable
let mut user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};

user1.email = String::from("anotheremail@example.com");

// As a constructor function
// Using the field init shorthand syntax
fn build_user(email: String, username: String) -> User {
    User {
        email,
        username,
        active: true,
        sign_in_count: 1,
    }
}

// Using the struct update syntax to copy user1 details
let user2 = User {
    email: String::from("another@example.com"),
    username: String::from("anotherusername567"),
    ..user1
};
```

Tuple structs have the added meaning that the struct name provides but don’t have names associated with their fields:

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

To print a struct use the `#[derive(Debug)]` annotation:

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!("rect1 is {:?}", rect1);  // One line
    println!("rect1 is {:#?}", rect1); // Multiple lines
}
```

A full example using a struct:

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        area(&rect1)
    );
}

fn area(rectangle: &Rectangle) -> u32 {
    rectangle.width * rectangle.height
}
```

Methods are different from functions in that they’re defined within the context of a struct, and their first parameter is always self, which represents the instance of the struct the method is being called on.

Defining a method and an associated function on a struct:

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

// Uses `self` reference
impl Rectangle {
    // Method using self
    fn area(&self) -> u32 {
        self.width * self.height
    }

    // Another method with an extra parameter
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }

    // Associated function (does not use self)
    fn square(size: u32) -> Rectangle {
        Rectangle {
            width: size,
            height: size,
        }
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };
    let rect2 = Rectangle {
        width: 10,
        height: 40,
    };
    let rect3 = Rectangle {
        width: 60,
        height: 45,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
    println!("Can rect1 hold rect2? {}", rect1.can_hold(&rect2));
    println!("Can rect1 hold rect3? {}", rect1.can_hold(&rect3));

}
```