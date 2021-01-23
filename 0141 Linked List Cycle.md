# 题目地址

https://leetcode.com/problems/linked-list-cycle/



# 思路

#### HashTable

相当简单, 直接遍历check, 如果在set里就return true, 不在就加入set

时间复杂度O(n), 空间复杂度O(n),<font color = grape>**需要把空间复杂度优化到O(1)**</font>

#### 双指针

用一个left和一个right, left每次走到下一个, right每次走两个, <font color = grape>**那么每次right和left的差距就会+1**</font>

+ 如果right走到了null, 那么必然是false的!
+ 如果left和right `==`了, 那么必然是说明存在了cycle! ==这里用连等号是因为left和right指向同一个reference==
+ <font color = grape>**同时, left和right的初始值都是head, 但是edge case 存在one single node, 所以需要检测一下**</font>

时间复杂度O(n), 空间复杂度O(1)

```java
public boolean hasCycle(ListNode head) {
	
    if (head == null) {return false;}
    ListNode left = head;
    ListNode right = head;
	// edge case: one single node
    if (head.next == null) {return false;}

    while (right != null) {

        left = left.next;
        // 直接走两步
        if (right != null && right.next != null) { 
            right = right.next.next;
        } else {
            return false;
        }
        if (left == right) {return true;}
    }
    return false;
}
```



