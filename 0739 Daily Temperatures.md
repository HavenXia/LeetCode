维护一个单调栈, 不过存的是index, **注意栈中一定decreasing**
在loop中, 如果当前的nums[i] > stack.peek()
那就取出peak的index, 然后res[index] = i - index
注意这里要用while来包, 因为遇到一个大的之后可能连着解决几个的问题
在这之后才把i放进去

单调栈的题目, 还是要重温一遍!



```java
public int[] dailyTemperatures(int[] T) {

    if (T == null || T.length == 0) return null;
    int[] res = new int[T.length];
    // use stack to store indexes
    // all temperatures in stack must be decreasing
    Deque<Integer> stack = new ArrayDeque<Integer>();

    for (int i = 0; i < T.length; i++) {

        // once find larger, remove all smaller then it in stack
        // to ensure the stack is decreasing on T[i]
        while (!stack.isEmpty() && T[i] > T[stack.peek()]) {
            int index = stack.pop();
            res[index] = i - index;
        }
        // always push after poping
        stack.push(i);
    }
    // all that cannot find larger will keep 0
    return res;
}
```

