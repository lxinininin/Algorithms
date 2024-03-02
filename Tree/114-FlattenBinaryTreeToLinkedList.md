# 114. Flatten Binary Tree to Linked List

给你二叉树的根结点 `root` ，请你将它展开为一个单链表：

-   展开后的单链表应该同样使用 `TreeNode` ，其中 `right` 子指针指向链表中下一个结点，而左子指针始终为 `null` 。
-   展开后的单链表应该与二叉树 [**先序遍历**](https://baike.baidu.com/item/先序遍历/6442839?fr=aladdin) 顺序相同。

 

**示例 1：**

![img](assets/flaten.jpg)

```
输入：root = [1,2,5,3,4,null,6]
输出：[1,null,2,null,3,null,4,null,5,null,6]
```

**示例 2：**

```
输入：root = []
输出：[]
```

**示例 3：**

```
输入：root = [0]
输出：[0]
```

 

**提示：**

-   树中结点数在范围 `[0, 2000]` 内
-   `-100 <= Node.val <= 100`

 

**进阶：**你可以使用原地算法（`O(1)` 额外空间）展开这棵树吗？



**方法一: 首先我们想到的是用 Pre-order 的顺序将树的每个元素都加到一个 List 里面, 然后再按照 List 的顺序合成一颗根据题意顺序的树**

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
  List<TreeNode> list = new ArrayList<>();

  public void flatten(TreeNode root) {

    flatten_r(root);

    TreeNode dummy = new TreeNode(-1);
    TreeNode pre = dummy;
    for (TreeNode node : list) {
      pre.left = null;
      pre.right = node;
      pre = node;
    }

    root = dummy.right;
  }

  public void flatten_r(TreeNode root) {
    if (root == null) return;

    list.add(root);
    flatten_r(root.left);
    flatten_r(root.right);
  }
}
```



**方法二: 寻找前驱节点, 具体做法是, 对于当前节点, 如果其左子节点不为空, 则在其左子树中找到最右边的节点, 作为前驱节点, 将当前节点的右子节点赋给前驱节点的右子节点, 然后将当前节点的左子节点赋给当前节点的右子节点, 并将当前节点的左子节点设为空. 对当前节点处理结束后, 继续处理链表中的下一个节点, 直到所有节点都处理结束**

<img src="assets/1.png" alt="img" style="zoom: 25%;" />

<img src="assets/2.png" alt="img" style="zoom:25%;" />

<img src="assets/3-20240302232547782.png" alt="img" style="zoom:25%;" />

<img src="assets/4.png" alt="img" style="zoom:25%;" />

<img src="assets/5.png" alt="img" style="zoom:25%;" />

<img src="assets/6.png" alt="img" style="zoom:25%;" />

<img src="assets/7.png" alt="img" style="zoom:25%;" />

<img src="assets/8.png" alt="img" style="zoom:25%;" />

<img src="assets/9.png" alt="img" style="zoom:25%;" />

<img src="assets/10.png" alt="img" style="zoom:25%;" />

<img src="assets/12.png" alt="img" style="zoom:25%;" />

<img src="assets/13.png" alt="img" style="zoom:25%;" />

<img src="assets/14.png" alt="img" style="zoom:25%;" />

<img src="assets/15.png" alt="img" style="zoom:25%;" />

<img src="assets/16.png" alt="img" style="zoom:25%;" />

<img src="assets/17.png" alt="img" style="zoom:25%;" />

<img src="assets/18.png" alt="img" style="zoom:25%;" />

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
  public void flatten(TreeNode root) {
    TreeNode curr = root;
    while (curr != null) {
      if (curr.left != null) {
        TreeNode next = curr.left;
        TreeNode pre = next;
        while (pre.right != null) {
          pre = pre.right;
        }

        pre.right = curr.right;
        curr.left = null;
        curr.right = next;
      }

      curr = curr.right;
    }
  }
}
```

