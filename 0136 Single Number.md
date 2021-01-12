# 题目地址

https://leetcode.com/problems/single-number/



# 思路

#### HashTable

用HashMap存下每个number和其出现次数, 第一个loop全部加进去, 第二个loop查哪个次数为1

时间复杂度O(n),空间复杂度O(n)

#### Bitwise Operation

首先java里的^并不是power, 而是xor函数

<font color = grape>**XOR的特点, a^0 = a, a^a=0, 并且是Commutative(交换律)**</font>

所以每一个数字都xor之后, 相同的都变成0, 最后变成 <font color = grape>**0 xor single number = single number**</font>

<font color = red>时间复杂度O(n), 空间复杂度O(1)</font> 



# 题解

#### HashTable

```java
public int singleNumber(int[] nums) {

    Map<Integer, Integer> map = new HashMap<Integer, Integer>();

    for (int i: nums) {
        if (!map.containsKey(i)) {
            map.put(i, 1);
        } else {
            map.put(i, map.get(i) + 1);
        }
    }
    for (int i: nums) {
        if (map.get(i) == 1) {
            return i;
        }
    }
    return -1;
}
```

#### Bitwise Operation

```java
public int singleNumber(int[] nums) {

    int a = 0;
    for (int i : nums) {
        a ^= i;
    }
    return a;

}
```

