# 207. Course Schedule

你这个学期必须选修 `numCourses` 门课程，记为 `0` 到 `numCourses - 1` 。

在选修某些课程之前需要一些先修课程。 先修课程按数组 `prerequisites` 给出，其中 `prerequisites[i] = [ai, bi]` ，表示如果要学习课程 `ai` 则 **必须** 先学习课程 `bi` 。

-   例如，先修课程对 `[0, 1]` 表示：想要学习课程 `0` ，你需要先完成课程 `1` 。

请你判断是否可能完成所有课程的学习？如果可以，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

```
输入：numCourses = 2, prerequisites = [[1,0]]
输出：true
解释：总共有 2 门课程。学习课程 1 之前，你需要完成课程 0 。这是可能的。
```

**示例 2：**

```
输入：numCourses = 2, prerequisites = [[1,0],[0,1]]
输出：false
解释：总共有 2 门课程。学习课程 1 之前，你需要先完成课程 0 ；并且学习课程 0 之前，你还应先完成课程 1 。这是不可能的。
```

 

**提示：**

-   `1 <= numCourses <= 2000`
-   `0 <= prerequisites.length <= 5000`
-   `prerequisites[i].length == 2`
-   `0 <= ai, bi < numCourses`
-   `prerequisites[i]` 中的所有课程对 **互不相同**



**Topological Sort + DFS**

```java
class Solution {
  int[] visited;
  List<List<Integer>> edges;
  boolean valid = true;

  public boolean canFinish(int numCourses, int[][] prerequisites) {
    // 我们用 0, 1, 2 分别表示未搜索, 搜索中, 已完成搜索
    visited = new int[numCourses];
    edges = new ArrayList<>();
    for (int i = 0; i < numCourses; i++) {
      edges.add(new ArrayList<Integer>());
    }

    // Load data
    for (int[] info : prerequisites) {
      edges.get(info[1]).add(info[0]);
    }

    // 我们用 DFS 去遍历和搜索每个点
    for (int i = 0; i < numCourses && valid; i++) {
      if (visited[i] == 0) {
        dfs(i);
      }
    }

    return valid;
  }

  public void dfs(int u) {
    visited[u] = 1;

    for (int v : edges.get(u)) {
      // 当 v 为未搜索状态(0)时, 我们开始搜索 v, 待搜索完成回溯到 u
      // 当 v 为搜索中(1)时, 那么说明图中出现了一个环, 因此不存在拓扑排序, 没有结果
      // 当 v 为已完成搜索(2)时, 说明 u 何时都不会影响 (u, v) 之前的拓扑关系, (意思是可以先把 v 以及 v 的前提课程学完再去安排 u), 以及不用进行任何操作
      if (visited[v] == 0) {
        dfs(v);
        if (!valid) return;
      }
      else if (visited[v] == 1) {
        valid = false;
        return;
      }
    }

    visited[u] = 2;
  }
}
```

