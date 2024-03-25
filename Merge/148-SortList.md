# 148. Sort List

给你链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表**。

 

**示例 1：**

![img](assets/sort_list_1.jpg)

```
输入：head = [4,2,1,3]
输出：[1,2,3,4]
```

**示例 2：**

![img](assets/sort_list_2.jpg)

```
输入：head = [-1,5,3,4,0]
输出：[-1,0,3,4,5]
```

**示例 3：**

```
输入：head = []
输出：[]
```

 

**提示：**

-   链表中节点的数目在范围 `[0, 5 * 104]` 内
-   `-105 <= Node.val <= 105`

 

**进阶：**你可以在 `O(n log n)` 时间复杂度和常数级空间复杂度下，对链表进行排序吗？



```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
  public ListNode sortList(ListNode head) {
    return sortList_r(head, null);
  }

  public ListNode sortList_r(ListNode head, ListNode tail) {
    if (head == null) {
      return null;
    }

    if (head.next == tail) {
      head.next = null;
      return head;
    }

    // 使用 Merge Sort 我们需要找到 List 的中点, 以中点为分界, 将链表拆分成两个 SubList
    // 寻找 List 的中点我们可以使用快慢指针的做法, 快指针每次移动 2 步, 慢指针每次移动 1 步
    // 当快指针到达 List 的末尾时, 慢指针指向的 List 的 Node 即为 List 的 中点
    ListNode slow = head, fast = head;
    while (fast != tail) {
      slow = slow.next;
      fast = fast.next;
      if (fast != tail) {
        fast = fast.next;
      }
    }

    ListNode mid = slow;
    ListNode left = sortList_r(head, mid);
    ListNode right = sortList_r(mid, tail);

    return merge(left, right);
  }

  public ListNode merge(ListNode head1, ListNode head2) {
    ListNode dummy = new ListNode(-1);
    ListNode ptr = dummy, ptr1 = head1, ptr2 = head2;
    while (ptr1 != null && ptr2 != null) {
      if (ptr1.val <= ptr2.val) {
        ptr.next = ptr1;
        ptr1 = ptr1.next;
      } else {
        ptr.next = ptr2;
        ptr2 = ptr2.next;
      }

      ptr = ptr.next;
    }

    if (ptr1 != null) {
      ptr.next = ptr1;
    }
    if (ptr2 != null) {
      ptr.next = ptr2;
    }

    return dummy.next;
  }
}
```

