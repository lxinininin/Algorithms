# 95. Unique Binary Search Trees II

给你一个整数 `n` ，请你生成并返回所有由 `n` 个节点组成且节点值从 `1`到 `n` 互不相同的不同 **二叉搜索树** 。可以按 **任意顺序** 返回答案。

 

**示例 1：**

![img](assets/uniquebstn3.jpg)

```
输入：n = 3
输出：[[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]
```

**示例 2：**

```
输入：n = 1
输出：[[1]]
```

 

**提示：**

-   `1 <= n <= 8`



```java
class Solution {
  // BST 关键的性质是根节点的值大于左子树所有节点的值, 小于右子树所有节点的值, 且左子树和右子树也同样为 BST
  // 因此在生成所有可行的 BST 的时候, 假设当前序列长度为 n, 如果我们枚举根节点的值为 i, 那么根据 BST 的性质我们可以知道左子树的节点值的集合为 [1 … i−1], 右子树的节点值的集合为 [i+1 … n]
  public List<TreeNode> generateTrees(int n) {
    if (n == 0) return null;

    return generateTrees_r(1, n);
  }

  // 此递归函数返回序列 [start, end] 生成的所有可行的 BST
  public List<TreeNode> generateTrees_r(int start, int end) {
    List<TreeNode> allTrees = new ArrayList<>();

    if (end < start) {
      // 不能直接 return null;
      allTrees.add(null);
      return allTrees;
    }

    // 枚举 [start, end] 中的值 i 为当前 BST 的根
    for (int i = start; i <= end; i++) {
      // 获得所有可行的左子树集合
      List<TreeNode> leftTrees = generateTrees_r(start, i - 1);
      // 获得所有可行的右子树集合
      List<TreeNode> rightTrees = generateTrees_r(i + 1, end);

      // 从左子树集合中选出一棵左子树, 从右子树集合中选出一棵右子树, 拼接到根节点上
      for (TreeNode left : leftTrees) {
        for (TreeNode right : rightTrees) {
          TreeNode curTreeHead = new TreeNode(i);
          curTreeHead.left = left;
          curTreeHead.right = right;
          allTrees.add(curTreeHead);
        }
      }
    }

    return allTrees;
  }
}
```

