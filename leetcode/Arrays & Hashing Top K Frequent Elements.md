# Arrays & Hashing: Top K Frequent Elements

Given an integer array nums and an integer k, return *the* k *most frequent elements*. You may return the answer in **any order**.
 
**Example 1:**
**Input:** nums = [1,1,1,2,2,3], k = 2
**Output:** [1,2]
**Example 2:**
**Input:** nums = [1], k = 1
**Output:** [1]

```ts
function topKFrequent(nums: number[], k: number): number[] {
  const freqMap = new Map<number, number>();

  for (const num of nums) {
    freqMap.set(num, (freqMap.get(num) ?? 0) + 1);
  }

  const bucket: number[][] = Array(nums.length + 1).fill(null).map(() => []);

  for (const [num, freq] of freqMap.entries()) {
    bucket[freq].push(num);
  }

  const result: number[] = [];

  for (let i = bucket.length - 1; i >= 0 && result.length < k; i--) {
    for (const num of bucket[i]) {
      result.push(num);
      if (result.length === k) break;
    }
  }

  return result;
};
```

---

#leetcode