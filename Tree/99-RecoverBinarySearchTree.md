# 99. Recover Binary Search Tree

给你二叉搜索树的根节点 `root` ，该树中的 **恰好** 两个节点的值被错误地交换。*请在不改变其结构的情况下，恢复这棵树* 。

 

**示例 1：**

![img](assets/recover1.jpg)

```
输入：root = [1,3,null,null,2]
输出：[3,1,null,null,2]
解释：3 不能是 1 的左孩子，因为 3 > 1 。交换 1 和 3 使二叉搜索树有效。
```

**示例 2：**

![img](assets/recover2.jpg)

```
输入：root = [3,1,4,null,null,2]
输出：[2,1,4,null,null,3]
解释：2 不能在 3 的右子树中，因为 2 < 3 。交换 2 和 3 使二叉搜索树有效。
```

 

**提示：**

-   树上节点的数目在范围 `[2, 1000]` 内
-   `-231 <= Node.val <= 231 - 1`

 

**进阶：**使用 `O(n)` 空间复杂度的解法很容易实现。你能想出一个只使用 `O(1)` 空间的解决方案吗？



**方法一：中序遍历**

首先我们想到的就是可以对这棵树进行中序遍历, 并将遍历出来的数存到数组中, 因为正确的 BST 中序遍历出来的数组一定是单调递增的, 通过数组, 我们能容易地得找到这两个位置错误的数

例如, 正确的 BST 中序遍历出来的数组是 `a = [1 2 3 4 5 6 7]`, 当交换了 2 和 6 所在节点的位置后, 中序遍历出来的数组就是 `a = [1 6 3 4 5 2 7]`, 此时数组中有两个位置不满足 `a_i < a_(i+1)`, 在这个数组中体现为 6 > 3, 5 > 2, 那么我们如果通过这两个位置来找到被错误交换的两个节点呢 (找到 2, 6 而不是选择 2, 3 或 3, 5 等等)

我们可以这样: 面对交换两个节点后的 BST 中序遍历出来的数组, 首先找到不满足条件的位置, 有可能是两个位置或者一个位置 (一个位置的情况就是正好相邻两个数进行了交换); 

*   <u>如果有两个位置, 我们记为 i 和 j (`i < j` 且 `a_i < a_(i+1)` 且 `a_j < a_(j+1)`), 那么对应被错误交换的节点即为 `a_i` 对于的节点和 `a_(j+1)` 对应的节点, 我们分别记为 x 和 y</u>

*   <u>如果有一个位置, 我们记为 i, 那么对应被错误交换的节点即为 `a_i` 对于的节点和 `a_(i+1)` 对应的节点, 我们分别记为 x 和 y</u>

*   此处的代码为

    ```java
    public int[] findTwoSwapped(List<Integer> nums) {
      int n = nums.size();
      int index1 = -1, index2 = -1;
      for (int i = 0; i < n - 1; ++i) {
        if (nums.get(i + 1) < nums.get(i)) {
          index2 = i + 1;
          if (index1 == -1) {
            index1 = i;
          } else {
            break;
          }
        }
      }
      int x = nums.get(index1), y = nums.get(index2);
      return new int[]{x, y};
    }
    ```

最后交换 x 和 y 两个节点即可

以上方法是显式地将中序遍历的值序列保存在一个 nums 数组中，然后再去寻找被错误交换的节点，但我们也可以隐式地在中序遍历的过程就找到被错误交换的节点 x 和 y

具体来说，由于<u>我们只关心中序遍历的值序列中每个相邻的位置的大小关系是否满足条件，且错误交换后最多两个位置不满足条件，因此在中序遍历的过程我们只需要维护当前中序遍历到的最后一个节点 pred，然后在遍历到下一个节点的时候，看两个节点的值是否满足前者小于后者即可，如果不满足说明找到了一个交换的节点，且在找到两次以后就可以终止遍历</u>。

这样我们就可以在中序遍历中直接找到被错误交换的两个节点 x 和 y，不用显式建立 nums 数组。中序遍历的实现有迭代和递归两种等价的写法，在本方法中提供迭代实现的写法。**使用迭代实现中序遍历需要手动维护栈**。

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
  public void recoverTree(TreeNode root) {
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode x = null, y = null, pred = null;

    // 利用 Stack 对树进行中序遍历
    while (!stack.isEmpty() || root != null) {
      while (root != null) {
        stack.push(root);
        root = root.left;
      }

      root = stack.pop();
      
      // 这里跟上面的 findTwoSwapped 相类似
      if (pred != null && pred.val > root.val) {
        y = root;
        if (x == null) {
          x = pred;
        } else {
          break;
        }
      }

      pred = root;
      root = root.right;
    }

    swap(x, y);
  }

  public void swap(TreeNode x, TreeNode y) {
    int tmp = x.val;
    x.val = y.val;
    y.val = tmp;
  }
}
```

