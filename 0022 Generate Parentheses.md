# 题目地址

https://leetcode.com/problems/generate-parentheses/



# 题目描述


Given `n` pairs of parentheses, write a function to *generate all combinations of well-formed parentheses*.

**Example 1:**

```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

**Example 2:**

```
Input: n = 1
Output: ["()"]
```

 

**Constraints:**

- `1 <= n <= 8`



# 思路

用dfs就可以很轻松的解决, 重点在于有<font color = grape>两个出口</font>

+ 第一个出口是 `string.length() == 2 * n`,  此时加入result并return
+ 第二个出口是 `open < close` , 只要出现右括号比左括号多就可以直接return了!

时间复杂度为O(4<sup>n</sup>),  <font color = grape>因为一共有2n层, 每个节点有2个选择, 所以是$2^{2n}$</font> 

空间复杂度也是O(4<sup>n</sup>), 最后一层一共有$2^{2n}$种可能



# 题解

```java
public List<String> generateParenthesis(int n) {
    
    List<String> result = new ArrayList<String>();
    dfs(result, "", 0, 0, n);
    return result;
}

private void dfs(List<String> result, String cur, int open, int close, int max) {
    if (open < close) {
        return;
    }
    if (cur.length() == 2 * max) {
        result.add(cur);
        return;
    }
    if (open < max) {
        dfs(result, cur + "(", open + 1, close, max);
    }
    if (close < max) {
        dfs(result, cur + ")", open, close + 1, max);
    }
}
```

