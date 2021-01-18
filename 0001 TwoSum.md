# 题目地址

https://leetcode.com/problems/two-sum/



# 思路

#### 排序+双指针

首先用对数组进行sort, 复杂度为O(nlogn), <font color = gree>注意这里并没有说是否每个integer都是unique的!</font>

接着双指针遍历, 如果 start + end > target, 那么<font color = grape>**必须end--**</font>; 如果start + end < target, <font color = grape>**必须start++**</font>

但是! 因为需要返回原数组的index, 所以这道题需要用HashMap来存储. 

但是! 因为每个integer不一定是unqiue的, 所以也不能直接用`<Integer, Integer>` 来存储, 必须自己写一个Pair class并且用Pair[] 来做排序

时间复杂度最后为O(nlogn), 并不是特别好



### HashTable

#### Two-Pass

首先把所有的nums和对应的index加入到HashMap中, O(n)

然后遍历, 检查`map.containsKey(target - nums[i])`, <font color = grape>**如果存在, 还需要检查 当前的对应index和这个数是否是同一个, 如果是就continue!**</font> 

<font color = grape>**注意: HashMap遇到相同的key会自动覆盖掉前一个, 所以如果有多个num相同,那么Hashmap里存的是最后一个num的index, 但是我们的traverse是从0开始的, 此时如果target-num1 = num2. 那么返回的两个index就是第一个num1和最后一个num2的index. 但是这道题是只有exactly one solution的, 所以不用担心中间还有其他的num! **</font>  当num1 = nums2时同理, 因为exact, 所以不会有问题

时间复杂度O(n)

```java
public int[] twoSum(int[] nums, int target) {

    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        map.put(nums[i], i);
    }
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement) && map.get(complement) != i) {
            return new int[] { i, map.get(complement) };
        }
    }
    return new int[2];
}
```

#### One-Pass

因为数组没有被排序过, 所以就直接从0开始往HashMap里放

<font color = grape>**每次都是先检测map是否contains(target - nums[i]), 此时nums[i]还没有加进去, 必然是在之前的部分中查找, 这样就算target- num1 = num2, 查到的也是在i之前的最后一个num2! 不过这里是exactly one solution, 所以前一个num2必然是唯一的num2!**</font> 当num1 = nums2时同理, 因为exact, 所以不会有问题

时间复杂度O(n)

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {

        if (map.containsKey(target - nums[i])) {
            return new int[]{map.get(target - nums[i]), i};
        }     
        map.put(nums[i], i); 
    }
    return new int[2];
}
```















