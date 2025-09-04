# Stack: Valid Parentheses

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:
1 Open brackets must be closed by the same type of brackets.
2 Open brackets must be closed in the correct order.
3 Every close bracket has a corresponding open bracket of the same type.

⠀ 
**Example 1:**
**Input:** s = "()"
**Output:** true
**Example 2:**
**Input:** s = "()[]{}"
**Output:** true
**Example 3:**
**Input:** s = "(]"
**Output:** false
**Example 4:**
**Input:** s = "([])"
**Output:** true
**Example 5:**
**Input:** s = "([)]"
**Output:** false

```ts
function isValid(s: string): boolean {
    if (s.length % 2 !== 0) return false;

    let stack: string[] = [];
    let map = new Map<string, string>([
        [")", "("],
        ["]", "["],
        ["}", "{"],
    ]);
    
    for (let char of s) {
        if (map.has(char)) {
            if (stack.pop() !== map.get(char)) {
                return false;
            }
        } else {
            stack.push(char);
        }
    }

    return stack.length === 0;
};
```

---

#leetcode