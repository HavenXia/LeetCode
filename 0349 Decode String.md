一个stack存count, 一个stack存string
遇到digit, 要计算当前的count, 记得每次 * 10
遇到[, push count, 同时也push当前的stirng
遇到letter, curString + 当前char
遇到], strings.poll() + counts.poll() * curString 

这里的cur尽量保持在两个bracket之间一直是同一个stringBuilder, 比直接string 相加好很多
注意, stringBuilder.append() 可以接受stringBuilder, string和char!



有recursion和double stack两种写法

```java
class Solution {
    public String decodeString(String s) {
        return dfs(s, 0)[0];
    }

    //首先解释下String[]是什么
    //当String[]的长度为2时，第二个元素为解码出的子字符串，第一个元素为解码出的子字符串的最后字符']'在s中的下标
    //当String[]的长度为1时，数组中只存储了解码出的字符串
    //dfs函数的意思：对字符串s从i往后的子串进行解码，并存在数组里；必要的时候还会在数组里存子串最后那个']'的下标
    private String[] dfs(String s, int i){
        StringBuilder res = new StringBuilder();
        //multi为k[encoded_string]的k，是一个正整数
        int multi = 0;
        while (i < s.length()){
            //当前字符为数字，需要更新multi的值
            //为啥要乘10？因为是从左往右扫描的，如果k不是个位数而是n位整数的话就要通过不停的乘10来更新值
            if (s.charAt(i) >= '0' && s.charAt(i) <= '9')
                multi = 10 * multi + Integer.parseInt(String.valueOf(s.charAt(i)));
            //当前字符为'['，此时需要递归地去解码'['后面的子串
            else if (s.charAt(i) == '['){
                //子串从'['的下一位开始，用tmp保存解码的结果和子串最后的']'在s中的下标
                String[] tmp = dfs(s, i + 1);
                //更新i的值，由于在上一行的递归中子串以及子串内部的子串都被求出来了，所以在外层就不用管它们了，直接把i跳到tmp[0]表示的位置
                i = Integer.parseInt(tmp[0]);
                //这个while循环达到了把k[encoded_string]内的encoded_string在res后拼接k次的效果（这里的encoded_string就是tmp[1]）
                while (multi > 0){
                    res.append(tmp[1]);
                    multi--;
                }
            }
            //当前字符为']'，返回这个子串结尾处的下标和其解码结果
            else if (s.charAt(i) == ']'){
                return new String[] {String.valueOf(i), res.toString()};
            }
            //当前字符为非数字、非'['']'，则把它拼接到res后
            else {
                res.append(String.valueOf(s.charAt(i)));
            }
            //i后移
            i++;
        }
        // only happen of the outest recursion
        // 但凡不是最外层的, 都必定是因为[进去的, 那么一定会碰到]
        return new String[] {res.toString()};
    }
}
```

