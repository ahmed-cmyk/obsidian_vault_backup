# Rust Common Collections

## Vectors (`Vec<T>`)

Vectors allow you to store more than one value in a single data structure that puts all the values next to each other in memory. Vectors can only store values of the same type.

### Creating a Vector

```rust
let v: Vec<i32> = Vec::new(); // Empty vector, type annotation needed

let v = vec![1, 2, 3]; // Macro for convenience, type inferred
```

### Updating a Vector

To add elements to a vector, you need to make it mutable.

```rust
let mut v = Vec::new();
v.push(5);
v.push(6);
v.push(7);
```

### Reading Elements of a Vector

There are two ways to reference a value stored in a vector:

1.  **Using `&` and `[]` (indexing)**: This will `panic!` if the index is out of bounds.
    ```rust
    let v = vec![1, 2, 3, 4, 5];
    let third: &i32 = &v[2];
    println!("The third element is {}", third);
    ```

2.  **Using `get` method**: This returns an `Option<&T>`, which is safer as it returns `None` if the index is out of bounds, allowing you to handle the case gracefully.
    ```rust
    let v = vec![1, 2, 3, 4, 5];
    match v.get(2) {
        Some(third) => println!("The third element is {}", third),
        None => println!("There is no third element."),
    }
    ```

### Iterating Over Vectors

```rust
let v = vec![100, 32, 57];
for i in &v { // Immutable references
    println!("{}", i);
}

let mut v = vec![100, 32, 57];
for i in &mut v { // Mutable references
    *i += 50; // Dereference to change the value
}
println!("{:?}", v);
```

### Storing Different Types in a Vector

To store different types in a vector, you can use an enum.

```rust
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
```

## Strings (`String`)

Rust has only one string type in the core language: `str`, the string slice (`&str`). The `String` type is provided by the standard library, a growable, mutable, owned, UTF-8 encoded string.

### Creating a New String

```rust
let mut s = String::new();
let s = String::from("initial contents");
let s = "initial contents".to_string();
```

### Updating a String

- **`push_str`**: Appends a string slice.
  ```rust
  let mut s1 = String::from("foo");
  let s2 = "bar";
  s1.push_str(s2);
  println!("s1 is {}", s1);
  println!("s2 is {}", s2); // s2 is still valid
  ```

- **`push`**: Appends a single character.
  ```rust
  let mut s = String::from("lo");
  s.push('l');
  ```

- **Concatenation with `+` or `format!` macro**:
  - `+` operator takes ownership of the first `String`.
  ```rust
  let s1 = String::from("Hello, ");
  let s2 = String::from("world!");
  let s3 = s1 + &s2; // s1 is moved here and can no longer be used
  // println!("{}", s1); // Error
  ```
  - `format!` macro is more efficient and doesn't take ownership.
  ```rust
  let s1 = String::from("tic");
  let s2 = String::from("tac");
  let s3 = String::from("toe");
  let s = format!("{}-{}-{}", s1, s2, s3); // s1, s2, s3 are still valid
  ```

### Indexing into Strings (Caution!)

Rust strings are UTF-8 encoded, so direct indexing into a `String` by byte index is generally not recommended as it might not correspond to a valid Unicode scalar value.

```rust
let s1 = String::from("hello");
// let h = s1[0]; // This will not compile!
```

To access characters, iterate over `chars()` or `bytes()`:

```rust
for c in "नमस्ते".chars() {
    println!("{}", c);
}

for b in "नमस्ते".bytes() {
    println!("{}", b);
}
```

## Hash Maps (`HashMap<K, V>`)

Hash maps store a mapping of keys to values. Like vectors, hash maps store their data on the heap.

### Creating a Hash Map

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
```

### Accessing Values in a Hash Map

Use the `get` method, which returns an `Option<&V>`.

```rust
let team_name = String::from("Blue");
let score = scores.get(&team_name);

match score {
    Some(s) => println!("Score for Blue team: {}", s),
    None => println!("Blue team not found."),
}
```

### Iterating Over a Hash Map

```rust
for (key, value) in &scores {
    println!("{}: {}", key, value);
}
```

### Updating a Hash Map

- **Overwriting a Value**: If you insert a key-value pair where the key already exists, the old value will be replaced.
  ```rust
  scores.insert(String::from("Blue"), 25);
  ```

- **Inserting Only If Key Has No Value**: Use `entry` and `or_insert`.
  ```rust
  scores.entry(String::from("Yellow")).or_insert(50);
  scores.entry(String::from("Green")).or_insert(50);
  ```

- **Updating a Value Based on the Old Value**: For example, counting occurrences of words.
  ```rust
  let text = "hello world wonderful world";
  let mut map = HashMap::new();

  for word in text.split_whitespace() {
      let count = map.entry(word.to_string()).or_insert(0);
      *count += 1;
  }
  println!("{:?}", map);
  ```

