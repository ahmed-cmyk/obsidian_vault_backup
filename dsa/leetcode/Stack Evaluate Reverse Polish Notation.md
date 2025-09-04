# Stack: Evaluate Reverse Polish Notation

You are given an array of strings tokens that represents an arithmetic expression in a [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).
Evaluate the expression. Return *an integer that represents the value of the expression*.
**Note** that:
* The valid operators are '+', '-', '*', and '/'.
* Each operand may be an integer or another expression.
* The division between two integers always **truncates toward zero**.
* There will not be any division by zero.
* The input represents a valid arithmetic expression in a reverse polish notation.
* The answer and all the intermediate calculations can be represented in a **32-bit** integer.

⠀ 
**Example 1:**
**Input:** tokens = ["2","1","+","3","*"]
**Output:** 9
**Explanation:** ((2 + 1) * 3) = 9
**Example 2:**
**Input:** tokens = ["4","13","5","/","+"]
**Output:** 6
**Explanation:** (4 + (13 / 5)) = 6
**Example 3:**
**Input:** tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
**Output:** 22
**Explanation:** ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22

```ts
function multiply(a: number, b: number) {
    return a * b;
}

function divide(a: number, b: number) {
    return Math.trunc(a / b); // usually integer division in RPN problems
}

function add(a: number, b: number) {
    return a + b;
}

function subtract(a: number, b: number) {
    return a - b;
}

function evalRPN(tokens: string[]): number {
    const stack: number[] = [];

    for (let token of tokens) {
        if (token === "+") {
            const b = stack.pop()!;
            const a = stack.pop()!;
            stack.push(add(a, b));
        } else if (token === "-") {
            const b = stack.pop()!;
            const a = stack.pop()!;
            stack.push(subtract(a, b));
        } else if (token === "*") {
            const b = stack.pop()!;
            const a = stack.pop()!;
            stack.push(multiply(a, b));
        } else if (token === "/") {
            const b = stack.pop()!;
            const a = stack.pop()!;
            stack.push(divide(a, b));
        } else {
            stack.push(Number(token));
        }
    }

    return stack.pop()!;
}
```

---

#leetcode