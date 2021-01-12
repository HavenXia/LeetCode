# 题目地址

https://leetcode.com/problems/divide-two-integers/



# 题目描述

Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.

 

**Example 1:**

```
Input: s = "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()".
```

**Example 2:**

```
Input: s = ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()".
```

**Example 3:**

```
Input: s = ""
Output: 0
```

 

# 思路

属于前缀型中的划分型DP

+ State: `dp[i]` 表示从index i开始, valid parenthese的长度

+ Initialize: `dp[i] = 0` ,  如果在index 的处为 `')'`, 那么肯定是0

+ Function: 此处需要<font color = grape>反向loop, 因为`dp[i]` 代表的是i作开头, 所以要从dp[n-2]开始(dp[n-1]必然是0)</font>

  + 如果 `s[i] == '(' && s[i+1] == ')' ` , 那么i到i+1这一对parenthese必然是符合的

    此时 `dp[i] = (i+2>n ? 0 : dp[i+2]) + 2` , 直接+2即可

  + 但是如果<font color = grape>**`s[i] == '(' && s[i+1] == '('` , 连续两个左括号**</font>

    就必须接着判断从 **第二个左括号index** 加上 **其最长valid值** 再+1, 是否为 `')'`

    即 判断`s[i + dp[i+1] + 1] == ')'`, 如果是的话:

    此时 `dp[i] = dp[i+1] + 2 + dp[i + dp[i+1] + 2] `

    为什么还会加上`dp[i + dp[i+1] + 2] `? 

    ==这代表当前双括号找到对应的双括号后面一位的dp值!==

    可以理解成<font color = grape>**左边每读到一个多余的(, 就去右边抵消一个)**</font>

    因为可能出现 `((()))()()`, 此时后8位dp 值分别为 2 0 0 0 4 0 2 0

    但是此时dp[1] 应该是 dp[2] + 2之后, <font color = grape>**再加上dp[4]**</font>, 此时后九位dp值为 8 2 0 0 0 4 0 2 0

    即使有第三个左括号, 此时dp[0 + dp[1] + 2] 也必然是达到了0的位置, 不会影响

+ Answer: 返回dp[0]

+ **需要注意的是, 全局需要一个max来在每个dp值计算出来后比较一下**

时间复杂度O(n), 空间复杂度O(n)

# 题解

```java
public int longestValidParentheses(String s) {

    if (s == null || s.length() == 0) {
        return 0;
    }

    // state and initialize
    int[] dp = new int[s.length() + 1];
    int max = 0;
    int n = s.length();

    for (int i = n - 2; i >= 0; i--) {
        if (s.charAt(i) == '(') {

            // 是右括号
            if (s.charAt(i+1) == ')') {
                dp[i] = (i+2>n ? 0 : dp[i+2]) + 2;
            } else {
                if (i + dp[i+1] + 1 < n && s.charAt(i + dp[i+1] + 1) == ')') {
                    dp[i] = dp[i + 1] + 2 + dp[i + dp[i+1] + 2];
                }
            }             
        }
        max = Math.max(max, dp[i]);              
    }
    return max;
}
```





