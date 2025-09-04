# Arrays & Hashing: isAnagram

Given two strings s and t, return true if t is an anagram of s, and false otherwise.
 
**Example 1:**
**Input:** s = "anagram", t = "nagaram"
**Output:** true
**Example 2:**
**Input:** s = "rat", t = "car"
**Output:** false

```ts
function isAnagram(s: string, t: string): boolean {
    if (s.length !== t.length) return false;

    const count = new Map<string, number>();

    for (let char of s) {
        count.set(char, (count.get(char) ?? 0) + 1);
    }

    for (let char of t) {
        if (!count.has(char)) return false;
        count.set(char, count.get(char)! - 1);
        if (count.get(char) === 0) count.delete(char);
    }

    return count.size === 0;
};

```

---

#leetcode