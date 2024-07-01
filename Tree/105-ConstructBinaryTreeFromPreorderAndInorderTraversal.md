# 105. Construct Binary Tree from Preorder and Inorder Traversal

给定两个整数数组 `preorder` 和 `inorder` ，其中 `preorder`是二叉树的**前序遍历**， `inorder` 是同一棵树的**中序遍历**，请构造二叉树并返回其根节点。

 

**示例 1:**

![img](assets/tree.jpg)

```
输入: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
输出: [3,9,20,null,null,15,7]
```

**示例 2:**

```
输入: preorder = [-1], inorder = [-1]
输出: [-1]
```

 

**提示:**

-   `1 <= preorder.length <= 3000`
-   `inorder.length == preorder.length`
-   `-3000 <= preorder[i], inorder[i] <= 3000`
-   `preorder` 和 `inorder` 均 **无重复** 元素
-   `inorder` 均出现在 `preorder`
-   `preorder` **保证** 为二叉树的前序遍历序列
-   `inorder` **保证** 为二叉树的中序遍历序列



对于任意一颗树而言, 前序 (pre order) 遍历的形式总是

`[ 根节点, [左子树的前序遍历结果], [右子树的前序遍历结果] ]`

即根节点总是前序遍历中的第一个节点

而中序 (in order) 遍历的形式总是

`[ [左子树的中序遍历结果], 根节点, [右子树的中序遍历结果] ]`

<u>只要我们在中序遍历中定位到根节点，那么我们就可以分别知道左子树和右子树中的节点数目。</u>由于同一颗子树的前序遍历和中序遍历的长度显然是相同的，因此我们就可以对应到前序遍历的结果中，对上述形式中的所有<u>左右括号进行定位</u>。

这样以来，我们就知道了左子树的前序遍历和中序遍历结果，以及右子树的前序遍历和中序遍历结果，我们就可以递归地对构造出左子树和右子树，再将这两颗子树接到根节点的左右位置。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

class Solution {
  Map<Integer, Integer> indexMap;

  public TreeNode buildTree(int[] preorder, int[] inorder) {
    int n = inorder.length;

    // 构造哈希映射，帮助我们快速定位根节点以及它的位置
    indexMap = new HashMap<>();
    for (int i = 0; i < n; i++) {
      indexMap.put(inorder[i], i);
    }

    return buildTree_r(preorder, inorder, 0, n - 1, 0, n - 1);
  }

  public TreeNode buildTree_r(int[] preorder, int[] inorder, int preorderLeft, int preorderRight, int inorderLeft, int inorderRight) {
    if (preorderLeft > preorderRight) return null;

    // 前序遍历中的第一个节点就是根节点
    int preorderRoot = preorderLeft;
    // 在中序遍历中定位根节点
    int inorderRoot = indexMap.get(preorder[preorderRoot]);

    // 建立根节点
    TreeNode root = new TreeNode(preorder[preorderRoot]);

    // 得到左子树中的节点数目
    int leftSubtreeSize = inorderRoot - inorderLeft;

     // 递归地构造左子树, 并连接到根节点
     // 先序遍历中「从 左边界+1 开始的 LeftSubtreeSize」个元素就对应了中序遍历中「从 左边界 开始到 根节点定位-1」的元素
    root.left = buildTree_r(preorder, inorder, preorderLeft + 1, preorderLeft + leftSubtreeSize, inorderLeft, inorderRoot - 1);
		// 递归地构造右子树, 并连接到根节点
    // 先序遍历中「从 左边界+1+左子树节点数目 开始到 右边界」的元素就对应了中序遍历中「从 根节点定位+1 到 右边界」的元素
    root.right = buildTree_r(preorder, inorder, preorderLeft + leftSubtreeSize + 1, preorderRight, inorderRoot + 1, inorderRight);

    return root;
  }
}
```

