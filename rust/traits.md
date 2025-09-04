# Rust Traits

Traits define shared behavior that types can implement. They are similar to interfaces in other languages. Traits allow you to specify that a generic type must have a particular behavior.

### Defining a Trait

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```

### Implementing a Trait for a Type

```rust
pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}

fn main() {
    let tweet = Tweet {
        username: String::from("horse_ebooks"),
        content: String::from("of course, as you already know, people"),
        reply: false,
        retweet: false,
    };

    println!("1 new tweet: {}", tweet.summarize());

    let article = NewsArticle {
        headline: String::from("Penguins win the Stanley Cup Championship!"),
        location: String::from("Pittsburgh, PA"),
        author: String::from("Iceburgh"),
        content: String::from(
            "The Pittsburgh Penguins once again are the best hockey team in the NHL.",
        ),
    };

    println!("New article available! {}", article.summarize());
}
```

### Traits as Parameters

- **Trait Bound Syntax**: `fn notify(item: &impl Summary)` or `fn notify<T: Summary>(item: &T)`.

```rust
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}

// Equivalent using trait bound syntax
pub fn notify_generic<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```

- **Multiple Trait Bounds**: `T: Summary + Display`.
- **`where` Clauses**: For cleaner syntax with many trait bounds.
  ```rust
  fn some_function<T, U>(t: &T, u: &U) -> i32
      where T: Display + Clone,
            U: Clone + Debug
  {
      // ...
  }
  ```

### Default Implementations

Traits can have default implementations for some or all of their methods.

```rust
pub trait Summary {
    fn summarize_author(&self) -> String;

    fn summarize(&self) -> String {
        format!("(Read more from {}...)", self.summarize_author())
    }
}

impl Summary for NewsArticle {
    fn summarize_author(&self) -> String {
        format!("@{}", self.author)
    }
}
```