# 33. Search in Rotated Sorted Array

整数数组 `nums` 按升序排列，数组中的值 **互不相同** 。

在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了 **旋转**，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 **从 0 开始** 计数）。例如， `[0,1,2,4,5,6,7]` 在下标 `3` 处经旋转后可能变为 `[4,5,6,7,0,1,2]` 。

给你 **旋转后** 的数组 `nums` 和一个整数 `target` ，如果 `nums` 中存在这个目标值 `target` ，则返回它的下标，否则返回 `-1` 。

你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```

**示例 2：**

```
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
```

**示例 3：**

```
输入：nums = [1], target = 0
输出：-1
```

 

**提示：**

-   `1 <= nums.length <= 5000`
-   `-104 <= nums[i] <= 104`
-   `nums` 中的每个值都 **独一无二**
-   题目数据保证 `nums` 在预先未知的某个下标上进行了旋转
-   `-104 <= target <= 104`



![](assets/33_fig1.png)



```java
class Solution {
  public int search(int[] nums, int target) {
    int n = nums.length;
    if (n == 0) return -1;
    if (n == 1) return nums[0] == target ? 0 : -1;

    int l = 0, r = n - 1;
    while (l <= r) {
      int mid = (l + r) / 2;
      if (nums[mid] == target) {
        return mid;
      }

      if (nums[0] <= nums[mid]) {
        // 在 mid 左边的区间一定是递增的
        if (nums[0] <= target && target < nums[mid]) {
          r = mid - 1;
        } else {
          l = mid + 1;
        }
      } else {
        // 在 mid 右边的区间一定是递增的
        if (nums[mid] < target && target <= nums[n - 1]) {
          l = mid + 1;
        } else {
          r = mid - 1;
        }
      }
    }

    return -1;
  }
}
```

**复杂度分析**

*   时间复杂度: `O(log n)`, 其中 `n` 为 `nums` 数组的大小. 整个算法时间复杂度即为二分查找的时间复杂度 `O(log n)` 

*   空间复杂度: `O(1)` . 我们只需要常数级别的空间存放变量.







# 81. Search in Rotated Sorted Array II

相同题意, 整数数组 `nums` 按升序排列，数组中的值 **不必互不相同**

**示例 1：**

```
输入：nums = [2,5,6,0,0,1,2], target = 0
输出：true
```

**示例 2：**

```
输入：nums = [2,5,6,0,0,1,2], target = 3
输出：false
```

 

**提示：**

-   `1 <= nums.length <= 5000`
-   `-104 <= nums[i] <= 104`
-   题目数据保证 `nums` 在预先未知的某个下标上进行了旋转
-   `-104 <= target <= 104`



```java
class Solution {
  public boolean search(int[] nums, int target) {
    int n = nums.length;
    if (n == 0) return false;
    if (n == 1) return nums[0] == target;

    int l = 0, r = n - 1;
    while (l <= r) {
      int mid = (l + r) / 2;

      if (nums[mid] == target) {
        return true;
      }
			
      // 对于数组中有重复元素的情况, BinarySearch 可能会有 nums[l] = nums[mid] = nums[r], 此时无法判断区间 [l, mid] 和区间 [mid + 1, r] 哪个是有序的
      // ex: nums=[3,1,2,3,3,3,3], target=2, 首次 BinarySearch 时无法判断区间 [0,3] 和区间 [4,6] 哪个是有序的
      // 对于这种情况, 我们只能将当前二分区间的左边界加一, 右边界减一, 然后在新区间上继续二分查找
      if (nums[l] == nums[mid] && nums[mid] == nums[r]) {
        l++; r--;
      }
      else if (nums[l] <= nums[mid]) {
        if (nums[l] <= target && target < nums[mid]) {
          r = mid - 1;
        } else {
          l = mid + 1;
        }
      } 
      else {
        if (nums[mid] < target && target <= nums[r]) {
          l = mid + 1;
        } else {
          r = mid - 1;
        }
      }
    }

    return false;
  }
}
```

**复杂度分析**

*   时间复杂度: `O(n)`, 其中`n` 是数组`nums` 的长度. 最坏情况下数组元素均相等且不为`target` , 我们需要访问所有位置才能得出结果.

*   空间复杂度: `O(1)` 
