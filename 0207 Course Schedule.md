# 题目地址

https://leetcode.com/problems/course-schedule/



# 思路

和0200一样, 是练习DFS和BFS的题目

<font color = grape>**这道题本质是找出directed graph中有没有cycle!**</font>

#### BFS

需要用TopoSort, 先把所有入度为0的加入queue, 然后通过Hashmap存储的递进关系进行BFS, 每poll()出一个就count++

当某个点的indegree也变成0的时候, 就加入queue, (<font color = grape>**这也是和普通bfs graph不同的地方, 最后bfs结束, 如果count != courseNum就说明这个toposort走不完整个graph**</font>)

<font color = red>**拓扑排序如果不能遍历完, 那么一定是有cycle!**</font>

这里的Map是 preCourse: Arraylist of nextCourse, int[] 是 indegree[Course] = indegree

时间复杂度O(|V| + |E|) BFS本身是|E|, 空间复杂度O(|V| + |E|), 来自hashMap



#### DFS

并不是很好找cycle, 最蠢的方法就是从每个node开始走到头, 每次都创一个Hash判断有没有访问过, 但是这样时间复杂度worst case是O(E+ V<sup>2</sup>)

优化: 可以运用DP的思想, 首先<font color = grape>**在Directed Graph中, 如果一个node的所有decendent nodes没有组成cycle, 那么把当前node加回去也不会有新的cycle**</font>

其实是运用了dp的思想,  在普通dfs中加入checked来表示node及其decendent没有cycle, 下次再dfs到这个node直接返回即可



# 题解

BFS

```java
public boolean canFinish(int numCourses, int[][] prerequisites) {

    if (prerequisites == null) {
        return false;
    }
    // here length == 0 is possible
    if (prerequisites.length == 0) {
        return true;
    }


    Map<Integer, List<Integer>> map = new HashMap<>();
    int[] indegree = new int[numCourses];

    for (int i = 0; i < prerequisites.length; i++) {

        if (!map.containsKey(prerequisites[i][1])) {

            map.put(prerequisites[i][1], new ArrayList<Integer>()); 
        } 
        map.get(prerequisites[i][1]).add(prerequisites[i][0]);    
        indegree[prerequisites[i][0]]++;
    }
    // add degree == 0 into queue
    Queue<Integer> queue = new LinkedList<>();
    int visited = 0;
    for (int i = 0; i < numCourses; i++) {
        // 这里加入的是i!!!
        if (indegree[i] == 0) {queue.offer(i);}
    }

    // begin bfs
    while (!queue.isEmpty()) {
        int cur = queue.poll();
        visited++;

        // if traverse to end, continue
        if (!map.containsKey(cur)) {
            continue;
        }

        for (int nextCourse: map.get(cur)) {
            indegree[nextCourse]--;
            if (indegree[nextCourse] == 0) {
                queue.offer(nextCourse);
            }
        }
    }
    if (visited == numCourses) {
        return true;
    }
    return false;
}
```

