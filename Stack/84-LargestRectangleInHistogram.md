# 84. Largest Rectangle in Histogram

给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

 

**示例 1:**

![img](assets/histogram.jpg)

```
输入：heights = [2,1,5,6,2,3]
输出：10
解释：最大的矩形为图中红色区域，面积为 10
```

**示例 2：**

![img](assets/histogram-1.jpg)

```
输入： heights = [2,4]
输出： 4
```

 

**提示：**

-   `1 <= heights.length <=105`
-   `0 <= heights[i] <= 104`



**方法一: 单调栈**

首先我们会先想到暴力破解来解决这道题目, 就是我们逐个遍历每一个高度, 并且求出在这个高度中尽可能大的面积范围, 就是找出在此高度中做矩形, 其最大的左边界和右边界分别是多少

例如: heights = [6,7,5,2,4,5,9,3], 以 height = 4 为例, 其左边界只能到旁边的 2 这里 (不包括 2), 因为 4 > 2, 如果我们要选择 4 为高度的矩形, 我们就无法选择 2 成为以 4 为高度的矩形的一部分, 并且 2 和在 2 之前的数我们也就不会考虑了; 同理, 右边界到最后一个数 3 这里 (不包括 3), 在这个边界里的数个个都大于等于 4

而暴力解法的时间复杂度太高, 我们可以运用这个思路结合栈将时间复杂度优化成 O(N)

**栈的思路**: 我们在枚举到第 i 根柱子的时候, 就可以先把之前遍历过的所有高度大于等于 height[i] 的 j 值全部移除 (因为他们都可以作为 height[i] 为高度的矩形的一部分), 剩下的 j 值中高度最高的即为答案 (即找到了不能作为 height[i] 为高度的矩形的一部分的值, 即确立了左边界或者右边界的位置); 在这之后, 我们将 i 放入栈中, 开始接下来的枚举; 如果我们枚举到 i+1 根柱子的时候, 如果 height[i] >= height[i + 1], 那么我们就得把 i 移出栈, 此时我们不用担心之前被 i 移除栈的元素, 因为他们的高度肯定都大于等于 i+1 的高度

若上述方法为确立每个高度的左边界, 那么我们只需要再用相同的思路即可确立每个高度的右边界, 即可找出每个高度尽可能大的面积范围, 然后取最大值就是答案

```java
class Solution {
  public int largestRectangleArea(int[] heights) {
    int n = heights.length;
    // left 和 right 分别表示每个数的左右边界
    int[] left = new int[n];
    int[] right = new int[n];

    // 尽量避免使用 Stack<Integer> monoStack = new Stack<>(); 会增加时间复杂度
    Deque<Integer> monoStack = new ArrayDeque<>();
    // 遍历每个数找出每个数的左边界
    for (int i = 0; i < n; i++) {
      while (!monoStack.isEmpty() && heights[monoStack.peek()] >= heights[i]) {
        monoStack.pop();
      }

      left[i] = monoStack.isEmpty() ? -1 : monoStack.peek();

      monoStack.push(i);
    }

    monoStack.clear();
    // 遍历每个数找出每个数的右边界
    for (int i = n - 1; i >= 0; i--) {
      while (!monoStack.isEmpty() && heights[monoStack.peek()] >= heights[i]) {
        monoStack.pop();
      }

      right[i] = monoStack.isEmpty() ? n : monoStack.peek();

      monoStack.push(i);
    }

    // 找出最大的面积
    int res = 0;
    for (int i = 0; i < n; i++) {
      res = Math.max(res, (right[i] - left[i] - 1) * heights[i]);
    }

    return res;
  }
}
```



**方法二: 单调栈 + 常数优化**

上面的解法需要遍历两次才能分别求出每个高度的左右区间, 我们可以对其进行优化到只需要遍历一次就能直接求出每个高度的左右区间

在方法一中, 我们在对位置 i 进行入栈操作时, 确定了它的左边界; 实际上, 我们在对位置 i 进行出栈操作时可以确定它的右边界!! 其关键思路是<u>当位置 i 被弹出栈时, 说明此时遍历到的位置 i0 的高度小于或者等于 height[i], 并且在 i0 和 i 之间没有其他高度小于或等于 height[i] 的柱子</u>; 这是因为, <u>如果在 i 和 i0 之前还有其他位置 j 的高度小于或等于 height[i] 的, 那么在遍历到 j 的时候, i 应该已经被弹出栈了</u>; 所以位置 i0 就是位置 i 的右边界

```java
class Solution {
  public int largestRectangleArea(int[] heights) {
    int n = heights.length;
    int[] left = new int[n];
    int[] right = new int[n];
    Arrays.fill(right, n);  // 先把所有数的右边界都设置为 n

    Deque<Integer> monoStack = new ArrayDeque<>();
    for (int i = 0; i < n; i++) {
      while (!monoStack.isEmpty() && heights[monoStack.peek()] >= heights[i]) {
        right[monoStack.peek()] = i;  // 确定弹出栈的数的右边界是 i
        monoStack.pop();
      }

      left[i] = monoStack.isEmpty() ? -1 : monoStack.peek();

      monoStack.push(i);
    }

    int res = 0;
    for (int i = 0; i < n; i++) {
      res = Math.max(res, (right[i] - left[i] - 1) * heights[i]);
    }

    return res;
  }
}
```

