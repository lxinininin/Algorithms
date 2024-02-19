[](https://leetcode.cn/problems/merge-k-sorted-lists/description/)



<img src="assets/Merge.png" alt="Merge" style="zoom:50%;" />  



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
  public ListNode mergeKLists(ListNode[] lists) {
    return merge_r(lists, 0, lists.length - 1);
  }

  // this method is what we showed the picture
  // similar as the MergeSort
  public ListNode merge_r(ListNode[] lists, int l, int r) {
    if (l > r) {
      return null;
    }

    if (l == r) return lists[l];

    int mid = (l + r) / 2;

    return mergeTwoLists(merge_r(lists, l, mid), merge_r(lists, mid+1, r));
  }
  
  public ListNode mergeTwoLists(ListNode a, ListNode b) {
    ListNode head = new ListNode(0);
    ListNode tail = head;

    while (a != null && b != null) {
      if (a.val <= b.val) {
        tail.next = a;
        a = a.next;
        tail = tail.next;
      } else {
        tail.next = b;
        b = b.next;
        tail = tail.next;
      }
    }

    if (a != null) {
      tail.next = a;
    }
    if (b != null) {
      tail.next = b;
    }

    return head.next;
  }
}
```

