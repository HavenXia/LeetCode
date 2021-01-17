# 题目地址

https://leetcode.com/problems/jump-game-ii/



# 思路

和0055 很相似, 不过0055只需要可行性,这里需要count所有的方法, <font color = grape>**已经默认一定能走到last index了!**</font>

+ Status: dp[i]代表能够到index 为i的minimum jumps, ==用dp[i]= -1来代表走不到index i==

+ Initialize: `dp[0] = 0 `, 初始index必然0步, 这样dp[1]就需要`dp[0] != -1 && 0 + nums[0] >= 1`

+ Function: 对于dp[i],  for (int j = 0; j < i; j++), <font color = grape>**判断( j + nums[j] >= i && dp[j] != -1) , then min = Math.min(min, dp[j] + 1)**</font> 

  注意,是要在循环中判断出最小的min才行, 如果整个循环结束, min还是Integer.MAX_VALUE, 那dp[i]就更新为-1, 反之则为min

+ Answer: dp[n-1]

思路是对的,但是在这道题里<font color = red>超时了!</font>

```java
public int jump(int[] nums) {
    if (nums == null || nums.length == 0) {
        return -1;
    }
    int n = nums.length;
    int[] dp = new int[n];
    //initialize
    dp[0] = 0;
    for (int i = 1; i < n; i++) {

        int min = Integer.MAX_VALUE;
        for (int j = 0; j < i; j++) {
            if (j + nums[j] >= i && dp[j] != -1) {
                min = Math.min(min, dp[j] + 1);
            }
        }
        if (min == Integer.MAX_VALUE) {
            dp[i] = -1;
        } else {
            dp[i] = min;
        }
    }
    return dp[n - 1];
}
```



#### Greedy

Greedy能够把复杂度降低到O(n)!

<font color = grape>**首先明确, 如果能走到last index, 那必然能走到中间所有的点, 否则走不到某个点,自然就跨不过那个index, 所以到不了last index!**</font>

此时依旧需要maxPos: 代表当前index之前所有index作为starting position能到的最远距离, ==这里不会再出现maxPos < i了!==

直接用`maxPos = Math.max(maxPos, i + nums[i]);` 更新即可

同时需要 **curEnd:** 代表当前这一jump可以到达的最远距离, <font color = red>当index i走到curEnd时, 此时的maxPos就是i(= curEnd) 之前所有index起跳能到达的Pos</font>

所以实际上它是个决策点, 当走过他(i > curEnd)的时候, 直接`curEnd = maxPos` 并且 `jump++`

<font color = gree>注意: 这里不用i >= curEnd是为了避免最后一位的时候多跳一下. 但是用 i> curEnd, 必须先更新curEnd, 以防出现maxPos其实是curEnd + 1作为start 跳到的</font>

时间复杂度O(n), 空间复杂度O(1)





# 题解

```java
public int jump(int[] nums) {

    int maxPos = nums[0];
    int curEnd = nums[0];
    int jump = 1;
    int n = nums.length;
    if (n < 2) { return 0;}

    for (int i = 1; i < n; i++) {

        if (i > curEnd) {
            curEnd = maxPos;
            jump++;      
        } 
        maxPos = Math.max(maxPos, i + nums[i]);
    }
    return jump;
}
```













