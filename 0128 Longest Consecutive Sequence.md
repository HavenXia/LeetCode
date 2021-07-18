用hashset存左边界,然后++一直找到不存在的, 就得到一次长度
并且整体遍历的实际上就是每个num判断了一次, O(n)
如果找到了多个左边界, 那么查找length一共也是O(n)次

注意: 这里的n是set的长度, 而不是arr的长度!!! 差距很大,效果是一样的!!



```java
public int longestConsecutive(int[] nums) {

    if (nums == null || nums.length == 0) {
        return 0;
    }

    Set<Integer> set = new HashSet<Integer>();
    int maxLen = 0;

    // add all into set
    for (int num: nums) {
        set.add(num);
    }


    // find the left bound
    // must run on set!
    for (int num: set) {

        // num is not left bound
        if (set.contains(num - 1)) {
            continue;
        } else {
            // if find, check length
            int len = 0;
            while (set.contains(num++)) {
                len++;
            }
            // update max
            maxLen = Math.max(maxLen, len);
        }

    }
    return maxLen;
}
```

