# 题目地址

https://leetcode.com/problems/majority-element/



# 思路

需要找出一个array里的majority(出现次数超过一半的), 但是问题在于需要O(1)的space

#### HashTable: 时间复杂度O(n), 空间复杂度O(n)

#### Sort之后取index n/2: 

无论length是odd even, majority是大是小, 都会在index n/2的位置可以确定下来.

时间复杂度O(nlogn), 空间复杂度O(1)

####  Boyer-Moore Voting Algorithm

算法本身很简单, 首先以nums[0]作为candidate, 然后开始遍历, <font color = grape>**遇到相同的, count++, 遇到不同的, count--**</font>

重点在于 ==**当count回到0的时候, 就把candidate变更为当前的value!**==

+ 在过程中出现candidate变成0, 那么可以理解成 <font color = grape>**从start到current的前一个的所有element都被disgard了, 从current开始, 重新count, 需要注意的是, 被disgard的这一部分一定是length = even, 且上一个candidate在其中一定有一半!**</font>

  <img src="/Users/parallax/Library/Application Support/typora-user-images/image-20210124132800413.png" alt="image-20210124132800413" style="zoom:100%;" align="left"/>

  比如这里走到5点时候, 新的candidate变成5, 前面被disgard了

   如果一共有n=a+b个, a为majority, 那么 a > b, a - x > b - x

+ <font color = grape>**注意: 如果7真的是majority, 那么抛掉相等的majority和minority是不会影响的, 在后续的过程中, 7还是会成为majority**</font>

  但是, 如果7不是majority呢?  <font color = grape>**那么抛掉的majority 是 <= minority的, 它甚至有可能还没出现, 所以在后续的不分离, 它还是majority**</font>

+ 虽然还有点难理解, 但是这样消除下去, 最后一定是candidate为majority的!



# 题解

```java
public int majorityElement(int[] nums) {

    if (nums == null || nums.length == 0) {return -1;}

    int count = 0;
    Integer candidate = null;

    for (int num : nums) {
        if (count == 0) {
            candidate = num;
        }
        count += (num == candidate) ? 1 : -1;
    }

    return candidate;    
}
```

