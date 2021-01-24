# 题目地址

https://leetcode.com/problems/number-of-islands/



# 思路

BFS老题新做

<font color = grape>**注意: BFS一定要有一个queue来存储position,BFS是没有recursion的!**</font>

这里是用每个pos离初始点位的距离来作为辨识加入queue的, 否则的话就需要自己用Point class来做了

每次进入BFS, 先把当前位置改成0, 然后放入queue里面, 接着就是标准的BFS模板

==注意这里每次生产新的x和y都要检测一下有没有出界==

时间复杂度O(mn), <font color = red>空间复杂度O(min(m,n))</font>, 想像成一层一层, 最多一层也只能有min(m,n)个



```java
public int numIslands(char[][] grid) {

    if (grid == null || grid.length == 0 || grid[0].length == 0) {
        return 0;
    }

    int m = grid.length;
    int n = grid[0].length;
    int count = 0;

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == '1') {
                count++;
                bfs(grid, i, j);
            }    
        }
    }
    return count;
}

private void bfs(char[][] grid, int x, int y) {

    int[][] direction = new int[][]{{1, 0}, {-1, 0}, {0, -1}, {0,1}};
    Queue<Integer> queue = new LinkedList<Integer>();

    grid[x][y] = '0';
    queue.offer(x * grid[0].length + y);

    while (!queue.isEmpty()) {

        int pos = queue.poll();
        int curX = pos / grid[0].length;
        int curY = pos % grid[0].length;

        for (int i = 0; i < direction.length; i++) {

            int newX = curX + direction[i][0];
            int newY = curY + direction[i][1];

            if (newX < 0 || newX >= grid.length || newY < 0 || newY >= grid[0].length) {
                continue;
            }
            if (grid[newX][newY] == '1') {
                grid[newX][newY] = '0';
                queue.offer(newX * grid[0].length + newY);
            }
        }  
    }
}
```



#### DFS

用dfs来做的的话, 其实代码量上更少了, 因为不需要单独研究queue

```java
class Solution {
    void dfs(char[][] grid, int r, int c) {
        // nr = number of rows
        // nc = number of columns
        int nr = grid.length;
        int nc = grid[0].length;
        
		// 判断出界 & 已经等于0!
        if (r < 0 || c < 0 || r >= nr || c >= nc || grid[r][c] == '0') {
            return;
        }
		// 改掉当前值
        grid[r][c] = '0';
        // dfs
        dfs(grid, r - 1, c);
        dfs(grid, r + 1, c);
        dfs(grid, r, c - 1);
        dfs(grid, r, c + 1);
    }

    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }
		
        int nr = grid.length;
        int nc = grid[0].length;
        int num_islands = 0;
        // 遍历
        for (int r = 0; r < nr; ++r) {
            for (int c = 0; c < nc; ++c) {
                if (grid[r][c] == '1') {
                    ++num_islands;
                    dfs(grid, r, c);
                }
            }
        }

        return num_islands;
    }
}
```





