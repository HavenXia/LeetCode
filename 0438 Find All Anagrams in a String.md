正常的sliding window, 不过这里可以确定window的长度
但是这样做其实比较慢, 这里是长度必定相同的sliding window
如果普通的left和right处理, 判断right是否存在
不存在就把left移过去, 存在还要判断是否三项都是0, 中间各种restore map

所以更简单的方法是用两个map, 直接存当前window的char和count, 然后直接equals比较hashmap. 这样对map的操作也一共就O(n)次
然后更简单的是equals array, 因为一共26个字母...



```java
public List<Integer> findAnagrams(String s, String p) {

    int ns = s.length(), np = p.length();
    if (ns < np) return new ArrayList();

    int [] pCount = new int[26];
    int [] sCount = new int[26];
    // build reference array using string p
    for (char ch : p.toCharArray()) {
        pCount[(int)(ch - 'a')]++;
    }

    List<Integer> output = new ArrayList();
    // sliding window on the string s
    for (int i = 0; i < ns; ++i) {
        // add at right
        sCount[(int)(s.charAt(i) - 'a')]++;

        // remove at left after map is initialized
        if (i >= np) {
            sCount[(int)(s.charAt(i - np) - 'a')]--;
        }

        // compare array in the sliding window
        if (Arrays.equals(pCount, sCount)) {
            output.add(i - np + 1);
        }
    }
    return output;
}
```

