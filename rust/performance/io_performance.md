
## **Locking**

- **What happens:**
    - print! and println! macros lock stdout on every call.
    - For many writes, this repeated locking becomes inefficient.
    
- **Better approach:**
    Manually lock stdout once, then reuse it:

```rust
use std::io::Write;

let mut stdout = std::io::stdout();
let mut lock = stdout.lock();

for line in lines {
    writeln!(lock, "{}", line)?;
}
// lock is released when dropped
```

> **Note:** same applies to stdin and stderr.

---

## **Buffering**

- **What happens:**
    - Rust file I/O is **unbuffered by default**.
    - Many small I/O operations → many system calls → performance hit.
- **Solution:** use BufReader or BufWriter.
    - They keep an in-memory buffer to reduce syscalls.
    
- **Writer example:**
    
Unbuffered:

```rust
use std::io::Write;

let mut out = std::fs::File::create("test.txt")?;
for line in lines {
    writeln!(out, "{}", line)?;
}
```

Buffered:

```rust
use std::io::{BufWriter, Write};

let mut out = BufWriter::new(std::fs::File::create("test.txt")?);
for line in lines {
    writeln!(out, "{}", line)?;
}
out.flush()?; // makes error explicit
```

- **Key points:**
    - Writers: same API (Write), so easy to switch between buffered/unbuffered.
    - Readers: Read vs BufRead traits → buffered reading unlocks line-based APIs (read_line, lines).
- **Extra tip:** Combine **manual locking + buffering** when writing lots to stdout.

---
## **Reading Lines**

- **Problem:** naive line reading may cause **excessive allocations**.
- **Solution:** prefer BufRead::read_line or BufRead::lines which reuse buffers efficiently.

---
## **Raw Bytes vs UTF-8**

- **Default:** String enforces UTF-8, adding overhead for validation.
- **Alternative:** use BufRead::read_until if you just need **raw bytes** (e.g. ASCII data).
- **Crates:** there are dedicated crates for byte-oriented line reading and byte strings.

---