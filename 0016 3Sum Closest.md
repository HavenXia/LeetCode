# 题目地址

https://leetcode.com/problems/3sum-closest/



# 题目描述

Given an array `nums` of *n* integers and an integer `target`, find three integers in `nums` such that the sum is closest to `target`. Return the sum of the three integers. You may assume that each input would have exactly one solution.

 

**Example 1:**

```
Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

 

**Constraints:**

- `3 <= nums.length <= 10^3`
- `-10^3 <= nums[i] <= 10^3`
- `-10^4 <= target <= 10^4`



# 思路

按照普通的3sum思路即可, 用一个for 循环套一个twoSum来做

但是因为返回的是<font color = red>sum</font>,所以如果要用专门的twoSum method的话,需要把diff再传出去

**注意:**

+ diff 和 closest都需要设置一个默认值
+ 如果想用 `sum = nums[start] + nums[end]` ,这句一定得在while里面,才会在每次循环时都变



**还有一种不套路的做法就是不用子函数直接写完, 很方便**



# 题解

时间复杂度O(n<sup>2</sup>), 空间复杂度O(1)

```java
public int threeSumClosest(int[] nums, int target) {
    // null case
    if (nums == null || nums.length == 0) {
        return -1;
    }

    Arrays.sort(nums);

    int diff = Integer.MAX_VALUE;
    int closest = nums[0] + nums[1] + nums[nums.length - 1];

    for (int i = 0; i < nums.length - 2; i++) {
        
        int sum = nums[i] + twoSumClosest(nums, i , diff, target - nums[i]);       
        if (diff > Math.abs(target - sum)) {
            diff = Math.abs(target - sum);           
            closest = sum;   
        }
    }
    return closest;  
    

private int twoSumClosest(int[] nums, int i, int diff, int target) {
        
    int start = i + 1, end = nums.length - 1;
    int closestSum = nums[start] + nums[end];

    while (start < end) {

        int sum = nums[start] + nums[end];
        if (sum == target) {
            return sum;
        } 
        if (nums[start] + nums[end] > target) {
            end--;
        }
        if (sum < target) {
            start++;
        }
        if (diff > Math.abs(target - sum)){
            diff = Math.abs(target - sum);
            closestSum = sum;
        }
    }
    return closestSum;
}
```



### 不用子函数,直接写完, 可以快一点

```java
public int threeSumClosest(int[] nums, int target) {
    // null case
    if (nums == null || nums.length == 0) {
        return -1;
    }

    Arrays.sort(nums);

    int result = nums[0] + nums[1] + nums[nums.length - 1];

    for (int i = 0; i < nums.length - 2; i++) {

        int start = i + 1, end = nums.length - 1;

        while (start < end) {

            int sum = nums[i] + nums[start] + nums[end];

            if (sum > target) {
                end--;
            } else if (sum < target) {
                start++;
            } else {
                return sum;
            }

            if (Math.abs(sum - target) < Math.abs(result - target)) {
                result = sum;
            }
        }
    }
    return result;
}
```



