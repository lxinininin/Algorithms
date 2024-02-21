# 63. Unique Paths II

一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

网格中的障碍物和空位置分别用 `1` 和 `0` 来表示。

 

**示例 1：**

![img](assets/robot1.jpg)

```
输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
输出：2
解释：3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右
```

**示例 2：**

![img](assets/robot2.jpg)

```
输入：obstacleGrid = [[0,1],[0,0]]
输出：1
```

 

**提示：**

-   `m == obstacleGrid.length`
-   `n == obstacleGrid[i].length`
-   `1 <= m, n <= 100`
-   `obstacleGrid[i][j]` 为 `0` 或 `1`





**the first method we can think is using dp by using matrix:**

```java
class Solution {
  public int uniquePathsWithObstacles(int[][] obstacleGrid) {
    if (obstacleGrid[0][0] == 1)
      return 0;

    int m = obstacleGrid.length;
    int n = obstacleGrid[0].length;
    int[][] dpMat = new int[m][n];

    for (int i = 0; i < m; i++) {
      if (obstacleGrid[i][0] == 0) {
        dpMat[i][0] = 1;
      } else {
        if (n == 1)
          return 0;
        break;
      }
    }

    for (int j = 1; j < n; j++) {
      if (obstacleGrid[0][j] == 0) {
        dpMat[0][j] = 1;
      } else {
        if (m == 1)
          return 0;
        break;
      }
    }

    for (int i = 1; i < m; i++) {
      for (int j = 1; j < n; j++) {
        if (obstacleGrid[i][j] == 1) {
          continue;
        } else {
          dpMat[i][j] = dpMat[i - 1][j] + dpMat[i][j - 1];
        }
      }
    }

    return dpMat[m - 1][n - 1];
  }
}
```



**Then we can think squeeze the matrix in an array to reduce the space complexity, because when we finish iterate one row, we can update our dp array and keep iterating to next row**

```java
class Solution {
  public int uniquePathsWithObstacles(int[][] obstacleGrid) {
    if (obstacleGrid[0][0] == 1)
      return 0;

    int m = obstacleGrid.length;
    int n = obstacleGrid[0].length;
    // we use the number of columns to set dp array
    int[] dp = new int[n];
    dp[0] = 1;

    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        if (obstacleGrid[i][j] == 1) {
          dp[j] = 0;
        } 
        else if (j - 1 >= 0 && obstacleGrid[i][j-1] == 0) {
          // in here, the meaning is same is the  dp matrix
          // up cell different paths (dp[j]) + left cell different paths (dp[j-1]) = current cell different paths
          dp[j] += dp[j-1];
        }
      }
    }

    return dp[n-1];
  }
}
```

