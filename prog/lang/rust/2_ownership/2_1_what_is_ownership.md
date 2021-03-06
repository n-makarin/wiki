# What is ownership

/ [home](/README.md) / [prog](/prog/README.md) / [lang](/prog/lang/README.md) / [rust](/prog/lang/rust/README.md) / [2. Ownership](/prog/lang/rust/2_ownership/README.md) / [2.1. What is ownership](/prog/lang/rust/2_ownership/2_1_what_is_ownership.md)

## Table of Contents

- [What is ownership](#what-is-ownership)
  - [Table of Contents](#table-of-contents)
  - [Main rules](#main-rules)
  - [The Stack and the Heap](#the-stack-and-the-heap)
  - [Variable Scope](#variable-scope)
  - [The String Type](#the-string-type)
  - [Memory and Allocation](#memory-and-allocation)
  - [Move](#move)
  - [Ways Variables and Data Interact: Clone](#ways-variables-and-data-interact-clone)
  - [Stack-Only Data: Copy](#stack-only-data-copy)
  - [Ownership and Functions](#ownership-and-functions)
  - [Return Values and Scope](#return-values-and-scope)

## Main rules

- Each value in Rust has a variable that’s called its owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

## The Stack and the Heap

All simple values with a known fixed values are pushed onto the stack.

All sdk and created types are pushed onto the heap.

## Variable Scope

A scope is the range within a program for which an item is valid.

The value of value will be dropped when the owner goes out of scope if it's value wasn't moved before.

## The String Type

```rust
    // owned String, heap allocated
    let mut s = String::from("hello");

    //  reference to string literal hardcoded in binary, 'static &str
    let x = "Hi";
```

```rust
fn main() {
    let mut s = String::from("hello");

    s.push_str(", world!");

    println!("{}", s); // This will print `hello, world!`
}
```

## Memory and Allocation

- The memory must be requested from the memory allocator at runtime.

  Starts when we call String::from
  
- We need a way of returning this memory to the allocator when we’re done with our String.

**allocate** - allocate memory

**free** - free memory

**drop** - it’s where the author of String can put the code to return the memory. Rust calls drop automatically at the closing curly bracket.

## Move

There is no error cause of simple values doesn't pushing to the heap.

```rust
fn main() {
    let x = 5;
    let y = x;

    println!("x = {}, y = {}", x, y);
}
```

Representation in memory of a String holding the value "hello" bound to s1.

A String is made up of three parts, shown on the left: a pointer to the memory that holds the contents of the string, a length, and a capacity.

This group of data is stored on the stack.

On the right is the memory on the heap that holds the contents.

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;
}
```
<!-- markdownlint-disable MD033 -->
<img src="/img/prog/lang/rust/string_in_heap.svg" width="375" alt="representation in memory">

**ptr** - pointer refers to a data in heap

**length** - is how much memory, in bytes, the contents of the String is currently using.

**capacity** - the total amount of memory, in bytes, that the String has received from the allocator

When we assign s1 to s2, the String data is copied, meaning we copy the pointer, the length, and the capacity that are on the stack.

We do not copy the data on the heap that the pointer refers to.
<!-- markdownlint-disable MD033 -->
<img src="/img/prog/lang/rust/move_string.svg" width="375" alt="move string">

Both data pointers pointing to the same location.

This is a problem: when s2 and s1 go out of scope, they will both try to free the same memory.

🔹 Instead of trying to copy the allocated memory, Rust considers s1 to no longer be valid and, therefore, Rust doesn’t need to free anything when s1 goes out of scope.

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;

    println!("{}, world!", s1);
    // error: could not compile `ownership`.
}
```

## Ways Variables and Data Interact: Clone

If we do want to deeply copy the heap data of the String, not just the stack data, we can use a common method called clone.

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {}, s2 = {}", s1, s2);
}
```

## Stack-Only Data: Copy

Types whose values can be duplicated simply by copying bits.

By default, variable bindings have 'move semantics.

However, if a type implements Copy, it instead has 'copy semantics':

```rust
// We can derive a `Copy` implementation. `Clone` is also required, as it's
// a supertrait of `Copy`.
#[derive(Debug, Copy, Clone)]
struct Foo;

let x = Foo;

let y = x;

// `y` is a copy of `x`

println!("{:?}", x); // A-OK!
```

Here are some of the types that are `Copy`:

- All the integer types, such as u32.
- The Boolean type, bool, with values true and false.
- All the floating point types, such as f64.
- The character type, char.
- Tuples, if they only contain types that are also Copy. For example, (i32, i32) is Copy, but (i32, String) is not.

## Ownership and Functions

Passing a variable to a function will move or copy, just as assignment does.

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

## Return Values and Scope

Returning values can also transfer ownership.

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

It’s possible to return multiple values using a tuple:

```rust
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{}' is {}.", s2, len);
}

fn calculate_length(s: String) -> (String, usize) {
    let length = s.len(); // len() returns the length of a String

    (s, length)
}
```

But this is too much ceremony and a lot of work for a concept that should be common.

Luckily for us, Rust has a feature for this concept, called references.
🦀
