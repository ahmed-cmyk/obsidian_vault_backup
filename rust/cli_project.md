# Rust CLI Project: Mini-Grep

## Accepting Command Line Arguments

Rust's standard library provides `std::env::args` to access command-line arguments.

```rust
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();

    // The first argument is usually the program name
    println!("Program name: {}", args[0]);

    // Other arguments follow
    let query = &args[1];
    let filename = &args[2];

    println!("Searching for {}", query);
    println!("In file {}", filename);
}
```

## Reading Files

Use `std::fs::File` to open and read files.

```rust
use std::fs::File;
use std::io::prelude::*;

fn main() {
    // ... (argument parsing)

    let mut file = File::open(filename).expect("Could not open file");

    let mut contents = String::new();
    file.read_to_string(&mut contents).expect("Could not read file");

    println!("With text:\n{}", contents);
}
```

## Refactoring for Modularity and Error Handling

As the project grows, it's good practice to refactor code into functions and modules for better organization and error handling.

### Separating Concerns

Create a `Config` struct to hold parsed arguments and a `run` function for the main logic.

```rust
use std::env;
use std::fs;
use std::error::Error;

struct Config {
    query: String,
    filename: String,
}

impl Config {
    fn new(args: &[String]) -> Result<Config, &'static str> {
        if args.len() < 3 {
            return Err("not enough arguments");
        }
        let query = args[1].clone();
        let filename = args[2].clone();

        Ok(Config { query, filename })
    }
}

fn run(config: Config) -> Result<(), Box<dyn Error>> {
    let contents = fs::read_to_string(config.filename)?;

    println!("With text:\n{}", contents);

    Ok(())
}

fn main() {
    let args: Vec<String> = env::args().collect();

    let config = Config::new(&args).unwrap_or_else(|err| {
        eprintln!("Problem parsing arguments: {}", err);
        std::process::exit(1);
    });

    if let Err(e) = run(config) {
        eprintln!("Application error: {}", e);
        std::process::exit(1);
    }
}
```

- **`Box<dyn Error>`**: A *trait object* that means the function will return a type that implements the `Error` trait, but we don't know the exact type at compile time. This is useful for returning different error types.
- **`eprintln!`**: Prints to standard error (`stderr`), which is typically used for error messages, separating them from successful output.

## Developing the Library's Functionality (Search)

Move the core search logic into a separate library crate or module.

`src/lib.rs`:
```rust
pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {
    let mut results = Vec::new();

    for line in contents.lines() {
        if line.contains(query) {
            results.push(line);
        }
    }

    results
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn one_result() {
        let query = "duct";
        let contents = "Rust:\nsafe, fast, productive.\nPick three.";
        assert_eq!(vec!["safe, fast, productive."], search(query, contents));
    }
}
```

`src/main.rs` (updated to use the `search` function):
```rust
// ... (Config and run function from above)

fn run(config: Config) -> Result<(), Box<dyn Error>> {
    let contents = fs::read_to_string(config.filename)?;

    for line in minigrep::search(&config.query, &contents) {
        println!("{}", line);
    }

    Ok(())
}
```

## Working with Environment Variables

Rust programs can read environment variables using `std::env::var`.

```rust
use std::env;

fn main() {
    let case_sensitive = env::var("CASE_INSENSITIVE").is_err();
    // ... use case_sensitive in search logic
}
```

## Printing Error Messages to Standard Error

As shown in the refactoring section, `eprintln!` is used to print error messages to `stderr`.


