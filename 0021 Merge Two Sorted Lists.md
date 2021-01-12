# 题目地址

https://leetcode.com/problems/merge-two-sorted-lists/



# 题目描述

Merge two sorted linked lists and return it as a new **sorted** list. The new list should be made by splicing together the nodes of the first two lists.

**Example 1:**

<img src="https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg" alt="img" style="zoom:50%;" align="left"/>

```
Input: l1 = [1,2,4], l2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

**Example 2:**

```
Input: l1 = [], l2 = []
Output: []
```

**Example 3:**

```
Input: l1 = [], l2 = [0]
Output: [0]
```



# 思路

甚至都用不上双指针

只需要一个dummy node遍历即可

listnode的题目还是需要注意一下顺序

每次把node添加到cur之后l1和l2立刻前进

<font color = grape>出循环之后记得添加上剩余的node!</font>

时间复杂度O(m+n)



# 题解

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {

    if (l1 == null) {
        return l2;
    }
    if (l2 == null) {
        return l1;
    }
    ListNode dummy = new ListNode(-1);
    ListNode cur = dummy;
    while (l1 != null && l2 != null) {

        if (l1.val <= l2.val) {
            cur.next = l1;
            l1 = l1.next;
        } else {
            cur.next = l2;
            l2 = l2.next;
        }
        cur = cur.next;   
    }
    if (l1 == null) {
        cur.next = l2;
    } else {
        cur.next = l1;
    }

    return dummy.next;
}
```

