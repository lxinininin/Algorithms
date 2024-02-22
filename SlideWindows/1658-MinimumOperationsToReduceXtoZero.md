# 1658. Minimum Operations to Reduce X to Zero

给你一个整数数组 `nums` 和一个整数 `x` 。每一次操作时，你应当移除数组 `nums` 最左边或最右边的元素，然后从 `x` 中减去该元素的值。请注意，需要 **修改** 数组以供接下来的操作使用。

如果可以将 `x` **恰好** 减到 `0` ，返回 **最小操作数** ；否则，返回 `-1` 。

 

**示例 1：**

```
输入：nums = [1,1,4,2,3], x = 5
输出：2
解释：最佳解决方案是移除后两个元素，将 x 减到 0 。
```

**示例 2：**

```
输入：nums = [5,6,7,8,9], x = 4
输出：-1
```

**示例 3：**

```
输入：nums = [3,2,20,1,1,3], x = 10
输出：5
解释：最佳解决方案是移除后三个元素和前两个元素（总共 5 次操作），将 x 减到 0 。
```

 

**提示：**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 104`
-   `1 <= x <= 109`



```java
class Solution {
  public int minOperations(int[] nums, int x) {
    int n = nums.length;
    if (nums[0] > x && nums[n-1] > x) return -1;

    int sum = Arrays.stream(nums).sum();
    if (sum < x) return -1;
    if (sum == x) return n;

    int res = n + 1;
    // 当 left 指针在 i 上的时候, 即 leftSum 就是在 i 左边的 (包括 i) 的所有数之和
    // 当 right 指针在 i 上的时候, 即 rightSum 就是在 i 右边的 (包括 i) 的所有数之和
    // 我们初始化 leftSum = 0, rightSum = sum, 所以对应的索引就是 -1 和 0
    int left = -1, right = 0;
    int leftSum = 0, rightSum = sum;
    while (left < n) {
      if (left != -1) {
        leftSum += nums[left];
      }

      // 当我们确定了 left 指针的时候
      // 我们就通过右移 right 指针去减少 rightSum 来获得 leftSum 和 rightSum 之和正好等于 x
      // right 指针可以直接沿用上次循环时的位置, 因为随着 left 指针的逐渐右移, leftSum 会越来越大, 相对的, rightSum 必须逐渐减小才能使得 leftSum + rightSum = 0
      while (right < n && leftSum + rightSum > x) {
        rightSum -= nums[right];
        right++;
      }

      if (leftSum + rightSum == x) {
        // 注意: 左边去掉的元素数量是 (left + 1), 右边去掉的元素数量是 (n - right)
        res = Math.min(res, (left + 1) + (n - right));
      }

      left++;
    }

    // 当 res = n + 1 时, 说明没有结果
    return res == n + 1 ? -1 : res;
  }
}
```

