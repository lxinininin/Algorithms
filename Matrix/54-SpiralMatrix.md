# 54. Spiral Matrix

给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。

 

**示例 1：**

![img](assets/spiral1.jpg)

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

**示例 2：**

![img](assets/spiral.jpg)

```
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```

 

**提示：**

-   `m == matrix.length`
-   `n == matrix[i].length`
-   `1 <= m, n <= 10`
-   `-100 <= matrix[i][j] <= 100`



```java
class Solution {
  public List<Integer> spiralOrder(int[][] matrix) {
    List<Integer> ans = new ArrayList<>();
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return ans;

    int top = 0, bottom = matrix.length - 1;
    int left = 0, right = matrix[0].length - 1;

    // 从左到右, 从上到下, 从右到左, 从下到上轮流遍历, 到点就退出
    while (true) {
      // 从左到右
      for (int i = left; i <= right; i++) {
        ans.add(matrix[top][i]);
      }

      // 从上到下, 此时上一个从左到右的那一行已经全部填写进 List, 所以 top + 1, 注意边界条件
      if (++top > bottom) break;
      for (int i = top; i <= bottom; i++) {
        ans.add(matrix[i][right]);
      }

      // 从右到左, 同理
      if (--right < left) break;
      for (int i = right; i >= left; i--) {
        ans.add(matrix[bottom][i]);
      }

      // 从下到上, 同理
      if (--bottom < top) break;
      for (int i = bottom; i >= top; i--) {
        ans.add(matrix[i][left]);
      }

      if (++left > right) break;
    }

    return ans;
  }
}
```

