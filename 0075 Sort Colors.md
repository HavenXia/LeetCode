# 题目地址

https://leetcode.com/problems/sort-colors/



# 思路

方法一: just quick sort, 但是时间复杂度O(nlogn), 空间复杂度O(logn) 都太慢了

方法二: One-pass with O(1) space

<font color = grape>**需要在一次遍历中进行修改排序**</font>

需要三个指针, left和cur 起初指向左边, right起初指向末尾

+ 当 `nums[mid] = 1 `时,mid++

+ 当  `nums[mid] > 1 `时,将nums[mid] 和 nums[right]交换,<font color = gree>right左移但mid不动</font>

+ 当 `nums[mid] < 1 `时, 将nums[mid] 和 nums[left]交换, <font color = gree>并且mid和left都右移</font>

  

**前置条件为while (mid <= right),如果用mid<right会导致[1,0,0]变成[0,1,0], 但是无法交换后面的[1,0]**

<font color = red>注意: 这里each round里面的if必须是if else关系, 不然的话会导致连续互换, **和快排可不同!!!**</font> 

时间复杂度O(n)



# 题解

```java
public void sortColors(int[] nums) {

    if (nums == null || nums.length == 0) {
        return;
    }

    int left = 0;
    int cur = left;
    int right = nums.length - 1;

    while (cur <= right) {

        if (nums[cur] == 1) {
            cur++;
        } else if (nums[cur] < 1) {
            int temp = nums[cur];
            nums[cur] = nums[left];
            nums[left] = temp;
            left++;
            cur++;
        } else {
            int temp = nums[cur];
            nums[cur] = nums[right];
            nums[right] = temp;
            right--;
        }

    }       
}
```

