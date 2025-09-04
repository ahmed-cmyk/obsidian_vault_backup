# Rust Ownership

## What is Ownership?

Ownership is a system for managing memory. It ensures that memory is freed promptly and prevents common memory-related errors like dangling pointers, caught at compile time.

### Ownership Rules:
1.  Each value in Rust has a variable that's called its *owner*.
2.  There can only be one owner at a time.
3.  When the owner goes out of scope, the value will be *dropped* (memory deallocated).

## The Stack and the Heap

- **Stack**: Stores values in the order they get them and removes them in the opposite order (Last In, First Out - LIFO). Data on the stack must have a known, fixed size.
- **Heap**: Less organized. When you put data on the heap, you request a certain amount of space. The allocator finds an empty spot, marks it as being in use, and returns a *pointer* to that location. Accessing data on the heap is slower because you have to follow a pointer.

Rust's ownership rules manage heap data.

### `String` vs. String Literals

- **String Literals (`&str`)**: Immutable, fixed size, stored on the stack (or directly in the executable's binary).
- **`String`**: Growable, mutable, heap-allocated. Used when you need to modify text.

```rust
let s1 = String::from("hello");
let s2 = s1; // s1 is moved to s2, s1 is no longer valid
// println!("{}", s1); // This would cause a compile-time error

let s3 = s2.clone(); // Deep copy, both s2 and s3 are valid
println!("s2 = {}, s3 = {}", s2, s3);
```

## Ownership and Functions

Passing a variable to a function will *move* or *copy* it, similar to assignment.

- **Moving**: For types like `String`, ownership is transferred.
  ```rust
  fn takes_ownership(some_string: String) { // some_string comes into scope
      println!("{}", some_string);
  } // some_string goes out of scope and `drop` is called.

  let s = String::from("hello");
  takes_ownership(s); // s's value moves into the function
  // println!("{}", s); // s is no longer valid here
  ```

- **Copying**: For types that implement the `Copy` trait (e.g., integers, booleans, characters, tuples containing only `Copy` types), a copy is made, and the original variable remains valid.
  ```rust
  fn makes_copy(some_integer: i32) { // some_integer comes into scope
      println!("{}", some_integer);
  } // some_integer goes out of scope.

  let x = 5;
  makes_copy(x); // x's value copies into the function
  println!("{}", x); // x is still valid here
  ```

## References and Borrowing

References allow you to refer to a value without taking ownership of it. This is called *borrowing*.

- **Immutable References (`&`)**: You can have multiple immutable references to a value at any given time.
  ```rust
  fn calculate_length(s: &String) -> usize { // s is a reference to a String
      s.len()
  } // s goes out of scope, but because it does not have ownership of what it refers to, it is not dropped.

  let s1 = String::from("hello");
  let len = calculate_length(&s1); // Pass a reference
  println!("The length of '{}' is {}.", s1, len); // s1 is still valid
  ```

- **Mutable References (`&mut`)**: You can have *only one* mutable reference to a particular piece of data in a particular scope.
  ```rust
  fn change(some_string: &mut String) {
      some_string.push_str(", world");
  }

  let mut s = String::from("hello");
  change(&mut s);
  println!("{}", s);
  ```

- **Dangling References**: Rust prevents dangling references (pointers to data that has been deallocated) at compile time.
  ```rust
  // fn dangle() -> &String { // This function would not compile
  //     let s = String::from("hello");
  //     &s // We return a reference to `s`
  // } // `s` goes out of scope and is dropped. Its memory is deallocated.
  // // The reference would be dangling.
  ```

## Slices

Slices let you refer to a contiguous sequence of elements in a collection rather than the whole collection.

- **String Slices (`&str`)**: A reference to part of a `String`.
  ```rust
  let s = String::from("hello world");
  let hello = &s[0..5]; // or &s[..5]
  let world = &s[6..11]; // or &s[6..]
  let whole = &s[..]; // Reference to the entire string
  ```

- **Other Slices**: Slices can also be used with arrays.
  ```rust
  let a = [1, 2, 3, 4, 5];
  let slice = &a[1..3]; // slice is &[2, 3]
  ```

