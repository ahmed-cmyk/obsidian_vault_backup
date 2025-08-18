# Sliding Window: Longest Repeating Character Replacement

You are given a string s and an integer k. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most k times.
Return *the length of the longest substring containing the same letter you can get after performing the above operations*.
 
**Example 1:**
**Input:** s = "ABAB", k = 2
**Output:** 4
**Explanation:** Replace the two 'A's with two 'B's or vice versa.
**Example 2:**
**Input:** s = "AABABBA", k = 1
**Output:** 4
**Explanation:** Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.
There may exists other ways to achieve this answer too.

```ts
function characterReplacement(s: string, k: number): number {
    const count: Record<string, number> = {};
    let start = 0;
    let maxCount = 0;
    let maxLength = 0;

    for (let end = 0; end < s.length; end++) {
        const char = s[end];
        count[char] = (count[char] || 0) + 1;

        maxCount = Math.max(maxCount, count[char]);

        while ((end - start + 1) - maxCount > k) {
            count[s[start]]--;
            start++;
        }

        maxLength = Math.max(maxLength, end - start + 1);
    }

    return maxLength;
}
```

---

#leetcode