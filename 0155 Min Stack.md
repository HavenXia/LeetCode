# 题目地址

https://leetcode.com/problems/min-stack/



# 思路

#### LinkedList & Stack

简单的实现一个stack的一些功能, 这里选择用linkedList来实现即可

<font color = grape>**注意, 往stack上放东西, 在这里并不能直接往后放tail后面, 因为这样会无法实现pop(), 所以实际上是反着来的, stack 的top对应的是dummy.next!**</font> 

此处的stack是`<int[]>` , 存的是一个size 为2的array, 第一个是Node.val, 第二个是minSoFar

所以每次push新value时, 直接取stack.peek()[1] 就得到了minSoFar, 然后和当前val比较并放进去即可

每次pop的时候需要删掉node, 并且stack pop掉最顶上一层, <font color = grape>**这样取min的时候就直接peek就可以了**</font>

每个操作都是O(1)

#### 优化: One Stack

实际上可以直接简化为一个`<int[]>` 的stack, 每次放入value和minSoFar即可

pop和top按照pop和peek来做, <font color = grape>**getMin就是返回 stack.peek()[1]**</font> 

#### 优化: Two Stack

用两个stack也可以, 一个stack按照普通的放入value, <font color = grape>**第二个stack存放min**</font>

当push的新value **小于 minStack.peek()的时候, 就给minStack 也push这个value!** ==只有出现current value < minSoFar的时候, 才更新minStack==

这样每次getMin就直接peek minStack即可, 当pop的时候, <font color = grape>**如果stack里pop的value == minStack的peek, 那么minStack也pop掉!**</font>

<img src="/Users/parallax/Library/Application Support/typora-user-images/image-20210123174741261.png" alt="image-20210123174741261" style="zoom:80%;" align="left"/>



# 题解

LinkedList & Stack, 其他的去leetcode看

```java
class MinStack {

    private ListNode dummy;
    private ListNode tail;
    private Stack<int[]> mins;

    /** initialize your data structure here. */
    public MinStack() {      
        this.dummy = new ListNode(-1);
        this.tail = dummy;
        this.mins = new Stack<int[]>();
    }

    public void push(int x) {

        ListNode cur = new ListNode(x);
        cur.next = this.dummy.next;
        dummy.next = cur;

        if (mins.isEmpty()) {
            mins.push(new int[]{x, x});
            return;
        }
        int curMin = mins.peek()[1];
        this.mins.push(new int[]{x, Math.min(x, curMin)});
    }

    public void pop() {

        this.dummy.next = this.dummy.next.next;
        this.mins.pop();
    }

    public int top() {

        return this.dummy.next.val;
    }

    public int getMin() {

        return this.mins.peek()[1];
    }
}
```

