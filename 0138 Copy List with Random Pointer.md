# 题目地址

https://leetcode.com/problems/copy-list-with-random-pointer/



# 思路

看似是复制linked list, 然而实际上是复制graph!

==但凡node有两个及以上的pointer(比如next或者random), 那么就可以看做graph来做!==

<font color = grape>**Clone Graph 需要: 一个queue来存放old node, 一个HashMap来存放old node和new node的pair**</font>

注意每次的检测node和将其加入map中, <font color = gree>且无论是否map里已经有该node了, 都需要设置好random和next的关系</font>

但是这样太慢了!



#### 优化

只需要一个map, <font color = grape>**因为所有的random都point to node in list,**</font>所以第一个loop把所有的node和newNode都加入到map里

然后第二个loop, 用

```java
map.get(cur).next = map.get(cur.next);
map.get(cur).random = map.get(cur.random);
```

就可以轻松把剩下的node都加入!

时间复杂度不变, 空间上少用了一个queue, ==更好!==



# 题解

时间复杂度O(n), 空间复杂度O(n), 但是太慢了!

```java
public Node copyRandomList(Node head) {

    if (head == null) {return null;}

    Queue<Node> queue = new LinkedList<Node>();     
    Map<Node, Node> map = new HashMap<Node, Node>();

    queue.offer(head);
    Node newHead = new Node(head.val);
    map.put(head, newHead);

    while (!queue.isEmpty()) {

        Node cur = queue.poll();

        if (cur.next != null && !map.containsKey(cur.next)) {
            map.put(cur.next, new Node(cur.next.val));
            queue.offer(cur.next);
        }    
        map.get(cur).next = map.get(cur.next);

        if (cur.random != null && !map.containsKey(cur.random)) {
            map.put(cur.random, new Node(cur.random.val));
            queue.offer(cur.random);
        }    
        map.get(cur).random = map.get(cur.random); 

    }
    return newHead;
}
```



#### 优化

```java
public Node copyRandomList(Node head) {

    if (head == null) {return null;}

    Map<Node, Node> map = new HashMap<Node, Node>();

    Node cur = head;
    while (cur != null) {
        map.put(cur, new Node(cur.val));
        cur = cur.next;
    }

    cur = head;
    while (cur != null) {
        map.get(cur).next = map.get(cur.next);
        map.get(cur).random = map.get(cur.random);
        cur = cur.next;
    }
    return map.get(head);  
}
```

