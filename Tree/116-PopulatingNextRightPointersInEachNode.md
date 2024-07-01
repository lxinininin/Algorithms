# 116. Populating Next Right Pointers in Each Node

给定一个 **完美二叉树** ，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL`。

初始状态下，所有 next 指针都被设置为 `NULL`。

 

**示例 1：**

![img](assets/116_sample.png)

```
输入：root = [1,2,3,4,5,6,7]
输出：[1,#,2,3,#,4,5,6,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。序列化的输出按层序遍历排列，同一层节点由 next 指针连接，'#' 标志着每一层的结束。
```



**示例 2:**

```
输入：root = []
输出：[]
```

 

**提示：**

-   树中节点的数量在 `[0, 212 - 1]` 范围内
-   `-1000 <= node.val <= 1000`

 

**进阶：**

-   你只能使用常量级额外空间。
-   使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。



**方法一: 层次遍历 (需要一个栈)**

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/

class Solution {
  public Node connect(Node root) {
    if (root == null) return null;

    Deque<Node> stack = new ArrayDeque<>();
    stack.add(root);

    // 外层的 while 循环迭代的是层数
    while (!stack.isEmpty()) {
      // 提前记录当前层的 node 个数
      int size = stack.size();
      // 我们将二叉树每一层的节点拿出来遍历并连接
      // 遍历约束至当前层, size 以及之后的都是下一层的内容, 因为在遍历的同时, 我们会将当前遍历层的下一层放入栈尾以便于下次遍历
      for (int i = 0; i < size; i++) {
        Node node = stack.poll();

        // 当 i = size - 1 时, 说明已经遍历到当前层的最后一个数了, 故不做连接操作, 否则 next 就变成下一层的第一个数了
        if (i < size - 1) {
          // 连接操作
          node.next = stack.peek();
        }

        // 遍历的同时, 将当前遍历层的下一层放入栈尾以便于下次遍历
        if (node.left != null) stack.add(node.left);
        if (node.right != null) stack.add(node.right);
      }
    }

    return root;
  }
}
```



**方法二: 使用已建立的 next 指针, 只使用常量级额外空间**

```java
class Solution {
  public Node connect(Node root) {
    if (root == null) return null;

    // 每一层的遍历我们都从当前层的最左边的节点开始
    Node leftMost = root;

    while (leftMost.left != null) {
  		// 遍历这一层节点组织成的链表，为下一层的节点更新 next 指针
      Node node = leftMost;

      while (node != null) {
        // 当前节点的左子节点连接其右子节点
        node.left.next = node.right;

        // 让当前节点的右子节点连接当前节点的下一个节点的左子节点 (前提是有当前节点有下一个节点)
        // 在上一层操作的时候, 我们已经将当前层的节点连接在一起了, 所以可以直接 node.next 前往当前节点的下一个节点
        if (node.next != null) {
          node.right.next = node.next.left;
        }

        // 去当前节点的下一个节点执行上述相同操作
        node = node.next;
      }

      // 去下一层的最左的节点执行上述相同操作
      leftMost = leftMost.left;
    }

    return root;
  }
}
```



# 117. Populating Next Right Pointers in Each Node II

给定一个二叉树：

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL` 。

初始状态下，所有 next 指针都被设置为 `NULL` 。

 

**示例 1：**

![img](assets/117_sample.png)

```
输入：root = [1,2,3,4,5,null,7]
输出：[1,#,2,3,#,4,5,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。序列化输出按层序遍历顺序（由 next 指针连接），'#' 表示每层的末尾。
```

**示例 2：**

```
输入：root = []
输出：[]
```

 

**提示：**

-   树中的节点数在范围 `[0, 6000]` 内
-   `-100 <= Node.val <= 100`

**进阶：**

-   你只能使用常量级额外空间。
-   使用递归解题也符合要求，本题中递归程序的隐式栈空间不计入额外空间复杂度。



**方法一: 层次遍历 (需要一个栈)**: 此方法与上一题的此方法思路相同

```java
class Solution {
  public Node connect(Node root) {
    if (root == null) return null;

    Deque<Node> stack = new ArrayDeque<>();
    stack.add(root);

    while (!stack.isEmpty()) {
      int size = stack.size();
      for (int i = 0; i < size; i++) {
        Node node = stack.poll();
        if (i < size - 1) {
          node.next = stack.peek();
        }

        if (node.left != null) stack.add(node.left);
        if (node.right != null) stack.add(node.right);
      }
    }

    return root;
  }
}
```



**方法二: 使用已建立的 next 指针, 只使用常量级额外空间**

```java
class Solution {
  public Node connect(Node root) {
    if (root == null) return null;

    Node head = root; // 当前层的头节点
    // 循环遍历每一层，从上至下
    while (head != null) {
      Node dummy = new Node(0); // 用作下一层的虚拟头节点
      Node tmp = dummy; // 利用 tmp 去连接当前层的下一层节点

			// 遍历当前层，连接下一层的节点
      Node cur = head;
      while (cur != null) {
        if (cur.left != null) {
          // 连接并移动 tmp
          tmp.next = cur.left;
          tmp = tmp.next;
        }

        if (cur.right != null) {
          // 连接并移动 tmp
          tmp.next = cur.right;
          tmp = tmp.next;
        }

        cur = cur.next;
      }

			// 移动到下一层的实际头节点处
      head = dummy.next;
    }

    return root;
  }
}
```

