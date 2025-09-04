# Rust Object-Oriented Programming Features

## Characteristics of Object-Oriented Languages

Traditionally, OOP languages are defined by three characteristics:

-   **Encapsulation**: Bundling data and the methods that operate on that data within a single unit (e.g., an object). Rust achieves this with structs and enums, and privacy rules.
-   **Inheritance**: A mechanism whereby one object acquires all the properties and behaviors of a parent object. Rust does *not* have inheritance.
-   **Polymorphism**: The ability for objects of different types to respond to the same method call in different ways. Rust achieves this with traits and trait objects.

## Encapsulation with Structs and Enums

Rust uses structs and enums to group data and behavior. Privacy rules (the `pub` keyword) control the visibility of these items, providing encapsulation.

```rust
pub struct AveragedCollection {
    list: Vec<i32>,
    average: f64,
}

impl AveragedCollection {
    pub fn add(&mut self, value: i32) {
        self.list.push(value);
        self.update_average();
    }

    pub fn remove(&mut self) -> Option<i32> {
        let result = self.list.pop();
        match result {
            Some(value) => {
                self.update_average();
                Some(value)
            }
            None => None,
        }
    }

    pub fn average(&self) -> f64 {
        self.average
    }

    fn update_average(&mut self) {
        let total: i32 = self.list.iter().sum();
        self.average = total as f64 / self.list.len() as f64;
    }
}
```

In this example, `list` and `average` are private, and their modification is controlled by public methods like `add` and `remove`, demonstrating encapsulation.

## Traits as an Alternative to Inheritance

Rust does not have inheritance. Instead, it uses *traits* to define shared behavior. This allows for code reuse and polymorphism without the complexities of a class hierarchy.

```rust
pub trait Draw {
    fn draw(&self);
}

pub struct Screen {
    pub components: Vec<Box<dyn Draw>>,
}

impl Screen {
    pub fn run(&self) {
        for component in self.components.iter() {
            component.draw();
        }
    }
}

pub struct Button {
    pub width: u32,
    pub height: u32,
    pub label: String,
}

impl Draw for Button {
    fn draw(&self) {
        println!("Drawing a button with label: {}", self.label);
    }
}

pub struct SelectBox {
    pub width: u32,
    pub height: u32,
    pub options: Vec<String>,
}

impl Draw for SelectBox {
    fn draw(&self) {
        println!("Drawing a select box with options: {:?}", self.options);
    }
}

fn main() {
    let screen = Screen {
        components: vec![
            Box::new(SelectBox {
                width: 75,
                height: 10,
                options: vec![
                    String::from("Yes"),
                    String::from("Maybe"),
                    String::from("No"),
                ],
            }),
            Box::new(Button {
                width: 50,
                height: 10,
                label: String::from("OK"),
            }),
        ],
    };

    screen.run();
}
```

## Polymorphism with Trait Objects

Polymorphism in Rust is achieved through *trait objects*. A trait object allows you to use values of different types that implement a common trait interchangeably at runtime.

-   **`dyn Trait`**: The syntax for a trait object. It signifies that a value can be any type that implements the specified trait.
-   **Dynamic Dispatch**: When using trait objects, Rust performs dynamic dispatch, meaning it determines which method to call at runtime based on the concrete type of the value.
-   **`Box<dyn Draw>`**: We use `Box` because trait objects must have a known size at compile time. `Box` provides a pointer to the heap-allocated data, and the size of the pointer is known.

