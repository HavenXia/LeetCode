# 题目地址

https://leetcode.com/problems/minimum-path-sum/



# 思路

用DP来做

+ Status: `dp[i][j]` 代表从左上角到 i,j  position的minimum sum

+ Initialize: `dp[0][0] = grid[0][0]` 

  <font color = gree>以及第一列和第一行, `dp[0][j] = dp[0][j-1] + grid[0][j]`, `dp[i][0]= dp[i-1][0] + grid[i][0]`, 只能这么走</font>

+ Function: `dp[i][j] = Math.min(dp[i-1][j], dp[i][j-1]) + grid[i][j]`, 上方和左方的最小值加上本身

+ Answer: `dp[m-1][n-1]`

时间复杂度O(mn), 空间复杂度O(mn)



# 题解

```java
public int minPathSum(int[][] grid) {

    if (grid == null || grid.length == 0) {
        return 0;
    }


    int m = grid.length;
    int n = grid[0].length;

    int[][] dp = new int[m][n];
    //initialize
    dp[0][0] = grid[0][0];
    for (int i = 1; i < m; i++) {
        dp[i][0] = dp[i - 1][0] + grid[i][0];
    }
    for (int j = 1; j < n; j++) {
        dp[0][j] = dp[0][j - 1] + grid[0][j];
    }

    //function
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            dp[i][j] = Math.min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
        }
    }

    return dp[m-1][n-1];
}
```

