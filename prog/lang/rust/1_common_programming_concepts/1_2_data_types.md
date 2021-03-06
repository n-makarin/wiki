# Data types

/ [home](/README.md) / [prog](/prog/README.md) / [lang](/prog/lang/README.md) / [rust](/prog/lang/rust/README.md) / [1. Common programming concepts](/prog/lang/rust/1_common_programming_concepts/README.md) / [1.2. Data types](/prog/lang/rust/1_common_programming_concepts/1_2_data_types.md)

## Table of Contents

- [Data types](#data-types)
  - [Table of Contents](#table-of-contents)
  - [Scalar Types](#scalar-types)
    - [Integer Types](#integer-types)
    - [Floating-Point Types](#floating-point-types)
    - [Numeric Operations](#numeric-operations)
    - [The Boolean Type](#the-boolean-type)
    - [The Character Type](#the-character-type)
  - [Compound Types](#compound-types)
    - [The Tuple Type](#the-tuple-type)
    - [The Array Type](#the-array-type)

## Scalar Types

Rust has two data type subsets: **scalar** and **compound**.

A scalar type represents a single value.

- integer
- floating-point number
- Boolean
- character

### Integer Types

| Length  | Signed | Unsigned |
| ------- | ------ | -------- |
| 8-bit   | i8     | u8       |
| 16-bit  | i16    | u16      |
| 32-bit  | i32    | u32      |
| 64-bit  | i64    | u64      |
| 128-bit | i128   | u128     |
| arch    | isize  | usize    |

**Signed** － may be negative

**Unsigned** － only positive numbers

**isize**, **usize** － will store 64b bits on 64-bit architecture

and 32 bits if you’re on a 32-bit architecture.

Each variable may store numbers in range:

<!-- TODO: replace to math formulas -->
- signed
![signed](/img/prog/lang/rust/signed_variable_range_formula.png)
- unsigned
![unsigned](/img/prog/lang/rust/unsigned_variable_range_formula.png)

Example:
| Length | Signed          | Unsigned   |
| ------ | --------------- | ---------- |
| 8-bit  | -128 to 127     | 0 to 255   |
| 16-bit | -32768 to 32767 | 0 to 65536 |

Note that all number literals except the byte literal allow a type suffix, such as `57u8`, and `_` as a visual separator, such as `1_000`.

### Floating-Point Types

Rust has `f32` and `f64` (default) primitive types for floating-point numbers.

```rust
fn main() {
    let x = 2.0; // f64
    let y: f32 = 3.0; // f32
}
```

### Numeric Operations

```rust
fn main() {
    // addition
    let sum = 5 + 10;

    // subtraction
    let difference = 95.5 - 4.3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32_2;

    // remainder
    let remainder = 43 % 5;
}
```

### The Boolean Type

```rust
fn main() {
    let t = true;

    let f: bool = false; // with explicit type annotation
}
```

### The Character Type

Rust’s char type is four bytes in size and represents a Unicode Scalar Value.
Chinese, Japanese, and Korean characters; emoji; and zero-width spaces are all valid char values in Rust.

```rust
fn main() {
    let c = 'z';
    let z = 'ℤ';
    let heart_eyed_cat = '😻';
}
```

## Compound Types

Compound types can group multiple values into one type.

Rust has two primitive compound types: **tuples** and **arrays**.

### The Tuple Type

Tuples have a fixed length.

Each position in the tuple has a type, and the types of the different values in the tuple don’t have to be the same.

```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
}
```

Destructure a tuple value:

```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {}", y);
}
```

We can access a tuple element directly by using a period (.):

```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

### The Array Type

Every element of an array must have the same type.

Arrays have a fixed length.

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

Array that contains the same value for each element

```rust
let a = [3; 5]; //[3, 3, 3, 3, 3]
```

```rust
let first = a[0];
let second = a[1];
```
