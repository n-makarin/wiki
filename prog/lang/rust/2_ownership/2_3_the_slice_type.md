# The Slice Type

/ [home](/README.md) / [prog](/prog/README.md) / [lang](/prog/lang/README.md) / [rust](/prog/lang/rust/README.md) / [2. Ownership](/prog/lang/rust/2_ownership/README.md) / [2.3. The Slice Type](/prog/lang/rust/2_ownership/2_3_the_slice_type.md)

## Table of Contents

- [The Slice Type](#the-slice-type)
  - [Table of Contents](#table-of-contents)
  - [String Slices](#string-slices)
  - [String Literals Are Slices](#string-literals-are-slices)
  - [String Slices as Parameters](#string-slices-as-parameters)
  - [Other Slices](#other-slices)

## String Slices

 ```rust
 &variable[starting_index..ending_index]
 ```

`starting_index` is the first position in the slice and `ending_index` is one more than the last position in the slice.

```rust
#![allow(unused)]
fn main() {
    let s = String::from("hello");

    let len = s.len();

    let slice = &s[3..len];
    let slice = &s[3..];
    let slice = &s[..];
}
```

🔹 String slice range indices must occur at valid UTF-8 character boundaries.

There is how we can get word before space char:

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

fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s);
}
```

## String Literals Are Slices

```rust
let s = "Hello, world!";
```

The type of `s` here is `&str`: it’s a slice pointing to that specific point of the binary. This is also why string literals are immutable; `&str` is an immutable reference.

## String Slices as Parameters

It would be better pattern to use `&str` type instead of `&String`
cause it allow us to use the same functino on both `&String` values and `&str` values:

```rust
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}

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
```

## Other Slices

Just as we might want to refer to a part of a string, we might want to refer to part of an array. We’d do so like this:

```rust

#![allow(unused)]
fn main() {
    let a = [1, 2, 3, 4, 5];

    let slice = &a[1..3];
}
```
