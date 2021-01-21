# 题目地址

https://leetcode.com/problems/string-to-integer-atoi/



# 思路

LeetCode最弱智没有之一, Java用isDigit 模板: <font color = gree>**`Character.isDigit(str.charAt(i))`**</font> 

+ 首先检测第一个char, 如果<font color = grape>**不是digit && 不是加减 && 不是空格, return 0**</font>

+ 接着检测whitespace, 记得条件要加上 `i < n`, <font color = grape>**出循环后检测 `i == n`**</font> 
+ 接着检测+/-, <font color = grape>**这里题目要求只能有一个+或者-, 所以用if而不是while, 同样结束后检测`i == n`**</font>

+ 最后检测数字, 这里每一步都要 check isDigit, 确保到有nondigit的时候直接结束!
+ 检测out of bound 过程同[Reverse Integer](/Users/parallax/Documents/求职/LeetCode/0007 Reverse Integer.md), 不过因为已经提取了flag, <font color = grape>**这里只要和MAX_VALUE比较即可, 超了直接按flag返回**</font>



```java
public int myAtoi(String s) {

    if (s == null || s.length() == 0) {
        return 0;
    }

    int n = s.length();
    int i = 0;
    int flag = 1;
    int num = 0;

    // test first character
    if (!Character.isDigit(s.charAt(i)) && s.charAt(i) != '-' && s.charAt(i) != '+' && s.charAt(i) != ' ') {
        return 0;
    }

    // test whitespace
    while(i < n && s.charAt(i) == ' ') {i++;}
    if (i == n) {return 0;}

    // test + and -
    if (s.charAt(i) == '-') {
        flag = -1; i++;
    } else if (s.charAt(i) == '+') {
        flag = 1; i++;
    }

    // read nums
    if (i < n && !Character.isDigit(s.charAt(i))) {return 0;}

    while (i < n && Character.isDigit(s.charAt(i))) {
        int digit = s.charAt(i) - '0';
        i++;
        if (num > Integer.MAX_VALUE / 10 || (num == Integer.MAX_VALUE / 10 && digit > 7)) {
            num = flag == 1 ? Integer.MAX_VALUE: Integer.MIN_VALUE;
            return num;
        }
        num = num * 10 + digit;
    }

    return num * flag;
}
```

