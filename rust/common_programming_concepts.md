# Rust Common Programming Concepts

## Variables and Mutability

Rust variables are immutable by default, meaning once a value is bound to a name, you can't change that value.

- **Immutability**: By default, variables are immutable.
  ```rust
  let x = 5;
  println!("The value of x is: {}", x);
  // x = 6; // This would cause a compile-time error
  ```

- **Mutability**: To make a variable mutable, use the `mut` keyword.
  ```rust
  let mut x = 5;
  println!("The value of x is: {}", x);
  x = 6; // This is allowed
  println!("The value of x is: {}", x);
  ```

- **Constants**: Declared with `const`, must have their type annotated, and their value must be known at compile time. They cannot be mutable.
  ```rust
  const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
  ```

- **Shadowing**: You can declare a new variable with the same name as a previous one. The new variable *shadows* the old one, effectively hiding it. This is different from `mut` because it creates a *new* variable, allowing for type changes.
  ```rust
  let x = 5;
  let x = x + 1; // x is now 6
  let x = x * 2; // x is now 12
  println!("The value of x is: {}", x);

  let spaces = "   ";
  let spaces = spaces.len(); // spaces is now 3 (an integer)
  ```

## Data Types

Rust is a statically typed language, meaning the type of every variable must be known at compile time. Rust usually infers types, but sometimes explicit type annotations are needed.

### Scalar Types
Represent a single value.

- **Integers**: Whole numbers.
  - Signed (`i8`, `i16`, `i32`, `i64`, `i128`, `isize`): Can be positive or negative.
  - Unsigned (`u8`, `u16`, `u32`, `u64`, `u128`, `usize`): Only positive.
  - `isize` and `usize` depend on the architecture (32-bit or 64-bit).
  - Default is `i32`.
  - Integer overflow: In debug mode, Rust panics. In release mode, it performs two's complement wrapping.

- **Floating-Point Numbers**: `f32` (single-precision) and `f64` (double-precision).
  - Default is `f64`.
  ```rust
  let x = 2.0; // f64
  let y: f32 = 3.0; // f32
  ```

- **Booleans**: `bool` type, `true` or `false`.
  ```rust
  let t = true;
  let f: bool = false; // with explicit type annotation
  ```

- **Characters**: `char` type, represents a single Unicode scalar value.
  ```rust
  let c = 'z';
  let z = 'ℤ'; // Unicode character
  let heart_eyed_cat = '😻';
  ```

### Compound Types
Group multiple values into one type.

- **Tuples**: Fixed-size, ordered list of values of different types.
  ```rust
  let tup: (i32, f64, u8) = (500, 6.4, 1);
  let (x, y, z) = tup; // Destructuring
  println!("The value of y is: {}", y);

  let five_hundred = tup.0; // Accessing by index
  let six_point_four = tup.1;
  ```

- **Arrays**: Fixed-size, ordered list of values of the *same* type.
  - Stored on the stack.
  - Useful when you know the exact number of elements.
  ```rust
  let a = [1, 2, 3, 4, 5];
  let months = ["January", "February", "March"];
  let a: [i32; 5] = [1, 2, 3, 4, 5]; // Type annotation: [type; length]
  let a = [3; 5]; // [3, 3, 3, 3, 3]

  let first = a[0]; // Accessing elements
  // let index = 10;
  // let element = a[index]; // This would panic at runtime if index is out of bounds
  ```
  Rust performs runtime checks for array access to prevent invalid memory access.

## Functions

Functions are declared using the `fn` keyword.

- **Definition**: `fn function_name(parameters) -> return_type { ... }`
- **Parameters**: Type annotations are required for parameters.
- **Statements vs. Expressions**: Rust is an expression-based language.
  - **Statements**: Instructions that perform an action but don't return a value (e.g., `let y = 6;`).
  - **Expressions**: Evaluate to a value (e.g., `5 + 6`, function calls, blocks of code).
  - Expressions do not end with semicolons. Adding a semicolon turns an expression into a statement.

  ```rust
  fn main() {
      another_function(5, 6);

      let y = {
          let x = 3;
          x + 1 // This is an expression, no semicolon
      };
      println!("The value of y is: {}", y);
  }

  fn another_function(x: i32, y: i32) {
      println!("The value of x is: {}", x);
      println!("The value of y is: {}", y);
  }

  fn five() -> i32 {
      5 // This is an expression, implicitly returns 5
  }
  ```

## Comments

- **Line comments**: `// This is a line comment`
- **Doc comments**: `/// This is a documentation comment` (for items) or `//! This is a documentation comment` (for crates/modules).

## Control Flow

- **`if` Expressions**: Conditional execution.
  - Conditions must be `bool`.
  - `else if` for multiple conditions.
  - `if` is an expression, so it can return a value.
  ```rust
  let number = 3;
  if number < 5 {
      println!("condition was true");
  } else {
      println!("condition was false");
  }

  let number = if true { 5 } else { 6 }; // number will be 5
  ```

- **`loop`**: Repeats code indefinitely until explicitly stopped with `break`.
  - Can return a value.
  ```rust
  let mut counter = 0;
  let result = loop {
      counter += 1;
      if counter == 10 {
          break counter * 2; // Break and return a value
      }
  };
  println!("The result is {}", result);
  ```

- **`while` Loop**: Repeats code while a condition is true.
  ```rust
  let mut number = 3;
  while number != 0 {
      println!("{}", number);
      number -= 1;
  }
  println!("LIFTOFF!!!");
  ```

- **`for` Loop**: Iterates over a collection.
  - Safer and more concise than `while` for iterating.
  ```rust
  let a = [10, 20, 30, 40, 50];
  for element in a.iter() {
      println!("the value is: {}", element);
  }

  // Countdown using a range
  for number in (1..4).rev() {
      println!("{}", number);
  }
  println!("LIFTOFF!!!");
  ```
