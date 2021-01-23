# 题目地址

https://leetcode.com/problems/container-with-most-water/



# 思路

其实就是找到 `(j - i) * Math.min(height[i], height[j])` 的最小值!

但肯定不是brute force, O(n<sup>2</sup>)的时间还是太慢了, 可能要用双指针来做

+ 首先设maxArea = -1, 然后算curArea, 替换maxArea
+ <font color = grape>**如果start是短边, 那么移动end只会让area更小! 所以必然是start++**</font>
+ <font color = grape>**如果end是短边, 那么start++会让area更小! 所以必然是end--**</font>
+ 直到start 和 end相会, 返回maxArea

时间复杂度O(n)

```java
public int maxArea(int[] height) {

    if (height == null || height.length == 0) {
        return 0;
    }
    int start = 0, end = height.length - 1;
    int maxArea = 0;

    while (start < end) {
        int curArea = (end - start) * Math.min(height[start], height[end]);
        maxArea = Math.max(maxArea, curArea);
        if (height[start] < height[end]) {
            start++;
        } else {
            end--;
        }
    }
    return maxArea;
}
```

