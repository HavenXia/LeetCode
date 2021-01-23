# 题目地址

https://leetcode.com/problems/sort-list/



# 思路

<font color = grape>**O(nlogn) sort 链表 == merge sort, quick sort的partition步骤需要access random index, 链表无法O(1)完成这个操作**</font>

按照普通的merge sort

+ 首先用类似[0876 Middle of the LinkedList](0876 Middle of the LinkedList.md)的function找到<font color = grape>**middlePrev,断开它, 然后返回middle**</font>, O(n)

  要做到这一点, 只要在最开始让slow少走一步就可以, 比如`cur = (cur == null) ? head : cur.next;`

+ 然后分别对两个node, 即current head 和middle再call一次function, <font color = grape>**直到single node, 即`head.next == null` 作为出口**</font>

+ 而模板是分治法, ListNode left 原head, ListNode right 是原middle, 然后用 [0021 Merge Two Sorted Lists](0021 Merge Two Sorted Lists.md)的方法就可以进行merge

  <font color = grape>**注意这里是要return head的!**</font> 

+ 实际上考察了两道其他题目和分治法, 值得复习

时间复杂度O(nlogn), 空间复杂度O(logn)

```java
public ListNode sortList(ListNode head) {

    // edge case
    if (head == null) {return null;}

    return mergeSortHelper(head);
}

private ListNode mergeSortHelper(ListNode head) {
    // recursion ends
    if (head.next == null) {return head;}

    ListNode mid = getMiddle(head);
    ListNode leftHead = mergeSortHelper(head);
    ListNode rightHead = mergeSortHelper(mid);
    return merge(leftHead, rightHead);
}

private ListNode getMiddle (ListNode head) {

    if(head == null) {return null;}
    ListNode midPrev = null;
    while (head != null && head.next != null) {
        midPrev = midPrev == null ? head: midPrev.next;
        head = head.next.next;
    }
    ListNode mid = midPrev.next;
    midPrev.next = null;
    return mid;
}

private ListNode merge (ListNode left, ListNode right) {

    ListNode dummy = new ListNode(-1);
    ListNode cur = dummy;
    while (left != null && right != null) {
        if (left.val < right.val) {
            cur.next = left;
            cur = cur.next;
            left = left.next;
        } else {
            cur.next = right;
            cur = cur.next;
            right = right.next;
        }
    }
    // need append rest nodes
    cur.next = (left == null) ? right: left;
    return dummy.next;
}
```

