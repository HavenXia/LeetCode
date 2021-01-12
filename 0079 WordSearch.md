# 题目地址

https://leetcode.com/problems/word-search/submissions/



# 思路

对于wordSearch的两道题目, <font color = red>把除了本身以外的sequence当作一个prefix加入map, 对应value为0</font>

然后把整个word存入map中, 对应value为1, <font color = grape>**可以做到查询是否on right track的情况下, 区分出just prefix和full word**</font>

然后就是按照dfs来做, 四个方向, 检测出界, visited, 然后进入下一层的dfs

<font color = red>注意: 进入下一层之前,把当前的position 计作visited, **然后返回之后一定要修改回false!**</font>





## 题解

时间复杂度O(N·3<sup>L</sup>), 因为每次有三个方向选择, 一共要走L的长度, 然后start position有N种

空间复杂度O(L)

```java
public boolean exist(char[][] board, String word) {

    if (board == null || word == null) {return false;}
    Map<String, Integer> map = new HashMap<String, Integer>();
    for (int i = 1; i < word.length(); i++) {
        map.put(word.substring(0,i), 0);
    } 
    map.put(word, 1); // total word
    boolean[][] visited = new boolean[board.length][board[0].length];
    String curWord = "";
    int[][] direction = {{1,0},{0,1},{-1,0},{0,-1}};

    for (int i = 0; i < board.length; i++) {
        for (int j = 0; j < board[0].length; j++) {
            visited[i][j] = true;
            if (wordSearch(board, visited, map, direction, curWord, i, j) == true) {
                return true;
            }
            visited[i][j] = false;
        }
    }

    return false;
}

private boolean wordSearch(char[][] board, boolean[][] visited, 
                           Map<String, Integer> map, int[][] direction,
                           String cur, int x, int y) {

    cur += board[x][y];

    if (!map.containsKey(cur)) {return false;}
    if (map.get(cur) == 1) {return true;}


    for (int i = 0; i < 4; i++) {

        int row = x + direction[i][0];
        int col = y + direction[i][1];

        if (row < 0 || row >= board.length || col < 0 || col >= board[0].length) {continue;}              
        if (visited[row][col]) {continue;}


        visited[row][col] = true;
        if (wordSearch(board, visited, map, direction, cur, row, col)) {
            return true;
        }
        visited[row][col] = false;
    }   
    return false;
}
```

