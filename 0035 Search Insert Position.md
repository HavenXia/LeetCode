# 题目地址

https://leetcode.com/problems/search-insert-position/



# 题目描述

Given a <font color = grape>**sorted array of distinct integers**</font> and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

**Example 1:**

```
Input: nums = [1,3,5,6], target = 5
Output: 2
```

**Example 2:**

```
Input: nums = [1,3,5,6], target = 2
Output: 1
```

**Example 3:**

```
Input: nums = [1,3,5,6], target = 7
Output: 4
```

**Example 4:**

```
Input: nums = [1,3,5,6], target = 0
Output: 0
```

**Example 5:**

```
Input: nums = [1], target = 0
Output: 0
```

 

**Constraints:**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` contains **distinct** values sorted in **ascending** order.
- `-104 <= target <= 104`



# 思路

重点在于**sorted array of distinct integers**

普通二分法
<font color = grape>注意进入之前先检测一下小于最小和大于最大两种情况!</font>

时间复杂度O(logn)



# 题解

```java
public int searchInsert(int[] nums, int target) {

    if (nums == null || nums.length == 0) {
        return 0;
    }
    int start = 0;
    int end = nums.length - 1;
    // check smallest and biggest
    if (nums[start] > target) {
        return 0;
    }
    if (nums[end] < target) {
        return end + 1;
    }
    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (nums[mid] == target) {
            return mid;
        }
        if (nums[mid] < target) {
            start = mid;
        }
        if (nums[mid] > target) {
            end = mid;
        }
    }
    if (nums[start] == target) {
        return start;
    }
    if (nums[end] == target) {
        return end;
    }
    return start + 1;   
}
```

