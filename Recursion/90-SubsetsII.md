# 90. Subsets II

给你一个整数数组 `nums` ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。返回的解集中，子集可以按 **任意顺序** 排列。

 

**示例 1：**

```
输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

 

**提示：**

-   `1 <= nums.length <= 10`
-   `-10 <= nums[i] <= 10`



```java
class Solution {
  List<List<Integer>> res = new ArrayList<>();

  public List<List<Integer>> subsetsWithDup(int[] nums) {
    // 排序才能进行后面的操作
    Arrays.sort(nums);

    // 加入空集
    res.add(new ArrayList<Integer>());

    List<Integer> list = new ArrayList<>();
    // 把每种可能的子集长度都生成
    // ex: 1 4 5 子集长度会是 1, 2, 或者 3
    for (int i = 1; i <= nums.length; i++) {
      generate_subsets_r(nums, i, 0, list);
    }

    return res;
  }

  public void generate_subsets_r(int[] nums, int len, int index, List<Integer> list) {
    if (list.size() == len) {
      // 注意: list 一直在变, 这里必须新开一个 ArrayList 使其固定
      res.add(new ArrayList<>(list));
      return;
    }

    for (int i = index; i < nums.length; i++) {
      // 如果 nums 的元素没有重复的, 那么 sort 和这行代码可以省略
      // 若当前在此位置的数字与上次同样在此位置生成的数字相同, 则可以跳过当前生成的子集
      // 因为我们 nums 已经排序过了, 所以相同的数字都会凑在一起
      if (i > index && nums[i] == nums[i-1]) continue;
      
      list.add(nums[i]);
      generate_subsets_r(nums, len, i + 1, list);
      list.remove(list.size() - 1);
    }
  }
}
```

