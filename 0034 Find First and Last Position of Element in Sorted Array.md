# 题目地址

https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/



# 题目描述

Given an array of integers `nums` sorted in ascending order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

**Follow up:** Could you write an algorithm with `O(log n)` runtime complexity?

 

**Example 1:**

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2:**

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

**Example 3:**

```
Input: nums = [], target = 0
Output: [-1,-1]
```



# 思路

目前的想法是用两次二分

+ 首先是查找first position, 依旧是二分法求mid
  + 如果mid > target, end = mid
  + 如果mid < target, start = mid
  + <font color = grape>如果mid=target, 也是end= mid, 因为要确保找到第一个!</font> 

  出循环后, 先检测start再检测end

+ 接着是查找last position, 依旧是二分法求mid

  + 如果mid > target, end = mid
  + 如果mid < target, start = mid
  + <font color = grape>如果mid=target, 也是start = mid, 因为要确保找到最好一个!</font> 

  出循环后, 先检测end再检测start

<font color = grape>**最后存入result中**</font> 

时间复杂度O(logn)



# 题解

```java
public int[] searchRange(int[] nums, int target) {

    int[] result = new int[2];
    result[0] = -1;
    result[1] = -1;
    if (nums == null || nums.length == 0) {
        return result;
    }

    int start = 0, end = nums.length - 1;
    int mid;

    // search first
    while (start + 1 < end) {
        mid = start + (end - start) / 2;
        if (nums[mid] < target) {
            start = mid;
        } else {
            end = mid;
        }
    }
    if (nums[start] == target) {
        result[0] = start;
    } else if (nums[end] == target) {
        result[0] = end;
    } else {
        return result;
    }

    // search last
    start = 0;
    end = nums.length - 1;
    while (start + 1 < end) {
        mid = start + (end - start) / 2;
        if (nums[mid] > target) {
            end = mid;
        } else {
            start = mid;
        }
    }
    if (nums[end] == target) {
        result[1] = end;
    } else if (nums[start] == target) {
        result[1] = start;
    }

    return result;

}
```

