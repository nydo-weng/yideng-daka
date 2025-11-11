# 1. 删除排序链表中的重复元素

https://leetcode.cn/problems/remove-duplicates-from-sorted-list/

方法一: 自己写的 双指针? 逻辑有点绕

```Typescript
function deleteDuplicates(head: ListNode | null): ListNode | null {
    if (!head) return null;

    let prev = head;
    let cur = head;

    while (cur) {
         if (cur.next?.val === prev.val) {
            cur = cur.next;
         } else {
            prev.next = cur.next;
            cur = cur.next;
            prev = prev.next;
         }
    }
    return head;
};
```

方法二: 还是自己写的双指针, 但是语义逻辑上更加清楚好理解

```Typescript
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     val: number
 *     next: ListNode | null
 *     constructor(val?: number, next?: ListNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.next = (next===undefined ? null : next)
 *     }
 * }
 */

function deleteDuplicates(head: ListNode | null): ListNode | null {
    if (!head) return null;

    let prev = head;
    let cur = head.next;

    while (cur) {
        // 只有当 cur 不是 null 的时候才进入
         if (cur.val === prev.val) {
            cur = cur.next;
         } else {
            prev.next = cur;

            prev = prev.next;
            cur = cur.next;
         }
    }
    // 会缺失结尾的 null, 补上
    prev.next = null;

    return head;
};
// 时间复杂度, 遍历链表 O(n)
// 空间复杂度, O(1)
```

方法三: GPT 给的解, 更加清楚, 双指针合一

```Typescript
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     val: number
 *     next: ListNode | null
 *     constructor(val?: number, next?: ListNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.next = (next===undefined ? null : next)
 *     }
 * }
 */

function deleteDuplicates(head: ListNode | null): ListNode | null {
    if (!head) return null;

    let cur = head;

    while (cur && cur.next) {
        if (cur.val === cur.next.val) {
            cur.next = cur.next.next;
        } else {
            cur = cur.next;
        }
    }

    return head;
};
```
