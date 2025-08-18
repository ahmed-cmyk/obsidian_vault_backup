# Linked List: Find Duplicate

Given an array of integers nums containing n + 1 integers where each integer is in the range [1, n] inclusive.

There is only **one repeated number** in nums, return *this repeated number*.

You must solve the problem **without** modifying the array nums and using only constant extra space.

**Example 1:**
**Input:** nums = [1,3,4,2,2]
**Output:** 2

**Example 2:**
**Input:** nums = [3,1,3,4,2]
**Output:** 3

**Example 3:**
**Input:** nums = [3,3,3,3,3]
**Output:** 3


```ts
function findDuplicate(nums: number[]): number {
    let slow = nums[0];
    let fast = nums[0];

    // Phase 1: Detect the cycle
    do {
        slow = nums[slow];
        fast = nums[nums[fast]];
    } while (slow !== fast);

    // Phase 2: Find the entry point
    fast = nums[0];
    while (slow !== fast) {
        slow = nums[slow];
        fast = nums[fast];
    }

    return slow;
}
```

---
## Key Insight

Since all numbers are between `1` and `n`, we can **treat the array like a linked list**, where:

* **Index** = node
* **Value at index** = pointer to next node

So:

```ts
tsCopyEditlet next = nums[i];  // like node.next
```

Eventually, this forms a **cycle**, because one value repeats → pointing to the same "next".

That means we can use **Floyd’s Tortoise and Hare Algorithm** to detect the start of this cycle — which is the **duplicate number**.

---

## Code (Floyd’s Cycle Detection)

```ts
tsCopyEditfunction findDuplicate(nums: number[]): number {
    let slow = nums[0];
    let fast = nums[0];

    // Phase 1: Detect cycle
    do {
        slow = nums[slow];
        fast = nums[nums[fast]];
    } while (slow !== fast);

    // Phase 2: Find entry point of the cycle (duplicate number)
    fast = nums[0];
    while (slow !== fast) {
        slow = nums[slow];
        fast = nums[fast];
    }

    return slow;
}
```

---
## Explanation
### Phase 1: Cycle Detection

We move:

* `slow` by 1 step: `slow = nums[slow]`
* `fast` by 2 steps: `fast = nums[nums[fast]]`

Eventually, they meet **inside the cycle**.

### Phase 2: Cycle Entry (duplicate)

We reset `fast` to `nums[0]`, then move both pointers **one step at a time**:

```ts
tsCopyEditslow = nums[slow]
fast = nums[fast]
```

When they meet again — they’re at the **start of the cycle**, which corresponds to the **duplicate number**.

---
## Why This Works

* Since there are `n+1` values in the range `[1, n]`, **at least one number must repeat** (Pigeonhole Principle).
* The array acts like a linked list with a cycle.
* The cycle **starts** at the **duplicate number**.

---

#leetcode