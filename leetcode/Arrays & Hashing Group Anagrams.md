# Arrays & Hashing: Group Anagrams

Given an array of strings strs, group the anagrams together. You can return the answer in **any order**.
 
**Example 1:**
**Input:** strs = ["eat","tea","tan","ate","nat","bat"]
**Output:** [["bat"],["nat","tan"],["ate","eat","tea"]]
**Explanation:**
* There is no string in strs that can be rearranged to form "bat".
* The strings "nat" and "tan" are anagrams as they can be rearranged to form each other.
* The strings "ate", "eat", and "tea" are anagrams as they can be rearranged to form each other.

⠀**Example 2:**
**Input:** strs = [""]
**Output:** [[""]]
**Example 3:**
**Input:** strs = ["a"]
**Output:** [["a"]]

```ts
function groupAnagrams(strs: string[]): string[][] {
  const map = new Map<string, string[]>();

  for (const str of strs) {
    const count = new Array(26).fill(0);

    for (const char of str) {
      count[char.charCodeAt(0) - 97]++;
    }

    const key = count.join('#');

    if (!map.has(key)) {
      map.set(key, []);
    }

    map.get(key)!.push(str);
  }

  return Array.from(map.values());
}
```

---

#leetcode