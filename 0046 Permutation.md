# 题目地址

https://leetcode.com/problems/permutations/



# 思路

非常经典的DFS

用一个==visited数组==储存是否用过某个数字的信息

然后用一个 `List<List<Integers>>` 存result

如果没有访问过, 就继续dfs, 但是记得**backtracking**, <font color = grape>**返回之后一定要list.remove和used修改为false**</font>

```java
list.add(nums[i]);
used[i] = true;
dfs(nums, used, list, result);
list.remove(list.size() - 1);
used[i] = false;
```

时间复杂度和空间复杂度都是O(n * n!)



# 题解

```java
public List<List<Integer>> permute(int[] nums) {

    boolean[] used = new boolean[nums.length];
    List<List<Integer>> result = new ArrayList<List<Integer>>();
    List<Integer> list = new ArrayList<Integer>();

    dfs(nums, used, list, result);
    return result;
}

private void dfs(int[] nums, boolean[] used, List<Integer> list, List<List<Integer>> result) {

    if (list.size() == nums.length) {
        result.add(new ArrayList(list));
        return;
    }


    for (int i = 0; i < nums.length; i++) {
        if (used[i]) {
            continue;
        }
        list.add(nums[i]);
        used[i] = true;
        dfs(nums, used, list, result);
        list.remove(list.size() - 1);
        used[i] = false;
    }
    return;
}
```

