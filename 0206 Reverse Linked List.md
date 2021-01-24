# 题目地址

https://leetcode.com/problems/reverse-linked-list/



# 题目描述

反转一个linkedList

#### Iteratively 迭代

+ 用stack, 存进去即可, 再二次遍历取出, 每次让当前pop的next等于peek即可, <font color = grape>**注意: isEmpty的时候不能peek**</font>

  时间空间复杂度O(n)

+ 不用stack, 用cur遍历linked list, 然后

  先用next获取cur的下一位, 然后让cur.next = prev, 接着prev = cur, 最后cur=next, <font color = grape>**完成了转向, 并且继续保留了next**</font>

  时间复杂度O(n), 空间复杂度O(1)

  ```java
  public ListNode reverseList(ListNode head) {
  
      if (head == null) {return head;}
  
      ListNode cur = head;
      ListNode prev = null;
  
      while (cur != null) {
  
          ListNode next = cur.next;
          cur.next = prev;
          prev = cur;
          cur = next; 
      }
  
      return prev;
  }
  ```

  

#### Recursively 递归

首先递归出口必然是cur = null, 参数必须有当前的cur和prev两个, 然后在operation里面获取cur.next, 传入下一个dfs

<font color = grape>**但是注意递归出口时, 不能直接返回cur, 而是应该返回此时的prev, 也就是reverseList的head!!**</font>

时间复杂度O(n), 空间复杂度O(n), 空间复杂度来自递归

```java
public ListNode reverseList(ListNode head) {

    if (head == null) {return head;}

    ListNode prev = null;
    return dfs(head, prev);
}

private ListNode dfs(ListNode cur, ListNode prev) {

    if (cur == null) {return prev;}

    ListNode next = cur.next;
    cur.next = prev;
    prev = cur;
    return dfs(next, prev);

}
```





