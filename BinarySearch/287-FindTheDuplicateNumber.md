# 287. Find the Duplicate Number

给定一个包含 `n + 1` 个整数的数组 `nums` ，其数字都在 `[1, n]` 范围内（包括 `1` 和 `n`），可知至少存在一个重复的整数。

假设 `nums` 只有 **一个重复的整数** ，返回 **这个重复的数** 。

你设计的解决方案必须 **不修改** 数组 `nums` 且只用常量级 `O(1)` 的额外空间。

 

**示例 1：**

```
输入：nums = [1,3,4,2,2]
输出：2
```

**示例 2：**

```
输入：nums = [3,1,3,4,2]
输出：3
```

 

**提示：**

-   `1 <= n <= 105`
-   `nums.length == n + 1`
-   `1 <= nums[i] <= n`
-   `nums` 中 **只有一个整数** 出现 **两次或多次** ，其余整数均只出现 **一次**

 

**进阶：**

-   如何证明 `nums` 中至少存在一个重复的数字?
-   你可以设计一个线性级时间复杂度 `O(n)` 的解决方案吗？



```java
class Solution {
  public int findDuplicate(int[] nums) {
    int n = nums.length;
    // 这里我们可以用 l 和 r 想象成一个从 1 到 n-1 有序排列的 array
    // 根据题意, nums 是长度为 n 的 array, 且里面出现的数字只会在 [1, n-1] 的范围内
    int l = 1, r = n-1, ans = -1;
    while (l <= r) {
      int mid = (l + r) / 2;

      // 寻找小于或等于 mid 的数字有几个
      int count = 0;
      for (int i = 0; i < n; i++) {
        if (nums[i] <= mid) {
          count++;
        }
      }

      // 如果在 mid (包含 mid) 前面的数没有重复数字，那么 count 一定是小于等于 mid
      // 所以重复数字一定在我们想象的 array 中 mid 的右边
      // 反之亦然
      // 注意中间情况!!
      // 因为 mid 也有可能会是结果
      if (count <= mid) {
        l = mid + 1;
      } else {
        r = mid - 1;
        ans = mid;
      }
    }
    
    return ans;
  }
}
```

