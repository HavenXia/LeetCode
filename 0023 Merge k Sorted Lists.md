# 题目地址

https://leetcode.com/problems/merge-k-sorted-lists/



# 题目描述

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

*Merge all the linked-lists into one sorted linked-list and return it.*

 

**Example 1:**

```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```

**Example 2:**

```
Input: lists = []
Output: []
```

**Example 3:**

```
Input: lists = [[]]
Output: []
```



# 思路

### 非递归二分法

之前做过[如何merge两个linked list](/Users/parallax/Documents/求职/LeetCode/0021 Merge Two Sorted Lists.md), 把它和二分结合运用即可

需要用`ArrayList<ListNode>` 来存储

+ 如果list.length为偶数, 那就直接 `for (i = 0; i < length; i += 2)`, `merge (list.get(i), list.get(i+1))`

  <font color = grape>**注意: 这里接下来是不能用remove的, arraylist的remove会彻底搞乱index**</font> 

  所以此处是在每次循环后更新length这个int的值,来做的, 每次merge之后, <font color = grape>**用`result.set(i/2, newNode)` 来修改result**</font>

  即使这样会导致result后半部分也有值, 但是由于length的限制并不会取到后面! 很巧妙



+ 如果list.length为奇数, 那就  `for (i = 0; i < length ; i++)`, 但是要加一句 `if (i == length - 1),` <font color = grape>**直接`result.set(i/2, result.get(i))`**</font>

  因为这是最后一位, 不用参与合并了

+ merge的function 要用到一个dummy node来做

### Complexity

每轮的所有merge加起来一共是O(n), 类似于quick sort那样,然后k个node一共merge了logk次

时间复杂度O(nlogk)

空间复杂度O(n)



# 题解

### 非递归二分法

```java
public ListNode mergeKLists(ListNode[] lists) {

    if (lists == null || lists.length == 0 || (lists.length == 1 && lists[0] == null)) {
        return null;
    }

    int len = lists.length;
    ArrayList<ListNode> result = new ArrayList<ListNode>();
    // initialize
    for (int i = 0; i < lists.length; i++) {
        result.add(lists[i]);
    }

    // loop
    while (len != 1) {

        // even
        if (len % 2 == 0) {
            for (int j = 0; j <len; j+=2) {
                ListNode newNode = merge(result.get(j), result.get(j+1));
                result.set(j/2, newNode);
            }

            len = len / 2;
        } else if (len % 2 == 1) {
            for (int j = 0; j < len; j+=2) {
                // the last one
                if (j == len - 1) {
                    result.set(j/2, result.get(j));
                    continue;
                }

                ListNode newNode = merge(result.get(j), result.get(j+1));
                result.set(j/2, newNode);
            }
            len = len/2 + 1;
        }
    }

    return result.get(0);
}


public ListNode merge(ListNode a, ListNode b) {

    if (a == null) {
        return b;
    }
    if (b == null) {
        return a;
    }


    ListNode dummy = new ListNode(-1);
    ListNode cur = dummy;

    while (a != null && b != null) {

        if (a.val < b.val) {
            cur.next = a;
            a = a.next;
        } else {
            cur.next = b;
            b= b.next;
        }
        cur = cur.next;
    }

    // add rest
    if (a == null) {
        cur.next = b;
    } else {
        cur.next = a;
    }

    return dummy.next;
}
```







