用一个maxHeap和一个minHeap, 实现logn add和O(1) find
maxHeap装sorted array的右半部分
minHeap装左半部分
实现偶数个的时候, maxheap和minheap各一半, 奇数个的时候直接maxHeap 顶

如何实现?
数据进来的时候, 先进maxHeap,和前半段比,然后取出最大值放入minHeap和后半段比
<font color = grape>**确保此时前半段一定小于后半段!**</font>
但是, 为了方便find median, 此时要check总数, 如果odd就从minHeap取出最小的放回前半段当最大!

为什么不是even的时候? 这样就会导致奇数的时候,median在minHeap顶, 个人选择罢了



```java
class MedianFinder {

    private PriorityQueue<Integer> maxHeap;
    private PriorityQueue<Integer> minHeap;
    private int count;

    /** initialize your data structure here. */
    public MedianFinder() {

        maxHeap = new PriorityQueue<Integer>(new Comparator<Integer>() {
            public int compare(Integer a, Integer b) {
                return b - a;
            } 
        });
        minHeap = new PriorityQueue<>();
        count = 0;

    }

    public void addNum(int num) {

        count++;
        // maxHeap first, minHeap then
        maxHeap.offer(num);
        minHeap.offer(maxHeap.poll());
        // check if put it back to first half
        if (count % 2 == 1) {
            maxHeap.offer(minHeap.poll());
        }
    }

    public double findMedian() {
        // if odd, take from maxHeap
        if (count % 2 == 1) {
            return (double) maxHeap.peek();
        } else {
            return (double) (maxHeap.peek() + minHeap.peek()) / 2.0;
        }
    }
}

```

