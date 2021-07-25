最简单就是hashset走一遍, 存在就remove

two-pass做法
很神奇, 但是感觉遇到过!
因为value也是在[1..n]里, 所以可以直接用value当作index来判断
每遇到一个value, 就把index为value - 1的element negative
第二次遍历, 遇到positive的value, 那么index + 1一定是不存在的, 否则就变negative了!



```java
public List<Integer> findDisappearedNumbers(int[] nums) {

    List<Integer> res = new ArrayList<Integer>();
    if (nums == null || nums.length == 0) return res;

    // negate elements at index num - 1
    for (int num: nums) {

        int index = Math.abs(num) - 1;
        if (nums[index] > 0)  nums[index] *= -1;
    }

    // second pass to check which element is positive
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] > 0) {
            res.add(i + 1);
        } 
    }
    return res;
}
```

