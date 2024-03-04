# 131. Palindrome Patitioning

给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。

**回文串** 是正着读和反着读都一样的字符串。

 

**示例 1：**

```
输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]
```

**示例 2：**

```
输入：s = "a"
输出：[["a"]]
```

 

**提示：**

-   `1 <= s.length <= 16`
-   `s` 仅由小写英文字母组成



```java
class Solution {
  List<List<String>> res = new ArrayList<>();
  List<String> ans = new ArrayList<>();
  int n;
  int[][] mat;

  public List<List<String>> partition(String s) {
    n = s.length();
    // 我们在判断 s[i...j] 是否为回文串时, 常规的方法是使用双指针分别指向 i 和 j, 每次判断两个指针指向的字符是否相同, 直到两个指针相遇
    // 然而这种方法会产生重复计算, 例如 s = aaba, 对于前 2 个字符 aa, 我们有 2 种分割方法 [aa] 和 [a, a], 当我们每一次搜索到字符串的第 i = 2 个字符 b 时, 都需要对于每个 s[i...j] 使用双指针判断其是否为回文串, 这就产生了重复计算
    // 所以我们可以将字符串 s 的每个字串 s[i...j] 是否为回文串用 matrix 记录下来, mat[i][j] 表示 s[i...j] 是否为回文串
    mat = new int[n][n];

    partition_r(s, 0);

    return res;
  }

  public void partition_r(String s, int index) {
    if (index == n) {
      res.add(new ArrayList<String>(ans));
      return;
    }

    // 当我们当前搜索到字符串的第 index 个字符, 且 s[0...index-1] 位置的所有字符已经被分割成若干个回文串, 并且分割的结果已经放入了 ans 中, 那么我们只需要枚举下一个回文串的右边界 i, 使得 s[index...i] 是一个回文串
    for (int i = index; i < n; i++) {
      // 如果 s[index, i] 是回文串, 那么就将其加入 ans, 并以 i+1 作为新的 index 进行下一层搜索, 并在未来的回溯时将 s[index...i] 从 ans 中移除
      if (isPalindrome(s, index, i) == 1) {
        ans.add(s.substring(index, i + 1));
        partition_r(s, i + 1);
        ans.remove(ans.size() - 1);
      }
    }       
  }

  // mat[i][j] = 0 表示未搜索, 1 表示是回文串, -1 表示不是回文串
  public int isPalindrome(String s, int i, int j) {
    if (mat[i][j] != 0) {
      return mat[i][j];
    }
    // i > j 的情况出现在偶数长度的 s 中, 当最中间的两个数相同时, 那么在下一次 i 就会大于 j 了
    else if (i >= j) {
      mat[i][j] = 1;
    }
    else if (s.charAt(i) == s.charAt(j)) {
      // 当 s 长度为偶数时, 当最中间的两个数相同时, isPalindrome 里面, i+1 > j-1, 所以前面得考虑 i > j 的情况
      mat[i][j] = isPalindrome(s, i + 1, j - 1);
    }
    else {
      mat[i][j] = -1;
    }

    return mat[i][j];
  }
}
```

