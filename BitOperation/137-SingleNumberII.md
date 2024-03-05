# 137. Single Number II

给你一个整数数组 `nums` ，除某个元素仅出现 **一次** 外，其余每个元素都恰出现 **三次 。**请你找出并返回那个只出现了一次的元素。

你必须设计并实现线性时间复杂度的算法且使用常数级空间来解决此问题。

 

**示例 1：**

```
输入：nums = [2,2,3,2]
输出：3
```

**示例 2：**

```
输入：nums = [0,1,0,1,0,1,99]
输出：99
```

 

**提示：**

-   `1 <= nums.length <= 3 * 104`
-   `-231 <= nums[i] <= 231 - 1`
-   `nums` 中，除某个元素仅出现 **一次** 外，其余每个元素都恰出现 **三次**



**依次确定每一个二进制位**

我们可以考虑答案的第 i 个二进制位 (从后向前数, i 从 0 开始编号), 它可能成为 0 或 1.

对于数组中非答案的元素, 每一个元素都出现了 3 次, 对应着第 i 个二进制位的 3 个 0 或 3 个 1, 无论是哪一种情况, 它们的和都是 3 的倍数（即和为 0 或 3）

因此, 答案的第 i 个二进制位就是数组中所有元素的第 i 个二进制位之和除以 3 的余数

```java
class Solution {
  public int singleNumber(int[] nums) {
    int res = 0;
    for (int i = 0; i < 32; i++) {
      int tot = 0;
      for (int num : nums) {
        // num >> i: This is a bitwise right shift operation. It shifts the binary representation of the integer num to the right by i bits. This effectively moves the bit at position i to the rightmost position, making it the least significant bit (LSB).
        // (num >> i) & 1: This is a bitwise AND operation with 1. Since 1 in binary is 0001, performing a bitwise AND with 1 extracts the least significant bit of the result of the right shift operation. 
        // This operation effectively isolates the i-th bit of num.
        tot += ((num >> i) & 1);
      }

      if (tot % 3 != 0) {
        // 1 << i: This is a bitwise left shift operation. It shifts the binary representation of the integer 1 to the left by i bits. For example, if i is 3, 1 << 3 would result in 0001 being shifted to 1000 (in binary), which is 8 in decimal.
        // ans |= (1 << i): This is a compound assignment operation combining the bitwise OR operator | with assignment =. It performs a bitwise OR operation between the current value of ans and the result of 1 << i, and then assigns the result back to ans. 
        // This operation effectively sets the i-th bit of ans to 1.
        res |= (1 << i);
      }
    }

    return res;
  }
}
```

