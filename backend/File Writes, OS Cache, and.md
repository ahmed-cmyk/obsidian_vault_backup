# File Writes, OS Cache, and

# **fsync**

When an application writes a file to disk, the data **does not immediately reach the physical disk**. Instead, the OS uses a layer of **buffers and caching** for performance reasons.

---

## **How Writes Work (Typical Flow)**

```
Application → OS page cache (RAM) → Periodic flush → Disk (SSD/HDD)
```

### **Step-by-step:**

1. **Write request**

2. Your application calls something like write() or fwrite()

3. **Data goes into the OS file system cache (page cache)**

   * The data is stored in memory in **pages** (typically 4 KB)

   * It is **marked dirty** — i.e., pending write to disk

4. **Deferred flushing**

   * The OS **waits** and batches multiple dirty pages

   * Eventually, it performs a **bulk write** to disk

   * This improves performance and reduces **write amplification** and **fragmentation**

5. **Disk I/O scheduler**

   * Reorders and merges writes to reduce seek time

   * Converts logical writes to physical writes optimized for disk layout

⠀
---

## **Why Does the OS Delay Writes?**

* **Performance**: Writing to RAM is much faster than writing to disk

* **Batching**: Grouping multiple small writes into one large write improves throughput

* **Avoid fragmentation**: Writing every change individually increases fragmentation on disk

---
## **SSDs and Write Behavior**

Unlike spinning disks, **SSDs have limited write endurance** — each cell in NAND flash memory can only be written to a finite number of times (typically 3,000 to 100,000 cycles depending on the type of NAND). Writing too frequently or inefficiently can **wear out the SSD** faster.

---

### **⚠️ Why SSDs Are Sensitive to Writes**

1. **Erase-before-write**:

2. SSDs can’t overwrite data in-place. They must **erase a full block (often 128–512 KB)** before writing even a few bytes.

3. **Write Amplification**:

4. Writing small chunks of data frequently can trigger:

   * Reading an entire block into cache

   * Modifying a small part

   * Rewriting the **entire block** back to flash

This means a **small logical write** (e.g., 4 KB) can become a **large physical write** (e.g., 256 KB or more), especially if done repeatedly and uncoordinated.

---

### **How OS Write Caching Helps SSDs**

By batching small writes in memory:

* The OS allows more **sequential and block-aligned writes**

* Reduces the number of **erase/write cycles**

* Minimizes **write amplification factor (WAF)**

* Improves SSD lifespan and performance

> This is why blindly flushing every small write (e.g., calling fsync() too often or writing logs unbuffered) can **prematurely degrade SSDs** 

---

## **SSD-specific File System Behavior**

Modern file systems (like **ext4 with journaling**, **F2FS**, or **APFS**) and SSD-aware databases try to:

* **Align writes with SSD block sizes**

* Minimize random writes

* Use **TRIM commands** to inform the SSD which blocks are no longer in use

* Reduce reliance on excessive fsync() unless strictly necessary

---

## **Databases & SSDs**

* **PostgreSQL** and **MySQL** still use fsync() to ensure durability but may do it:

  * Once per transaction group

  * After every WAL flush

* SSD-aware tuning may include:

  * Adjusting commit_delay

  * Using O_DIRECT to bypass page cache and manage buffering manually

  * Tuning the **checkpoint interval** and **buffer pool size**

---

## **Summary Table**

|  **Concept**  |  **Role in SSD write handling**  | 
|---|---|
|  **OS Page Cache**  |  Batches writes to reduce WAF  | 
|  **fsync()**  |  Forces immediate write → more wear  | 
|  **Write Amplification**  |  Logical writes → inflated physical writes  | 
|  **Erase-before-write**  |  Requires entire blocks to be erased and rewritten  | 
|  **TRIM**  |  Frees unused blocks for reuse  | 
|  **Wear leveling**  |  SSD internal controller spreads writes across cells  | 

---

## **fsync()**

## **and Durability**

> But this behavior is a **problem for databases**, which care deeply about **durability** (the “D” in ACID).

If the system crashes **before** the OS flushes dirty pages to disk, **data is lost**, even if the application believes the write succeeded.

### **To fix this, applications can call:**

```
fsync(fd)
```

* This tells the OS: **“Flush everything for this file descriptor to disk now.”**

* It blocks until the data is guaranteed to be **physically persisted**

* This is expensive but **ensures durability**

---

## Who Uses fsync()?

* **Databases** (e.g., PostgreSQL, MySQL, SQLite):

* Use fsync() after writing critical data like WAL (Write-Ahead Logs), checkpoints, etc.

* **File systems with journaling**:

* Still benefit from fsync() for app-level control over when data is safely stored

---

## **Related Concepts**

| **Term**             | **Meaning**                                                                         |     |
| -------------------- | ----------------------------------------------------------------------------------- | --- |
| **Dirty page**       | A page in memory that has been modified but not yet flushed to disk                 |     |
| **Write-back cache** | The cache used by OS to defer disk writes                                           |     |
| **Journaling**       | File systems (e.g., ext4, NTFS) log metadata changes to avoid corruption            |     |
| **O_SYNC**           | A flag to open files such that every write is immediately flushed (like auto-fsync) |     |
| **Write barrier**    | A mechanism to enforce order and persistence of writes on disk                      |     |
| **Direct I/O**       | Bypasses OS cache entirely (used by some DBs and log systems)                       |     |

---

## **Trade-offs**

| **Behavior**             | **Pros**                    | **Cons**                     |     |
| ------------------------ | --------------------------- | ---------------------------- | --- |
| **Buffered writes**      | Fast, low fragmentation     | Risk of data loss on crash   |     |
| **fsync on every write** | Strong durability           | Slow performance             |     |
| **fsync periodically**   | Balance of speed and safety | May still lose recent writes |     |

---

## **Final Thoughts**

The OS page cache is a performance optimization, but **durability-sensitive applications** like databases must actively manage when data is flushed using fsync(), O_SYNC, or **journaling** techniques to prevent data loss.

---

#backend