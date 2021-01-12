# 题目地址

https://leetcode.com/problems/unique-paths/



# 思路

第一反应是用Traversal Recursion来做, 但是会超时, 所以实际上还是用dp来做就好

+ Status: `dp[i][j]` 代表从左上角到ij position的total ways
+ Initialize: <font color = gree>第一列和第一行, `dp[0][j] = 1`, `dp[i][0] = 1`, 只能这么走</font>
+ Function: `dp[i][j] = dp[i-1][j] + dp[i][j-1]` 是两者相加之和
+ Answeri: `dp[m-1][n-1]`

时间复杂度O(mn), 空间复杂度O(mn)



# 题解

```java
public int uniquePaths(int m, int n) {

    if (m == 0 || n == 0) {
        return 0;
    }

    int[][] dp = new int[m][n];
    //initialize
    for (int i = 0; i < m; i++) {dp[i][0] = 1;}
    for (int j = 0; j < n; j++) {dp[0][j] = 1;}
    //function
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            dp[i][j] = dp[i-1][j] + dp[i][j-1];
        }
    }
    return dp[m-1][n-1];
}
```

