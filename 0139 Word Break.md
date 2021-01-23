# 题目地址

https://leetcode.com/problems/word-break/



# 思路

可以用一维DP来做

+ State: `dp[i]` 代表index 从0 到i - 1的可行性,  所以此时dp[0]为空string, dp[1]是第一个char

+ Initialize:  `dp[0]= True`,<font color = grape>这里的dp[0]必须设置为True, 不然所有的都是false了</font> 

  <font color = red>这里有陷阱, 如果input是"", set里面contains("")的, 那么是应当返回true的</font>

+ Function: 虽然是一维数组, 但是需要循环

  `dp[1] = dp[0] && dict.contains(substring(0,1)) `

  `dp[2]= dp[0] &&dict.contains(substring(0,2)) || dp[1] &&dict.contains(substring(1,2)) ` `

因此$dp[i] = \sum_{k=0}^{i} dp[k]$ &&  `dict.contains(substring(k,i))`, <font color = grape>**只要有一组满足, 那么dp[i]就是true的**</font>

+ Answer: return dp[n]即可, <font color = grape>代表从0到n-1的可行性</font>

时间复杂度O(n<sup>3</sup>), <font color = grape>**因为s.substring也要O(n)**</font>  空间复杂度O(n)

```java
public boolean wordBreak(String s, List<String> wordDict) {

    if (s == null) {return false;}
    if (wordDict == null || wordDict.size() == 0) {return false;}
    Set<String> set = new HashSet<String>();
    for (String word: wordDict) {
        set.add(word);
    }
    // check if s is in the set
    if (set.contains(s)) {return true;}

    // dp
    int n = s.length();
    boolean[] dp = new boolean[n + 1];
    // initialize
    dp[0] = true;

    // function
    for (int i = 1; i <= n; i++) {
        dp[i] = false;
        for (int k = 0; k < i; k++) {
            if (dp[k] && set.contains(s.substring(k, i))) {
                dp[i] = true;
                continue;
            }
        }
    }
    return dp[n]; 
}
```

