普通的dfs, 需要验证 nums[i] >= nums[start]
但是重点在于去重: 4,7,7 只能出现一次4,7 和4,7,7
在每次dfs中都initialize一个set
每次dfs的loop中如果出现两次相同的, 第二次就跳过
比如4,第二个7的时候, set里已经有第一个7了!



```java
public List<List<Integer>> findSubsequences(int[] nums) {

    List<List<Integer>> res = new ArrayList<List<Integer>>();
    List<Integer> list = new ArrayList<Integer>();

    dfs(res, list, nums, 0);
    return res;
}

private void dfs(List<List<Integer>> res, List<Integer> list,
                 int[] nums, int start) {

    // no 递归出口, just add to list
    if (list.size() > 1) {
        res.add(new ArrayList<Integer>(list));
    }

    // in current loop, if have same num in set. need to jump it
    Set<Integer> set = new HashSet<Integer>();

    for (int i = start; i < nums.length; i++) {

        if (set.contains(nums[i])) {
            continue;
        }
        // put it into set
        set.add(nums[i]);

        // when start = 0, can always choose
        // ow, start - 1 is the last chosen num
        if (start == 0 || nums[i] >= nums[start - 1]) {
            list.add(nums[i]);
            dfs(res, list, nums, i + 1);
            list.remove(list.size() - 1);
        }
    }
}
```

