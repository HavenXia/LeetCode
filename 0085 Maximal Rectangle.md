首先这个题可以用dp和monotonic stack来解
`dp[j]`代表当前row, j-th 的number of consecutive 1s on each column
`dp[j] = matrix[i][j] == 1 ? dp[j] + 1 : 0`

然后每个row走完后, 用largest rectange的monotonic 来解
但凡发现下降的, 就remove 当前col, 并且计算一下area, update max



```java
public int maximalRectangle(char[][] matrix) {

    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
        return 0;
    }

    // calculate the height of int
    int[] height = new int[matrix[0].length];
    int maxArea = 0;

    // dp
    for (int i = 0; i < matrix.length; i++) {
        for (int j = 0; j < matrix[0].length; j++) {
            // height at current row
            height[j] = matrix[i][j] == '1' ? height[j] + 1 : 0;
        }
        // use leetcode 84 to solve
        maxArea = Math.max(maxArea, largestRectangleArea(height));
    }
    return maxArea;
}

private int largestRectangleArea(int[] height) {

    // push -1 on stack to avoid empty
    Deque<Integer> stack = new ArrayDeque<Integer>();
    stack.push(-1);
    int maxArea = 0;

    for (int i = 0; i < height.length; i++) {

        while (stack.peek() != -1 && height[stack.peek()] > height[i]) {
            // get current height
            int curHeight = height[stack.pop()];
            int curWidth = i - stack.peek() - 1; 
            maxArea = Math.max(maxArea, curHeight * curWidth);
        }
        // always push the new one
        stack.push(i);
    }

    // but after this, there may still have increasing series
    // like 1, 2, 4, also need to calculate area
    while (stack.peek() != -1) {
        maxArea = Math.max(maxArea, 
                           height[stack.pop()] * (height.length - stack.peek() - 1));
    }
    return maxArea;
}
```

