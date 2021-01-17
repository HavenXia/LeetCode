# 题目地址

https://leetcode.com/problems/jump-game/



# 思路

可行性的DP

+ Status: dp[i]代表能够到index 为i的可行性

+ Initialize: `dp[0] = true`, 初始index必然是可行的

+ Function: 对于dp[i],  for (int j = 0; j < i; j++), <font color = grape>**判断( j + nums[j] >= i ) **</font> , 只要有一个j是满足的, 那么dp[i]就是true, 反之false

  <font color = red>为什么这里不要判断 dp[j] == true? 因为如果到不了index j, 那说明怎么都跨不过这个position, 更不可能到last index了!</font>

+ Answer: dp[n-1]

时间复杂度O(n<sup>2</sup>), 空间复杂度O(n)



#### Greedy贪心

<font color = grape>**首先明确, 如果能走到last index, 那必然能走到中间所有的点, 否则就跨不过那个index!**</font>

用maxPosition代表: 

==the maximum position that one could reach starting from the previous cells, 到当前index之前所有index能到达的position的最大值!==

如果maxPos小于i本身, 那说明走不到i了, 即然跨不过i, 自然也跨不过lastPos, <font color = grape>**以这个作为false跳出的条件, 如果能一直走到lastindex, 那就true了!**</font>

只要有走不到的点就直接跳出, 如果能出forloop那必然就能走到last index了

```java
public boolean canJump(int[] nums) {
    int n = nums.length;

    // max position one could reach 
    // starting from index <= i
    int maxPos = nums[0];

    for (int i = 1; i < n; ++i) {
        // if one could't reach this point
        if (maxPos < i) {
            return false;
        }
        maxPos = Math.max(maxPos, nums[i] + i);
    }
    return true;
}
```

这样的话时间复杂度只有O(n) !



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

