# Rust Lifetimes

Lifetimes are a form of generics that give the Rust compiler information about how references relate to each other. This information is crucial for the borrow checker to ensure that all references are valid throughout their use, preventing dangling references.

### Preventing Dangling References

```rust
// This won't compile because the reference `r` would outlive `x`
// fn main() {
//     let r;
//     {
//         let x = 5;
//         r = &x;
//     }
//     println!("r: {}", r);
// }
```

### Generic Lifetimes in Functions

When a function takes references, you might need to annotate lifetimes to tell the borrow checker how the lifetimes of the input references relate to the lifetime of the output reference.

```rust
// 'a is a lifetime parameter
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}

fn main() {
    let string1 = String::from("abcd");
    let string2 = "xyz";
    let result = longest(string1.as_str(), string2);
    println!("The longest string is {}", result);

    let string3 = String::from("long string is long");
    {
        let string4 = String::from("xyz");
        let result = longest(string3.as_str(), string4.as_str());
        println!("The longest string is {}", result);
    }
}
```

- **Lifetime Annotation Syntax**: `'a` (a single quote followed by a lowercase letter).
- **Lifetime Elision Rules**: The compiler has some rules to infer lifetimes, so you don't always need to annotate them.

### Generic Lifetimes in Structs

```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}

fn main() {
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.').next().expect("Could not find a '.'");
    let i = ImportantExcerpt {
        part: first_sentence,
    };
}
```

### Static Lifetime

- **`'static`**: Denotes that the affected reference can live for the entire duration of the program. All string literals have the `'static` lifetime.

```rust
let s: &'static str = "I have a static lifetime.";
```