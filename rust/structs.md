# Rust Structs

## Defining and Instantiating Structs

To define a struct, you use the `struct` keyword and give the struct a name. Inside the struct, you define fields, each with a name and a type.

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

fn main() {
    // Creating an instance of a struct
    let user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };

    // Accessing values using dot notation
    println!("User 1 email: {}", user1.email);

    // Structs are immutable by default. To change a value, the instance must be mutable.
    let mut user2 = User {
        active: true,
        username: String::from("anotherusername"),
        email: String::from("another@example.com"),
        sign_in_count: 1,
    };
    user2.email = String::from("newemail@example.com");
    println!("User 2 new email: {}", user2.email);
}
```

### Field Init Shorthand
If a field name is the same as the variable name used to initialize it, you can use field init shorthand.

```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username, // shorthand for username: username
        email,    // shorthand for email: email
        sign_in_count: 1,
    }
}
```

### Creating Instances from Other Instances with Struct Update Syntax
You can use `..` to specify that the remaining fields should take the same values as another instance.

```rust
let user3 = User {
    email: String::from("third@example.com"),
    ..user1 // user3 will have active, username, and sign_in_count from user1
};
```

## Tuple Structs

Tuple structs are like tuples but have a name. They are useful when you want to give the whole tuple a name and make it a different type from other tuples, but don't need names for individual fields.

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);

    // Access fields by index
    println!("Black R: {}", black.0);
}
```

## Unit-Like Structs

Unit-like structs are structs that have no fields. They are useful when you need to implement a trait on some type but don't have any data that you want to store in the type itself.

```rust
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}
```

## Methods Syntax

Methods are functions associated with a struct instance. They are defined within an `impl` (implementation) block.

- **`self`**: The first parameter of a method is always `self`, which represents the instance of the struct the method is being called on. It can be `&self` (immutable reference), `&mut self` (mutable reference), or `self` (takes ownership).

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    // Method that takes an immutable reference to self
    fn area(&self) -> u32 {
        self.width * self.height
    }

    // Method that takes an immutable reference to self and another Rectangle
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }

    // Associated function (not a method, doesn't take self)
    // Often used as constructors
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
    println!("The area of the rectangle is {} square pixels.", rect1.area());

    let rect2 = Rectangle {
        width: 10,
        height: 40,
    };
    let rect3 = Rectangle {
        width: 60,
        height: 45,
    };

    println!("Can rect1 hold rect2? {}", rect1.can_hold(&rect2)); // true
    println!("Can rect1 hold rect3? {}", rect1.can_hold(&rect3)); // false

    let sq = Rectangle::square(25); // Calling an associated function
    println!("Square area: {}", sq.area());
}
```

