# 题目地址

https://leetcode.com/problems/jump-game/



# 思路

可行性的DP

+ Status: dp[i]代表能够到index 为i的可行性
+ Initialize: `dp[0] = true`, 初始index必然是可行的
+ Function: 对于dp[i],  for (int j = 0; j < i; j++), <font color = grape>**判断( j + nums[j] >= i && dp[j] == true) **</font> , 只要有一个j是满足的, 那么dp[i]就是true, 反之false
+ Answer: dp[n-1]

时间复杂度O(n<sup>2</sup>), 空间复杂度O(n)



#### Greedy贪心

<font color = grape>**如果从index i能走到index j, 那么必然就能走到i到j之间的任意一个位置**</font>

所以可以用贪心法, 从后往前走

[具体解析见此](https://leetcode.com/problems/jump-game/solution/)

# 题解

#### DP

```java
public boolean canJump(int[] nums) {

    if (nums == null || nums.length == 0) {
        return false;
    }

    int n = nums.length;
    boolean[] dp = new boolean[n];

    dp[0] = true;

    for (int i = 1; i < n; i++) {     
        for (int j = 0; j < i; j++) {
            if (j + nums[j] >= i && dp[j] == true) {
                dp[i] = true;
                break;
            } 
        }   
    }
    return dp[n - 1];
}
```

