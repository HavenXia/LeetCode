# 题目地址

https://leetcode.com/problems/linked-list-cycle-ii/



# 思路

类似[0141 Linked List Cycle](0141 Linked List Cycle.md)

第一种方法就是用HashTable, 第一次出现重复的node的时候直接return Node即可

时间复杂度O(n), 空间复杂度O(n),<font color = grape>**需要把空间复杂度优化到O(1)**</font>

#### 双指针(Floyd's Tortoise and Hare)

首先用0141相同的办法, left走一步, right走两步, 直到相遇, <font color = grape>**但是这里right最好不要先走一步, 方便计算**</font> 

不过这样就要单独排除 仅有一个head的情况, 此时right不先走的话, 是能够正常进入loop的!

<img src="/Users/parallax/Library/Application Support/typora-user-images/image-20210122151047658.png" alt="image-20210122151047658" style="zoom:67%;" align="left"/>

<font color = grape>**注意: 此时 F + a是left指针走的路, 2(F+a)是right走的路**</font>

而它们相会, 说明 ==2(F + a) - (F + a) = F + a = nC==, 这里nC是因为, 如果cycle is very small, left进入cycle的时候, right可能已经在里面走了很多遍了

此时记录下left or right, 就是在F + a的位置, <font color = grape>**已经知道F = nC - a, 所以left再走F部, 必然等同于back a步, 直接回到result node**</font>

**但是! 我们只知道F + a, 并不知道具体F和a是多少, 怎么办?**

用一个cur指向head, 然后cur和left同时走, <font color = grape>**走F步的时候, cur和left直接相会! 此处就是pos node!**</font>

时间复杂度O(n), 空间复杂度O(1)



```java
public ListNode detectCycle(ListNode head) {

    if (head == null) {return head;}
    ListNode left = head;
    ListNode right = head;
    // edge of single head node
    if (right.next == null) {return null;}
	
    // check for first meet
    while (right != null) {

        // if left not meet right
        left = left.next;
        if (right != null && right.next != null) { 
            right = right.next.next;
        } else {
            return null;
        }
		
        if (left == right) {
            // then left have F step to go
            ListNode cur = head;
            // left = left.next;
            while (cur != left) {
                cur = cur.next;
                left = left.next;
            }
            return cur;
        }
    }
    return null;
}
```

