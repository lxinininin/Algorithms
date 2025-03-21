# 239. Sliding Window Maximum

给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

返回 *滑动窗口中的最大值* 。

 

**示例 1：**

```
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**示例 2：**

```
输入：nums = [1], k = 1
输出：[1]
```

 

**提示：**

-   `1 <= nums.length <= 105`
-   `-104 <= nums[i] <= 104`
-   `1 <= k <= nums.length`



```java
class Solution {
  public int[] maxSlidingWindow(int[] nums, int k) {
    int n = nums.length;
    if (n == 0) return new int[0];

    int[] res = new int[n - k + 1];
    // 该队列将存储当前滑动窗口中元素的索引, 确保队列中的元素值是单调递减的
    Deque<Integer> deq = new LinkedList<>();

    for (int i = 0; i < n; i++) {
      // 移除不在当前滑动窗口内的元素索引, 最后在队列顶端的元素索引对应的元素即是当前滑动窗口中最大的元素值
      while (!deq.isEmpty() && deq.peekFirst() < i - k + 1) {
        deq.pollFirst();
      }

      // 从后向前依次移除队列中所有小于当前元素的元素
      while (!deq.isEmpty() && nums[deq.peekLast()] < nums[i]) {
        deq.pollLast();
      }

      // 并将当前元素的索引添加到队尾
      deq.offerLast(i);

      // 如果遍历的索引大于等于 k-1，开始记录结果
      // 防止当 i = 0, k = 3, 直接把 i = 0 对应的元素写入 res
      if (i >= k - 1) {
        res[i - k + 1] = nums[deq.peekFirst()];
      }
    }

    return res;
  }
}
```

