# 题目地址

https://leetcode.com/problems/longest-palindromic-substring/



# 题目描述

Given a string `s`, return *the longest palindromic substring* in `s`.

**Example 1:**

```
Input: s = "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

**Example 2:**

```
Input: s = "cbbd"
Output: "bb"
```

**Example 3:**

```
Input: s = "a"
Output: "a"
```

**Example 4:**

```
Input: s = "ac"
Output: "a"
```

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consist of only digits and English letters (lower-case and/or upper-case),



# 思路

最暴力的方法,  从index=0循环到index = length - 1

以每个点分别做一次odd 和 even的检测

<font color = grape>唯一注意的就是even的检测最后一次要跳过! right可能out of index</font>

时间复杂度为O(n<sup>2</sup>)



# 题解

```java
public String longestPalindrome(String s) {

    int left = 0;
    int right = 0;
    String result = "";

    for (int i = 0; i < s.length(); i++) {

        left = i;
        right = i;

        while (s.charAt(left) == s.charAt(right)) {
			
            // 直到当前的length 比result大了!
            if (result.length() < right - left + 1) {
                result = s.substring(left, right + 1);
            }

            if (left <= 0 || right >= s.length() - 1) {
                break;
            }
            left--;
            right++;   
        } 

        left = i;
        right = i + 1;
        if (right > s.length() - 1) {
            continue;
        }

        while (s.charAt(left) == s.charAt(right)) {

            if (result.length() < right - left + 1) {
                result = s.substring(left, right + 1);
            }

            if (left <= 0 || right >= s.length() - 1) {
                break;
            }
            left--;
            right++;   
        }
    }
    return result;

}
```

