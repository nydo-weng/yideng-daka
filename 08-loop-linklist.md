# 1. 环形链表

https://leetcode.cn/problems/linked-list-cycle/description/

方法一: 自己写的, 用 set

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

function hasCycle(head: ListNode | null): boolean {
    const set = new Set<ListNode | null>();
    while (head) {
        if (set.has(head.next)) {
            return true;
        } else {
            set.add(head);
            head = head.next;
        }
    }
    return false;
};
// 时间复杂度: O(n)
// 空间复杂度: O(n)
```

方法二: 快慢指针

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

function hasCycle(head: ListNode | null): boolean {
    let slow = head;
    let fast = head;

    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow === fast) return true;
    }
    return false;
};
// 时间复杂度: 链表长度 n, 非环部分为 k, 环长度为 n-k, slow 入环后, 最多 n-k 次后相遇, 所以为 O(k+n-k) 也就是 O(n)
// 空间复杂度: 两个指针 O(1)
```
