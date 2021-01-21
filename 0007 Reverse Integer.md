# 题目地址

https://leetcode.com/problems/reverse-integer/



# 思路

首先, java的module result会保持和divident 一样的sign, <font color = grape>**-21 % 10 = -1,  -21 / 10 = -2**</font>

##### 如何Reverse 一个数:

首先求remainder并存到reverse里, 然后更新x = x / 10, 到下个loop,  reverse = reverse * 10 + remainder.

然后再考虑初始情况, 此时先设置rev = 10, 即可adapt to reverse的update function

```java
while (x != 0) {
    int digit = x % 10;
    x /= 10;
    rev = rev*10 + digit;
}
```

但是注意, 这里还要求reverse的value肯定不能超过integer的范围, <font color = grape>**不能直接等reverse算完再比较, reverse本身就是int**</font>

所以reverse的检测必须穿插在loop中, ==不能放在rev求出之后!!! 因为有可能正好到这里x就等于0了, 我们要确保rev能进入下个loop!==

<font color = grape>**总结: out of bound means 当 rev > Integer.MAX_VALUE/10 且 rev还要在进行一次*10的时候才会out of bound!!**</font>

时间复杂度O(logx), 这里是log10(x)



# 题解

```java
public int reverse(int x) {
    int rev = 0;
    while (x != 0) {
        int digit = x % 10;
        x /= 10;
        
        // must compare the reverse result, or it may exceed Integer value bound
        if (rev > Integer.MAX_VALUE / 10 || (rev == Integer.MAX_VALUE / 10 && digit > 7)) {
            return 0;
        }
        if (rev < Integer.MIN_VALUE / 10 || (rev == Integer.MIN_VALUE / 10 && digit < -8)) {
            return 0;
        }
        rev = rev*10 + digit;
    }
    return rev;
}
```

