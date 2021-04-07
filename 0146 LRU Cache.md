#  题目地址

https://leetcode.com/problems/lru-cache/



# 思路

+ Capacity 没问题, 可以直接设定
+ get 需要以O(1)提取, 所以必然用到<font color = grape>**HashTable, 同时get要能做到对使用频率更新!**</font> 

+ put同样也是O(1), 但是为了做到<font color = grape>**detect least recently used key, 只有可能用priorityQueue或者linkdedList, 但是pq不能get, 也没法改变pq里面的element位置, 所以只能用linkedList**</font>

#### Get

那么HashTable,为了实现O(1)的get, 需要一个`<key, ListNode>` 的Hash Table, 且value就是key的preNode, 每次get时, 先找到对应的node, 然后

<font color = red>**将其移动到tail, 并且原来的位置delete掉, 然后修改Hashtable中被影响的cur.next(现在对应value为pre)和移动到tail后面的cur(对应value为tail)**</font>

#### Put

+ 如果<font color = grape>**HashTable里已经有这个key, 直接把对应node move to tail, 然后修改node的value, HashTable不变**</font>
+ 如果size小于capacity, <font color = grape>**create new node, 接在tail后面, 把<key, tail>放入HashTable中**</font>

+ 剩下的情况就是size == capacity,<font color = grape>**HashTable直接删除dummy.next.key, 然后在链表里直接修改dummy.next的key和val, 在HashTable中添加<dummy.next.key, dummy>, 最后move to tail即可**</font>

  + 为什么不直接从链表和hashtable都删除掉dummy.next, 然后直接在tail后面接一个? 因为如果capacity = 1, 此时tail已经被删掉了, 无法接在后面, 而此处只删hashtable不删listnode就可以避免这种情况

  

#### 这个考的频率极高,必须背的滚瓜烂熟!

```java
public class LRUCache {
    class ListNode {
        public int key, val;
        public ListNode next;
		
        // 必须设置key, 方便很多操作!
        public ListNode(int key, int val) {
            this.key = key;
            this.val = val;
            this.next = null;
        }
    }
    // capacity 和 size 用来检测是普通的append node, 还是要modify tail
    private int capacity, size;
    // 同时需要dummy 和tail
    private ListNode dummy, tail;
    // key就是每个node的key, prev是当前node的前一个, 方便进行delete操作
    private Map<Integer, ListNode> keyToPrev;


    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.keyToPrev = new HashMap<Integer, ListNode>();
        this.dummy = new ListNode(0, 0);
        this.tail = this.dummy;
    }
	
    private void moveToTail(int key) {
        ListNode prev = keyToPrev.get(key);
        ListNode curt = prev.next;
		// 如果自身是tail, 直接结束了
        if (tail == curt) {
            return;
        }
		// delete 原来的node, 接在tail后面, 记得要设置next为null
        prev.next = prev.next.next;
        tail.next = curt;
        curt.next = null;

        // 受影响的有原位置的后一个,现在其prevNode就是prev
        keyToPrev.put(prev.next.key, prev);
		// 还有受影响的就是自身, prevNode为tail
        keyToPrev.put(curt.key, tail);
		// tail更新为cur
        tail = curt;
    }


    public int get(int key) {
        // 如果不存在, 直接返回-1
        if (!keyToPrev.containsKey(key)) {
            return -1;
        }
		// 然后把用过的move到后
        moveToTail(key);

        // the key has been moved to the end
        return tail.val;
    }


    public void set(int key, int value) {
        // 如果key已经存在了, 先移动到tail, 再修改value, set里面的prevNode是用过moveToTail修改的
        if (keyToPrev.containsKey(key)) {
            moveToTail(key);
            ListNode prev = keyToPrev.get(key);
            prev.next.val = value;
            return;
        }
		// 排除上面的情况后, 如果还没满, 就是普通的接在最后, 记得放入map中
        if (size < capacity) {
            size++;
            ListNode curt = new ListNode(key, value);
            tail.next = curt;
            keyToPrev.put(key, tail);
			// 同样更新tail
            tail = curt;
            return;
        }

        // 到这里就是size = capacity的时候, 每次put都要删掉dummy.next
        ListNode first = dummy.next;
        keyToPrev.remove(first.key);
		// 但是linked list里面可以直接修改key和val, 其他不用改变
        first.key = key;
        first.val = value;
        // 重新put进map, 这么做的原因是这里key是改变的了, 不像第一种情况是key存在的
        keyToPrev.put(key, dummy);
		// 最好也要move to tail
        moveToTail(key);
    }
}
```

