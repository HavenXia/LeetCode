# 题目地址

https://leetcode.com/problems/middle-of-the-linked-list/



## 思路

用快慢指针走即可

#### 方法一: 不用dummy node

slow和fast都直接等于head, 然后slow走一步fast走两步

+ odd, 比如三个, slow正好在middle
+ even, 比如四个, slow正好在second middle

```java
public ListNode middleNode(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}
```

#### 方法二: 用dummy node

slow和fast都等于dummy, 然后slow走一步fast走两步, 这样会导致even的时候, 走到first middle

 <font color = grape>**所以while只检查fast本身, 到slow走了之后再检查fast.next**</font>

```java
public ListNode middleNode(ListNode head) {

    if (head == null) {return head;}
    ListNode dummy = new ListNode(-1);
    dummy.next = head;
    ListNode slow = dummy;
    ListNode fast = dummy;

    while (fast != null) {

        slow = slow.next;
        if (fast.next != null) {
            fast = fast.next.next;
        } else {
            break;
        }
    }
    return slow;

}
```



