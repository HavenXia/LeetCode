# 题目地址

https://leetcode.com/problems/zigzag-conversion/



# 思路

把 (2 * numRows - 2) 看成一组, <font color = grape>**而每一组的这(2 * numRows - 2)个char可以分到numRows个单独的String中**</font>

+ 如果 `i % round <= numRows - 1`,  可以直接把这个char加入到 `rows[i % round]` 里
+ 如果 `i % round > numRows - 1` , 比如说四行, 那么index 4 就是第五个, 反向的index就是`round - index` , index为2, 反向第三个

<font color = grape>**注意: 这里用StringBuilder 来append char 比用String += substring() 快很多! StringBuilder 可以多个相加(生成result)**</font>

==因为str.substring()实在是很慢!==

时间复杂度O(n), 空间复杂度O(n)

```java
public String convert(String s, int numRows) {

    if (s == null || s.length() == 0) {return null;}
    if (numRows == 1) {return s;}

    StringBuilder[] rows = new StringBuilder[numRows];
    for (int i = 0; i < numRows; i++) {
        rows[i] = new StringBuilder();
    }
    
    StringBuilder result = new StringBuilder();
    int n = s.length();
    int i = 0;
    int round = 2 * numRows - 2;

    while (i < n) {
        if (i % round <= numRows - 1) {
            rows[i % round].append(s.charAt(i));
        } else {
            rows[round - i % round].append(s.charAt(i));
        }
        i++;
    }
    for (StringBuilder row: rows) {
        result.append(row);
    }
    return result.toString();     
}
```



#### 优化 Visit by Row

No need to traverse the whole string, 而是按照每个row该有多少就visit 多少, <font color = grape>**可以优化掉StringBuilder的array, 只要一个就够了**</font>

<font color = grape>**用i作为remainder, j作为quotient, index就成了 i + j * round**</font>

对于任何i不是1也不是numRows-1的, 同一行还有一个对应的char, <font color = grape>**其index就是 (j + 1) * round - i, 从下一个round开始往前数相同的offset**</font> 

```java
public String convert(String s, int numRows) {

    if (s == null || s.length() == 0) {return null;}
    if (numRows == 1) {return s;}

    StringBuilder result = new StringBuilder();
    int n = s.length();
    int round = 2 * numRows - 2;

    for (int i = 0; i < numRows; i++) {
        for (int j = 0; i + j * round < n; j ++) {

            result.append(s.charAt(i + j * round));
            // deal with the uprise chars
            // corresponding char
            if (i != 0 && i != numRows - 1 && ((j+1)* round - i) < n) {
                result.append(s.charAt((j+1)* round - i));
            } 
        }
    }  
    return result.toString();     
}
```

