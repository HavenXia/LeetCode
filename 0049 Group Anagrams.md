# 题目地址

https://leetcode.com/problems/group-anagrams/



# 思路

初步推测可以结合HashMap和Priority Queue来做, 每个单词按letter放入pq并取出,组成新string, 然后放入 `HashMap<String, List<String>>`

最后用 `new Arraylist(map.values())` 可以直接把map的value转为一个arraylist返回

<font color = grape>**时间复杂度为O(nL), 其中L为最strs中最长string的长度, 空间复杂度为O(n)**</font>



#### 直接用sort

首先把每个string转成charArray, 然后直接用<font color = gree>`Array.sort()`, 在用new String()转换回String</font>

接着按照HashMap一样存进去, 最后转为ArrayList返回

时间复杂度O(n LlogL), LlogL是对于每个str直接用`Array.sort()`所耗费的时间

但是实际上更快..佛了



# 题解

#### PQ & HashMap

```java
public List<List<String>> groupAnagrams(String[] strs) {

    if (strs == null || strs.length == 0) {return null;}

    PriorityQueue<Character> pq = new PriorityQueue<Character>();
    Map<String, List<String>> map = new HashMap<String, List<String>>();

    for (String str: strs) {
        // add into pq
        for (int i = 0; i < str.length(); i++) {
            pq.offer(str.charAt(i));
        }
        String rearrange = "";
        int size = pq.size();
        for (int j = 0; j < size; j++) {
            rearrange += pq.poll();
        }
        // put into map
        if (!map.containsKey(rearrange)) {
            map.put(rearrange, new ArrayList<String>());                
        }
        map.get(rearrange).add(str);
    }
    return new ArrayList(map.values());
}
```

#### Sort & HashMap

```java
public List<List<String>> groupAnagrams(String[] strs) {

    if (strs == null || strs.length == 0) return new ArrayList();
    Map<String, List> ans = new HashMap<String, List>();
    for (String s : strs) {
        char[] ca = s.toCharArray();
        Arrays.sort(ca);
        String key = new String(ca);
        if (!ans.containsKey(key)) ans.put(key, new ArrayList());
        ans.get(key).add(s);
    }
    return new ArrayList(ans.values());
}
```

