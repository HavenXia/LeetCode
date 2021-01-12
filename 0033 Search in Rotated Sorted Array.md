# 题目地址

https://leetcode.com/problems/search-in-rotated-sorted-array/



# 题目描述

You are given an integer array `nums` sorted in ascending order, and an integer `target`.

Suppose that `nums` is rotated at some pivot unknown to you beforehand (i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

*If `target` is found in the array return its index, otherwise, return `-1`.*

 

**Example 1:**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

**Example 3:**

```
Input: nums = [1], target = 0
Output: -1
```



# 思路

依旧是标准的二分法

相当于有两块上升的array, 需要找到两块里面的target, 是unique的

依旧是start=0, end = length - 1.

首先比较target 和 start的大小

<font color = red>**注意这里每次和target比较的是original array的第一个值, 因此用first = nums[start]来储存, 避免不必要的麻烦**</font> 

+ <font color = grape>如果target >= first, 那么target必然是在左半部分</font> , 接着照常取medium并和target比较
  + 如果mid > end && mid < target, 那么必然是start =mid
  + <font color = grape>反之则是 mid =< end || mid >= target</font>, 都可以直接把end=mid, ==这里重点在于想到把他们拼起来==
+ <font color = grape>如果target <  first, 那么target必然是在右半部分</font> , 接着照常取medium并和target比较
  + 如果mid <= end && mid > target, 那么必然是end =mid
  + <font color = grape>反之则是 mid > end || mid <= target</font>, 都可以直接把start=mid, ==这里重点在于想到把他们拼起来==

总时间复杂度为O(logn)



# 题解

```java
public int search(int[] nums, int target) {

    if (nums == null || nums.length == 0) {
        return -1;
    }

    int start = 0, end = nums.length - 1;
    int first = nums[start];

    while (start + 1 < end) {

        int mid = start + (end - start) / 2;

        if (target >= first) {

            if (nums[mid] >= nums[start] && nums[mid] < target) {
                start = mid;
            } else {
                end = mid;
            }
        } else {

            if (nums[mid] <= nums[end] && nums[mid] > target) {
                end = mid;
            } else {
                start = mid;
            }
        }                
    }

    if (nums[start] == target) {return start;}
    if (nums[end] == target) {return end;}
        
        return -1;
    }
```

