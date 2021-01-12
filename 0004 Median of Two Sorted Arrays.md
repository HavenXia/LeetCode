# 题目地址

https://leetcode.com/problems/median-of-two-sorted-arrays/



# 题目描述

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

**Follow up:** The overall run time complexity should be `O(log (m+n))`.

**Example 1:**

```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```

**Example 2:**

```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```

**Example 3:**

```
Input: nums1 = [0,0], nums2 = [0,0]
Output: 0.00000
```

**Example 4:**

```
Input: nums1 = [], nums2 = [1]
Output: 1.00000
```

**Example 5:**

```
Input: nums1 = [2], nums2 = []
Output: 2.00000
```



# 思路

非常难, 且陷阱很多的一题

曾经做过[Median of Two Sorted Arrays](/Users/parallax/Documents/求职/LintCode/0065 Median of two sorted array.md)

**Edge cases:**

+ one array is empty, return <font color = red>`(nums2[nums2.length / 2] + nums2[(nums2.length - 1) / 2]) / 2.0` </font> , 同时适用于奇数和偶数
+ 如果n1 > n2,  必须进行调换, 返回 `findMedianSortedArrays(nums2, nums1)`

**非递归二分法:**

假设数组AB长度分别为n1, n2, 那么总长度是n1+n2, <font color = grape>但是对于合并数组C而言,index是到n1+n2-1</font>

定义K = (n1 + n2) / 2, <font color = grape>如果C的长度是偶数, k指向的就是右中位数,k-1指向左中位数</font>

​									 <font color = grape>如果C的长度是奇数, k指向的中位数</font>

可以理解为从A中拿出m1个元素(<font color = grape>A[0]到A[m1 - 1]</font>),从B中拿出m2个元素(<font color = grape>B[0]到B[m1 - 1]</font>), m1+m2 = k, 所以这些元素构成了<font color = grape>C[0]到C[k - 1]</font>

需要确保的是, 右半部分始终大于左半部分, 而在本数组中这是必然成立的, 所以只要A和B比, B和A比就可以

<font color = grape>left和right的初始值为0和length, 因为这里m1是代表右半部分的第一个, 所以是有可能整个nums1都在左半边的!</font>

+ 如果A[m1] < B[m2 - 1], <font color = grape>那说明A数组的m1还是选小了, 让left = m1 + 1</font>
+ 如果A[m1 - 1] > B[m2], <font color = grape>那说明A数组的m1选大了, 让right = m1 - 1 </font>

**这里必须用到mid+1或者-1是因为这样才能避免, start下一个就是end的情况下, mid永远保留在start上**

==为什么这里没有限制start和end不会相遇?==

+ 如果start和end在中间相遇了, 必然是直接就得出了答案的mid1!
+ 剩下start和end相遇的情况就是在0和length时, 此时也是可以直接满足的!

时间复杂度O(log(n+n))



# 题解

```java
public double findMedianSortedArrays(int[] nums1, int[] nums2) {
	
    // edge cases
    if (nums1 == null || nums2 == null) {
        return -1;
    }

    if (nums1.length == 0) {
        return (nums2[nums2.length / 2] + nums2[(nums2.length - 1) / 2]) / 2.0;
    }

    if (nums2.length == 0) {
        return (nums1[nums1.length / 2] + nums1[(nums1.length - 1) / 2]) / 2.0;
    }

    if (nums1.length > nums2.length) {
        return findMedianSortedArrays(nums2, nums1);
    }


    int len = nums1.length + nums2.length;
    int start = 0, end = nums1.length;
    int mid1 = 0, mid2 = 0;

    while (mid1 >= 0 && mid1 <= nums1.length) {

        mid1 = start + (end - start) / 2;
        mid2 = len / 2 - mid1;

        double L1 = (mid1 == 0) ? Integer.MIN_VALUE : nums1[mid1 - 1];
        double R1 = (mid1 == nums1.length) ? Integer.MAX_VALUE : nums1[mid1];
        double L2 = (mid2 == 0) ? Integer.MIN_VALUE : nums2[mid2 - 1];
        double R2 = (mid2 == nums2.length) ? Integer.MAX_VALUE : nums2[mid2];

        if (L1 > R2) {
            // mid1 too big
            end = mid1 - 1;
        } else if (L2 > R1) {
            start = mid1 + 1;
        } else {
            if (len % 2 == 0) {
                double leftPart = (L1 < L2) ? L2 : L1;
                double rightPart = (R1 < R2) ? R1 : R2;
                return (leftPart + rightPart) / 2.0;
            } else {
                return (R1 < R2) ? R1 : R2;
            }
        }
    }
    return -1;

}
```

