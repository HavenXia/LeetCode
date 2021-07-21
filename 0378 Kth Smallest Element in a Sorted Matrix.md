最普通的pq, 是nlogk
接着是用int[]{value, row number,col number}的复杂点的pq, 时间klogn
最后一种是二分查找

首先要知道可以通过从左下角往右上查, 找出<cur的个数
然后二分左上和右下
如果< mid count >= k, 那么 end = mid - 1 (必定小于mid)
如果< mid count < k, 那么 start = mid (大于等于mid)
最后start + 1 = end的时候跳出
count on start & end, 
count on start 一定小于8, 不然不可能是start
end+1 count 一定大于等于8, 不然不会更新end
如果count on end 小于8, 那就renturn end, 反之就return start



```java
public int kthSmallest(int[][] matrix, int k) {

    int m = matrix.length;
    int n = matrix[0].length;

    int start = matrix[0][0];
    int end = matrix[m - 1][n - 1];

    // binary search
    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        // if < mid count > k, answer strictly < mid
        // if < mid count = k, answer strictly < mid
        if (countLess(matrix, mid) >= k) {
            end = mid - 1;
        } else {
            // if < mid count < k, answer may >= mid
            start = mid;
        }
    }

    // countLess(start) must < k, that's how start update
    // countLess(end + 1) must >= k, that's how end update
    if (countLess(matrix, end) < k) {
        return end;
    } else {
        return start;
    }
}

private int countLess(int[][] matrix, int target) {
    int count = 0;
    int i = matrix.length - 1;
    int j = 0;

    // must specify i and j range, then check val
    while (i >= 0 && j < matrix[0].length) {

        if (matrix[i][j] >= target) {
            i--;
        } else {
            count += i + 1;
            j++;
        }
    }
    return count;
}
```

