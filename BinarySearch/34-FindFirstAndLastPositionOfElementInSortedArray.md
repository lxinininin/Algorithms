# 34. Find First and Last Position of Element in Sorted Array

给你一个按照非递减顺序排列的整数数组 `nums`，和一个目标值 `target`。请你找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

你必须设计并实现时间复杂度为 `O(log n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

**示例 2：**

```
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
```

**示例 3：**

```
输入：nums = [], target = 0
输出：[-1,-1]
```

 

**提示：**

-   `0 <= nums.length <= 105`
-   `-109 <= nums[i] <= 109`
-   `nums` 是一个非递减数组
-   `-109 <= target <= 109`



```java
class Solution {
  // 两次 BinarySearch, 分别查找最开始的位置和最结尾的位置
  public int[] searchRange(int[] nums, int target) {
    int n = nums.length;
    int start = -1, end = -1;
    
    // 寻找 target 开始的位置
    int l = 0, r = n - 1;
    while (l <= r) {
      int mid = (l + r) / 2;
      // 等于, 小于, 大于 三种情况分开讨论更清晰, 更不容易弄错!!!
      // 要是没有在 nums 中找到 target, 则 start 依旧是 -1, 方便了后续返回最后的结果
      if (target == nums[mid]) {
        start = mid;
        r = mid - 1; // 因为我们得找 target 在最左边的位置, 所以还得继续循环去寻找
      } 
      else if (target < nums[mid]) {
        r = mid - 1;
      }
      else {
        l = mid + 1;
      }
    }

    // 寻找 target 结束的位置
    l = 0; r = n - 1;
    while (l <= r) {
      int mid = (l + r) / 2;
      if (target == nums[mid]) {
        end = mid;
        l = mid + 1; // 因为我们得找 target 在最右边的位置, 所以还得继续循环去寻找
      } 
      else if (target < nums[mid]) {
        r = mid - 1;
      }
      else {
        l = mid + 1;
      }
    }

    return new int[]{start, end};
  }
}
```

