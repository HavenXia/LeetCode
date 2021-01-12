# 题目地址

https://leetcode.com/problems/valid-parentheses/



# 题目描述

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

**Example 1:**

```
Input: s = "()"
Output: true
```

**Example 2:**

```
Input: s = "()[]{}"
Output: true
```

**Example 3:**

```
Input: s = "(]"
Output: false
```

**Example 4:**

```
Input: s = "([)]"
Output: false
```

**Example 5:**

```
Input: s = "{[]}"
Output: true
```



# 思路

不是所有的parenthese都要想到DP, 这里要的是整个string的可行性

所以用stack就够了!



用Stack来做, easy solved

首先把遇到的每一个open parenthese 放入stack

然后每遇到一个close parenthese, qie stack.peek()等于其对应的open, 就 stack.pop(), <font color = red>注意此时stack不能是空的!</font> 

最后如果stack不是空的, 就返回false

<font color = grape>**这里真正讨巧的地方是: 如果是valid的话, 那么遇到close时stack顶部必然是其对应的open!!!!**</font>







# 题解

```java
public boolean isValid(String s) {

    Stack<Character> stack = new Stack<Character>();
    for (int i = 0; i < s.length(); i++) {

        // add open
        if (s.charAt(i) == '(' || s.charAt(i) == '[' || s.charAt(i) == '{') {
            stack.push(s.charAt(i));
        }
		
        // if empty here, must false
        if (stack.empty()) {
            return false;
        }

        // pop when meet close 
        if (s.charAt(i) == ')') {
            if (stack.peek() == '(') {
                stack.pop();
            } else {
                return false;
            }
        }
        if (s.charAt(i) == ']') {
            if (stack.peek() == '[') {
                stack.pop();
            } else {
                return false;
            }
        }
        if (s.charAt(i) == '}') {
            if (stack.peek() == '{') {
                stack.pop();
            } else {
                return false;
            }
        }

    }
    if (stack.empty()) {return true;}
    return false;
}
```

