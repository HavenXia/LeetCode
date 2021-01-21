# 题目地址

https://leetcode.com/problems/remove-nth-node-from-end-of-list/



# 思路

注意dummy node在这道题非常方便, 因为我们的curLeft总是要停在toDelete的前一位, 所以dummy node可以解决删除head的问题

#### Two-Pass

首先遍历找出length, 然后length - n得到 <font color = grape>**curLeft应该停在(length - n) th node, 又因为我们从dummy开始, 所以走对应步数就可以了**</font>

时间复杂度O(n), 空间复杂度O(1)

```java
public ListNode removeNthFromEnd(ListNode head, int n) {

    ListNode dummy = new ListNode(0);
    dummy.next = head;
    int length  = 0;
    ListNode first = head;
    while (first != null) {
        length++;
        first = first.next;
    }
    length -= n;
    first = dummy;
    while (length > 0) {
        length--;
        first = first.next;
    }
    first.next = first.next.next;
    return dummy.next;
}
```



#### One-Pass(本质上是twoPointer)

两个cur都从dummy起步, 先让curRight移动到curLeft前n位, <font color = grape>**这里就算n=length, 因为是从dummy起步也可以正好停在最后一个node**</font>

然后检测 `curRight.next != null`  来让两个pointer前进, 如果到了最后一个node, 此时curLeft就是toDelete前一位!

时间复杂度O(n), 空间复杂度O(1)

```java
public ListNode removeNthFromEnd(ListNode head, int n) {

    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode curLeft = dummy;
    ListNode curRight = dummy;

    for (int i = 0; i < n; i++) {
        curRight = curRight.next;
    }
    while(curRight.next != null) {
        curRight = curRight.next;
        curLeft = curLeft.next;
    }
    curLeft.next = curLeft.next.next;
    return dummy.next; 
}
```



