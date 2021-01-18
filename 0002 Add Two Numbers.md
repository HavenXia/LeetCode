# 题目地址

https://leetcode.com/problems/add-two-numbers/



# 思路

需要考虑到进位的事情, 利用到了dummy node

首先用一个新的LinkedList来装结果, 然后用`while(l1 != null || l2 != null)` 来作为循环条件

<font color = grape>**这里不用and是因为就算一边结束了, 但是carry还是有可能一直在往前进!**</font>

考虑到这样, 所以一上来检测l1 和l2是不是null, 如果是的话==直接赋值为0==

接着求当前digit就是 (val之和加上carry) % 10, 然后求carry

接着cur.next 就是这个digit(<font color = red>每次循环刚开始的时候, cur还是上一个node, 根据计算出的digit, cur.next生成, 然后cur=cur.next</font>)

最后更新一下l1和l2即可

**注意:** 最后出循环要检测下carry是不是0, 如果不是0那说明还有一个进位, <font color = grape>**需要再加一个cur.next**</font>

时间复杂度O(m+n)

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {

    if (l1 == null) {return l2;}
    if (l2 == null) {return l1;}
    ListNode dummy = new ListNode(-1);
    ListNode cur = dummy;
    int overflow = 0;

    while (l1 != null || l2 != null) {

        int val1 = l1 == null ? 0 : l1.val;
        int val2 = l2 == null ? 0 : l2.val;

        int digit = (val1 + val2 + overflow) % 10;
        overflow = (val1 + val2 + overflow)  / 10;
        cur.next = new ListNode(digit);


        cur = cur.next;
        if (l1 != null) {l1 = l1.next;}
        if (l2 != null) {l2 = l2.next;}
    }

    if (overflow != 0) {cur.next = new ListNode(overflow);}

    return dummy.next;
}
```

