# Sliding Window: Permutation in String

Given two strings s1 and s2, return true if s2 contains a permutation of s1, or false otherwise.
In other words, return true if one of s1's permutations is the substring of s2.
 
**Example 1:**
**Input:** s1 = "ab", s2 = "eidbaooo"
**Output:** true
**Explanation:** s2 contains one permutation of s1 ("ba").
**Example 2:**
**Input:** s1 = "ab", s2 = "eidboaoo"
**Output:** false

```ts
function checkInclusion(s1: string, s2: string): boolean {
  if (s1.length > s2.length) return false;

  const s1Count = new Array(26).fill(0);
  const s2Count = new Array(26).fill(0);

  const aCharCode = 'a'.charCodeAt(0);

  for (let i = 0; i < s1.length; i++) {
    s1Count[s1.charCodeAt(i) - aCharCode]++;
    s2Count[s2.charCodeAt(i) - aCharCode]++;
  }

  let matches = 0;
  for (let i = 0; i < 26; i++) {
    if (s1Count[i] === s2Count[i]) matches++;
  }

  let left = 0;
  for (let right = s1.length; right < s2.length; right++) {
    if (matches === 26) return true;

    const indexIn = s2.charCodeAt(right) - aCharCode;
    const indexOut = s2.charCodeAt(left) - aCharCode;
    left++;

    s2Count[indexIn]++;
    if (s2Count[indexIn] === s1Count[indexIn]) {
      matches++;
    } else if (s2Count[indexIn] === s1Count[indexIn] + 1) {
      matches--;
    }

    s2Count[indexOut]--;
    if (s2Count[indexOut] === s1Count[indexOut]) {
      matches++;
    } else if (s2Count[indexOut] === s1Count[indexOut] - 1) {
      matches--;
    }
  }

  return matches === 26;
}
```

---

#leetcode