# 题目地址

https://leetcode.com/problems/longest-substring-without-repeating-characters/



# 题目描述

Given a string `s`, find the length of the **longest substring** without repeating characters.

**Example 1:**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 2:**

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

**Example 4:**

```
Input: s = ""
Output: 0
```

**Constraints:**

- `0 <= s.length <= 5 * 104`
- `s` consists of English letters, digits, symbols and spaces.



# 思路

双指针可解

需要注意的是, 每次right pointer meet a char in the visited set, <font color = grape>不能直接让left = right, 而是只能left++ </font> 

比如 “abcade”, 遇到第二个a之后left不能直接到d,而是继续从b开始!

### 使用HashMap的解法

用hashmap来存储可以更快一点, 每个char对应其index, 一旦检测到相同的char, left就跳到重复char的index + 1, <font color = grape>然后right=left, HashMap清空</font>

但是依旧是O(n <sup>2</sup>)的复杂度, 只是比用HashSet 快一点

### 如何进一步优化?

需要做到不清空HashMap也能遍历

right指针一直向右侧遍历, 每次检测到出现遇到过的值, left就直接update成 `Math.max(max,visited.get(s.charAt(right)) + 1)` 

<font color = grape>这里用max是为了确保如果是和left之前的char重复, 就不更新left!</font> 

这样的话时间复杂度是O(n)



# 题解

```java
public int lengthOfLongestSubstring(String s) {

    if (s == null || s.length() == 0) {
        return 0;
    }

    int left = 0;
    int right = 0;
    int max = 0;
    Map<Character, Integer> visited = new HashMap<Character, Integer>();;

    for (right = 0; right < s.length(); right++) {
        
        if (visited.containsKey(s.charAt(right))) {

            max = Math.max(max, right - left);
            left = Math.max(left, visited.get(s.charAt(right)) + 1);                
        }
        // update 和 初次put 共用了一行code!
        visited.put(s.charAt(right), right);
    }
    max = Math.max(max, right - left);
    return max;
}
```

