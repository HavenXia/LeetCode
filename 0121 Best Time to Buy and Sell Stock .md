# 题目地址

https://leetcode.com/problems/best-time-to-buy-and-sell-stock/



# 思路

暴力搜索O(n<sup>2</sup>), 太无脑

One Pass更新, 需要让profit变成实时的!

首先需要设置minPrice = Integer.MAX_VALUE, 然后设置maxProfit = 0

<font color = grape>**接着用一个for loop, 在前进的过程中每走一步, 更新`min=Math.min(min,prices[i]), max=Math.max(prices[i] - min, max)`**</font>

因为是正向前进的, 所以max的值是==当前价格减去so far 最低价, 因此可以在O(n)下完成==



# 题解

```java
public int maxProfit(int[] prices) {
    int min = Integer.MAX_VALUE, max = 0;
    for (int price: prices) {
        min = Math.min(min, price);
        max = Math.max(price - min, max);
    }
    return max;
}
```

