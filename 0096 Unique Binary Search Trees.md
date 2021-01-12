# 题目地址

https://leetcode.com/problems/unique-binary-search-trees/



# 思路

看似是BFS,DFS之类的, 实则是用dp来做

+ State: dp[i] 为 consecutive increase i numbers(<font color = grape>**连续increase的i个数**</font>)所能排列的BST的数量

+ Initialize: `dp[0] = 1`, 这是必然的

+ Function: <font color = grape>**dp[n]的root位置一共有n种可能, 然后每个root一共有 left*right种可能**</font>

  + 

  + 如果第k个数在root的位置, 那么一共有`dp[k-1] * dp[n-k]` 种可能性

  + <font color = gree>Function:</font>  $dp[i]= \sum_{k=1}^{i} dp[k-1]\times dp[i-k]$ 

    ==注意这里i是从1到n, 而k是从1到i==

+ Answer: dp[n]



时间复杂度O(n<sup>2</sup>), 空间复杂度O(n)

# 题解

```java
public int numTrees(int n) {

    int[] dp = new int[n + 1];

    //initialize
    dp[0]=1;

    //Function
    for (int i = 1; i <= n; i++) {

        for (int k = 1; k <= i; k++) {

            dp[i] += dp[k - 1] * dp[i - k];
        }  

    }
    return dp[n];

}
```

