# 75. Sort Colors

给定一个包含红色、白色和蓝色、共 `n` 个元素的数组 `nums` ，**[原地](https://baike.baidu.com/item/原地算法)**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

我们使用整数 `0`、 `1` 和 `2` 分别表示红色、白色和蓝色。

必须在不使用库内置的 sort 函数的情况下解决这个问题。

 

**示例 1：**

```
输入：nums = [2,0,2,1,1,0]
输出：[0,0,1,1,2,2]
```

**示例 2：**

```
输入：nums = [2,0,1]
输出：[0,1,2]
```

 

**提示：**

-   `n == nums.length`
-   `1 <= n <= 300`
-   `nums[i]` 为 `0`、`1` 或 `2`

 

**进阶：**

-   你能想出一个仅使用常数空间的一趟扫描算法吗？



```java
class Solution {
  // 仅使用一次遍历
  public void sortColors(int[] nums) {
    // 我们用指针 ptr0 来交换 0, ptr2 来交换 2
    // ptr0 指向开头最后一个 0 的位置, ptr2 指向尾部第一个 2 的位置
    // 在遍历的过程中, 我们需要找出所有的 0 交换至数组的头部, 并且找出所有的 2 交换至数组的尾部
    int ptr0 = -1, ptr2 = nums.length;
    int i = 0;
    while (i < ptr2) {
      // 当 nums[i] = 0 时, 将其与 nums[ptr0] 进行交换, 并将 ptr0 向后移动一个位置以便后续使用
      if (nums[i] == 0) {
        swap(nums, i, ptr0 + 1);
        ptr0++;
        i++;
      }
      // // 当 nums[i] = 2 时, 将其与 nums[ptr2] 进行交换, 并将 ptr2 向前移动一个位置以便后续使用
      // 注意!!! 此情况不能 i++
      // 当我们将 nums[i] 与 nums[ptr2] 进行交换之后, 新的 nums[i] 可能仍是 2, 也可能是 0
      // 因此, 我们需要不停地将其与 nums[ptr2]进行交换, 直到新的 nums[i] 不等于 2
      else if (nums[i] == 2) {
        swap(nums, i, ptr2 - 1);
        ptr2--;
      }
      else {
        i++;
      }
    }
  }

  public void swap(int[] nums, int i, int j) {
    int tmp = nums[i];
    nums[i] = nums[j];
    nums[j] = tmp;
  }
}
```

