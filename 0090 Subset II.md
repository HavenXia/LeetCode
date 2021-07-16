和普通的subset差不多, dfs每一步判断是否要当前的element
但是: 
如果上一个element等于当前element, 且上一个not used, 那么当前的可以直接跳过
<font color = grape>**比如[1,2,2,3], 如果选择了第一个2, 那么不选第一个2,只选第二个2的所有情况,都被选择第一个2所涵盖了!**</font>

两种dfs法, 

第一种是每次都把当前的list加进去, 通过nums[i]和nums[i-1]比较剪枝

```java
public List<List<Integer>> subsetsWithDup(int[] nums) {

    List<List<Integer>> res = new ArrayList<List<Integer>>();
    List<Integer> list = new ArrayList<>();

    Arrays.sort(nums);
    // dfs to get the res
    dfs(res, list,  nums, 0);
    return res;
}


private void dfs(List<List<Integer>> res, List<Integer> list,
                 int[] nums, int start) {

    // add to res every time
    res.add(new ArrayList<Integer>(list)); 

    for (int i = start; i < nums.length; i++) {
        // 一定出现在前一个选了当前没选的情况下
        if (i != start && nums[i] == nums[i - 1]) {
            continue;
        }

        //backtrack
        list.add(nums[i]);
        dfs(res, list, nums, i + 1);
        list.remove(list.size() - 1);
    }
}
```





第二种是每次到index=len再加进去, 先不选,然后判断是否要选

```java
public List<List<Integer>> subsetsWithDup(int[] nums) {

    List<List<Integer>> res = new ArrayList<List<Integer>>();
    List<Integer> list = new ArrayList<>();

    Arrays.sort(nums);
    // dfs to get the res
    dfs(res, list,  nums, 0, true);
    return res;
}


private void dfs(List<List<Integer>> res, List<Integer> list,
                 int[] nums, int index, boolean choosePre) {

    // add to res every time
    if (index >= nums.length) {
        res.add(new ArrayList<Integer>(list)); 
        return;
    }


    // not choose first
    dfs(res, list, nums, index + 1, false);

    // check if need to jump
    if (!choosePre && nums[index] == nums[index - 1]) {
        return;
    }

    // else, choose it
    list.add(nums[index]);
    dfs(res, list, nums, index + 1, true);
    list.remove(list.size() - 1);
}
```

