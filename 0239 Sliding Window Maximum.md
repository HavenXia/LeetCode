用heap可以做, 但是heap的话就是nlogk, 因为max heap的操作都是logk

用deque做, 必须确保queue中一直是decreasing的, 这样每次加入的时候,
先检测下当前最左边的(最大的)是不是要被删去, 接着从右边开始删掉比nums[i]小的
这一套结束之后, 这个window的max=queue.getFirst()



```java
public int[] maxSlidingWindow(int[] nums, int k) {

    // edge case
    if (k > nums.length) {
        return null;
    }
    // k = 1 special case
    if (k == 1) {
        return nums;
    }

    // store the index of nums
    Deque<Integer> deque = new ArrayDeque<Integer>();
    int[] res = new int[nums.length - k + 1];

    // need to keep it decreasing
    for (int i = 0; i < k; i++) {
        // delete from right until meet larger then nums[i]
        while (!deque.isEmpty() && nums[deque.getLast()] < nums[i]) {
            deque.removeLast();
        }
        deque.addLast(i);
    }

    // put first
    res[0] = nums[deque.getFirst()];

    // begin to update 
    for (int i = k; i < nums.length; i++) {

        // check if need to delete biggest
        // here must not be empty
        // at least have current one in queue
        if (deque.getFirst() == i - k) {
            deque.removeFirst();
        }
        // delete from right until meet larger then nums[i]
        while (!deque.isEmpty() && nums[deque.getLast()] < nums[i]) {
            deque.removeLast();
        }
        deque.addLast(i);
        res[i - k + 1] = nums[deque.getFirst()];
    }

    return res;
}
```

