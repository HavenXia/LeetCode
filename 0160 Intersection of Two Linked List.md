# 题目地址

https://leetcode.com/problems/intersection-of-two-linked-lists/



# 思路

第一个想到的就是HashTable, 第一个走一遍然后全部存进去, 第二个一旦出现contains的话就直接返回

但是这样时间复杂度O(n), 空间复杂度也是O(n)

#### 优化: 快慢指针

思想上还是[0142 Linked List Cycle II](0142 Linked List Cycle II.md) 这种, 只不过这里有点小变化

+ 首先cur1和cur2 一起遍历, 用 <font color = grape>**while (cur1.next != null && cur2.next != null), 有一个到头就直接返回**</font>

+ 然后让没走完的那边继续走, 记录部署, <font color = grape>**这个步数就是cur1和cur2之差**</font>

  这里需要用`ListNode curLong = curA.next == null ? headB: headA;` 这样的判断才能方便的记录下长边短边

+ 接着再从头走一遍, 但是长边先走对应步数, 然后 <font color = grape>**while (cur1 != null && cur2 != null) 向前走, 这里不用next是因为可能在最后一个node相交, 返回intersection**</font>

时间复杂度O(n), 空间复杂度O(1)

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {

    if (headA == null || headB == null) {
        return null;
    }
    ListNode curA = headA;
    ListNode curB = headB;
    
    // first loop
    while (curA.next != null && curB.next != null) {
        curA = curA.next;
        curB = curB.next;
    }
    // find difference
    int diff = 0;
    ListNode curDiff = curA.next == null ? curB: curA;
    ListNode curLong = curA.next == null ? headB: headA;
    ListNode curShort = curA.next == null ? headA: headB;
    while (curDiff.next != null) {
        diff++;
        curDiff = curDiff.next;
    }
    // restart the loop
    for (int i = 0; i < diff; i++) {curLong = curLong.next;}
    while (curLong != null && curShort != null) {
        if (curLong == curShort) {return curLong;}
        curLong = curLong.next;
        curShort = curShort.next;
    }
    return null;
}
```

