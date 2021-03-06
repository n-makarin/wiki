# Control flow

/ [home](/README.md) / [prog](/prog/README.md) / [lang](/prog/lang/README.md) / [rust](/prog/lang/rust/README.md) / [1. Common programming concepts](/prog/lang/rust/1_common_programming_concepts/README.md) / [1.5. Control flow](/prog/lang/rust/1_common_programming_concepts/1_5_control_flow.md)

## Table of Contents

- [Control flow](#control-flow)
  - [Table of Contents](#table-of-contents)
  - [Conditions](#conditions)
  - [Loops](#loops)
    - [Returning Values from Loops](#returning-values-from-loops)
    - [Conditional Loops with `while`](#conditional-loops-with-while)
    - [Looping with `for`](#looping-with-for)

## Conditions

```rust
fn main() {
    let number = 3;

    if number < 5 {
        println!("condition was true");
    } else {
        println!("condition was false");
    }
}
```

❗️ The condition *must* be a `bool`.

```rust
fn main() {
    let number = 3;

    if number {
        println!("number was three");
    }
}
```

Because if is an expression, we can use it on the right side of a let statement:

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {}", number);
}
```

❗️ Variables in let statement condition *must* have a single type:

```rust
fn main() {
    let condition = true;

    let number = if condition { 5 } else { "six" };

    println!("The value of number is: {}", number);
}
```

## Loops

### Returning Values from Loops

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

### Conditional Loops with `while`

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

### Looping with `for`

This approach is slow and bug prone because of type checkin in every element on every iteration and we could cause the program to panic if the index length is incorrect.

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];
    let mut index = 0;

    while index < 5 {
        println!("the value is: {}", a[home]);

        index += 1;
    }
}
```

The better way is to use `for` loop:

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a.iter() {
        println!("the value is: {}", element);
    }
}
```

```rust
// (1..4) is a Range, which is a type provided by the standard library that generates all numbers in sequence starting from one number and ending before another number

// .rev() is a method that reverse the range
fn main() {
    for number in (1..4).rev() {
        println!("{}!", number);
    }
    println!("LIFTOFF!!!");
}

// 3!
// 2!
// 1!
// LIFTOFF!!!
```
