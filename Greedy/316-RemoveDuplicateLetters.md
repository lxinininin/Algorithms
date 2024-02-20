# 316.Remove Duplicate Letters

给你一个字符串 `s` ，请你去除字符串中重复的字母，使得每个字母只出现一次。需保证 **返回结果的字典序最小**（要求不能打乱其他字符的相对位置）。

 

**示例 1：**

```
输入：s = "bcabc"
输出："abc"
```

**示例 2：**

```
输入：s = "cbacdcbc"
输出："acdb"
```

 

**提示：**

-   `1 <= s.length <= 104`
-   `s` 由小写英文字母组成



```java
class Solution {
  public String removeDuplicateLetters(String s) {
    int n = s.length();
    // this array is used to check 'sb' contains the character or not
    boolean[] vis = new boolean[26];
    
    // this array is used to count the occurrance of each character in 's'
    int[] occ = new int[26];
    for (int i = 0; i < n; i++) {
      occ[s.charAt(i) - 'a']++;
    }

    StringBuffer sb = new StringBuffer();
    for (int i = 0; i < n; i++) {
      char ch = s.charAt(i);

      // check if 'sb' does not contains 'ch'
      // if it does, that means 'ch' has already in its most appropriate position
      // here is an example: sb = "abc", ch = 'a'
      // it is not possible to remove the first 'a' and append to the last ("abc" < "bca")
      if (!vis[ch - 'a']) {
        // find the most appropriate position for 'ch'
        // here is an example: sb = "bc", ch = 'a'
        // and the rest of the 's' still contains character 'b' and 'c'
        // so in this iteration we can remove 'b' and 'c', and put 'a' into the first position of 'sb', this is what the iteration does
        // if sb = "a", ch = 'b'
        // whatever the rest of the 's' contains character 'a' or not
        // 'b' just append to the behind of 'a' ("ab" < "ba")
        while (sb.length() > 0 && sb.charAt(sb.length() - 1) > ch) {
          // check the rest of the 's' contains the 'ch' or not
          if (occ[sb.charAt(sb.length() - 1) - 'a'] > 0) {
            // if we remove the character in 'sb'
            // don't forget to set the position of the character in 'vis' to false
            // this means 'sb' no longer contains the removed character
            vis[sb.charAt(sb.length() - 1) - 'a'] = false;
            sb.deleteCharAt(sb.length() - 1);
          // if not, it means 'ch' can only be that position
          // then jump out of the iteration
          } else {
            break;
          }
        }

        // mark the 'sb' contains 'ch'
        sb.append(ch);
        vis[ch - 'a'] = true;
      }
			
      // whatever 'sb' contains 'ch' or not, the occurrance of 'ch' need to reduce
      occ[ch - 'a']--;
    }

    return sb.toString();
  }
}
```

