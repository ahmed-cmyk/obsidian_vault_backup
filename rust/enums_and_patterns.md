# Rust Enums and Pattern Matching

## Defining an Enum

Enums are defined using the `enum` keyword. Each variant can optionally have data associated with it.

```rust
enum IpAddrKind {
    V4,
    V6,
}

fn main() {
    let four = IpAddrKind::V4;
    let six = IpAddrKind::V6;

    route(IpAddrKind::V4);
    route(IpAddrKind::V6);
}

fn route(ip_kind: IpAddrKind) {
    // ...
}
```

### Enum Variants with Data

Enum variants can store data directly within them, and each variant can have different types and amounts of associated data.

```rust
enum IpAddr {
    V4(String),
    V6(String),
}

fn main() {
    let home = IpAddr::V4(String::from("127.0.0.1"));
    let loopback = IpAddr::V6(String::from("::1"));
}
```

Even more complex data can be stored:

```rust
enum IpAddrComplex {
    V4(u8, u8, u8, u8),
    V6(String),
}

fn main() {
    let home = IpAddrComplex::V4(127, 0, 0, 1);
    let loopback = IpAddrComplex::V6(String::from("::1"));
}
```

## The `Option` Enum and Its Advantages Over Null Values

Rust does not have the concept of `null`. Instead, it has the `Option<T>` enum, which is defined in the standard library:

```rust
enum Option<T> {
    None,    // Represents no value or absence of a value
    Some(T), // Represents a value of type T
}
```

`Option<T>` is used to express that a value could be something or nothing. This forces you to explicitly handle the case where there is no value, preventing common null pointer exceptions found in other languages.

```rust
fn main() {
    let some_number = Some(5);
    let some_char = Some('e');
    let absent_number: Option<i32> = None;

    // You cannot directly add an Option<i32> to an i32
    // let x: i8 = 5;
    // let y: Option<i8> = Some(5);
    // let sum = x + y; // This would be a compile-time error

    // To use the value inside an Option, you must handle the Some and None cases.
    // This is typically done with a `match` expression.
}
```

## The `match` Control Flow Operator

The `match` expression allows you to compare a value against a series of patterns and then execute code based on which pattern the value matches. It is exhaustive, meaning all possibilities must be covered.

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => {
            println!("Lucky penny!");
            1
        },
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}

fn main() {
    println!("Penny value: {}", value_in_cents(Coin::Penny));
    println!("Quarter value: {}", value_in_cents(Coin::Quarter));

    let five = Some(5);
    let six = plus_one(five);
    let none = plus_one(None);

    println!("Five plus one: {:?}", six);
    println!("None plus one: {:?}", none);
}

fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}
```

## Concise Control Flow with `if let`

`if let` is a concise way to handle a single pattern that matches an `enum` variant, ignoring all other variants. It's syntactic sugar for a `match` that only cares about one case.

```rust
fn main() {
    let config_max = Some(3u8);
    match config_max {
        Some(max) => println!("The maximum is configured to be {}", max),
        _ => (),
    }

    // The equivalent using if let:
    if let Some(max) = config_max {
        println!("The maximum is configured to be {}", max);
    }

    // You can also have an else block with if let
    let mut count = 0;
    let coin = Coin::Quarter;

    if let Coin::Quarter = coin {
        println!("Lucky quarter!");
    } else {
        count += 1;
    }
    println!("Count: {}", count);
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}
```

