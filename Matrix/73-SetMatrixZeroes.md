# 73. Set Matrix Zeroes

给定一个 `m x n` 的矩阵，如果一个元素为 **0** ，则将其所在行和列的所有元素都设为 **0** 。请使用 **[in place](http://baike.baidu.com/item/原地算法)** 算法**。**

 

**示例 1：**

![img](assets/mat1.jpg)

```
输入：matrix = [[1,1,1],[1,0,1],[1,1,1]]
输出：[[1,0,1],[0,0,0],[1,0,1]]
```

**示例 2：**

![img](assets/mat2.jpg)

```
输入：matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
输出：[[0,0,0,0],[0,4,5,0],[0,3,1,0]]
```

 

**提示：**

-   `m == matrix.length`
-   `n == matrix[0].length`
-   `1 <= m, n <= 200`
-   `-231 <= matrix[i][j] <= 231 - 1`

 

**进阶：**

-   一个直观的解决方案是使用  `O(mn)` 的额外空间，但这并不是一个好的解决方案。
-   一个简单的改进方案是使用 `O(m + n)` 的额外空间，但这仍然不是最好的解决方案。
-   你能想出一个仅使用常量空间的解决方案吗？



**Method 1: 使用两个 Flag Arrays, 但是空间复杂度是 O(m + n)**

```java
class Solution {
  public void setZeroes(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    // 我们可以用两个 flag arrays 分别记录每一行和每一列是否有零出现
    boolean[] row = new boolean[m];
    boolean[] col = new boolean[n];

    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        if (matrix[i][j] == 0) {
          // 标记所在行和列有 0 出现
          row[i] = col[j] = true;
        }
      }
    }
    
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        // 该行或者该列有 0 出现
        // Matrix 上对应的数需设置为 0
        if (row[i] || col[j]) {
          matrix[i][j] = 0;
        }
      }
    }
  }
}
```



**Method 2: 使用两个 Flag Value 去减少空间复杂度, 我们可以用矩阵的第一行和第一列代替方法一中的两个 Flag Arrays, 以达到 O(1) 的额外空间. 但这样会导致原数组的第一行和第一列被修改, 无法记录它们是否原本包含 0. 因此我们需要额外使用两个标记变量分别记录第一行和第一列是否原本包含 0**

```java
class Solution {
  public void setZeroes(int[][] matrix) {
    int m = matrix.length;
    int n = matrix[0].length;
    // flagRow0 标记 index 为 0 的 row (第一行) 要不要设置为 0
    // flagCol0 标记 index 为 0 的 col (第一列) 要不要设置为 0
    boolean flagRow0 = false, flagCol0 = false;

    // 检查第一列有没有包含 0
    for (int i = 0; i < m; i++) {
      if (matrix[i][0] == 0) {
        flagCol0 = true;
        break;
      }
    }

    // 检查第一行有没有包含 0
    for (int j = 0; j < n; j++) {
      if (matrix[0][j] == 0) {
        flagRow0 = true;
        break;
      }
    }

    for (int i = 1; i < m; i++) {
      for (int j = 1; j < n; j++) {
        if (matrix[i][j] == 0) {
          // 将第一行和第一列分别作为两个不同的 flagArray
          // 作用等同于上一个方法 flagArray 的作用
          matrix[i][0] = 0;
          matrix[0][j] = 0;
        }
      }
    }

    for (int i = 1; i < m; i++) {
      for (int j = 1; j < n; j++) {
        // 该行或者该列有 0 出现
        // Matrix 上对应的数需设置为 0
        if (matrix[i][0] == 0 || matrix[0][j] == 0) {
          matrix[i][j] = 0;
        }
      }
    }

    // 如果第一列包含 0, 则在第一列被当作 flagArray 结束后, 把第一列全部设置为 0
    if (flagCol0) {
      for (int i = 0; i < m; i++) {
        matrix[i][0] = 0;
      }
    }

    // 如果第一行包含 0, 则在第一行被当作 flagArray 结束后, 把第一行全部设置为 0
    if (flagRow0) {
      for (int j = 0; j < n; j++) {
        matrix[0][j] = 0;
      }
    }
  }
}
```

