# 题目地址

https://leetcode.com/problems/rotate-image/



# 思路

这里是clockwise 90度, 第一遍 traverse(只要traverse 半个正方形)

 `matrix[i][j] = matrix[j][i]`, 得到transpose, 和clockwise 90度正好column顺序相反

```java
for (int i = 0; i < n; i++) {
    for (int j = i; i < n; j++) {
        int temp = matrix[i][j];
        matrix[i][j] = matrix[j][i];
        matrix[j][i] = temp;
    }
}
```

然后对每一行, 镜像即可

```java
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n / 2; j++) {
        int temp = matrix[i][j];
        matrix[i][j] = matrix[i][n - 1- j];
        matrix[i][n - 1- j] = temp;
    }
}
```

时间复杂度O(n), 空间复杂度O(1)





# 题解

```java
public void rotate(int[][] matrix) {

    if (matrix == null || matrix.length == 0) {
        return;
    }
    int n = matrix.length;
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            int temp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = temp;
        }
    }
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n / 2; j++) {
            int temp = matrix[i][j];
            matrix[i][j] = matrix[i][n - 1- j];
            matrix[i][n - 1- j] = temp;
        }
    }
}
```

