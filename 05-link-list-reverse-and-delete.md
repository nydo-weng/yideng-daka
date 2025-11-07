链表相关概念 -> react 的 fiber, fiber tree, fiber dom.

# 1. 反转链表

https://leetcode.cn/problems/reverse-linked-list/description/

方法一: 值反转, 利用栈

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

function reverseList(head: ListNode | null): ListNode | null {
    if (!head) return null;

    const stack: number[] = [];
    let node = head;

    while (node) {
        stack.push(node.val);
        node = node.next;
    }
    node = head;
    while (node) {
        node.val = stack.pop();
        node = node.next;
    }
    return head;
};

// 时间复杂度, 链表长度为n, 遍历链表两遍, 时间复杂度为 O(n)
// 空间复杂度, 一个 stack 用来存放等同于链表长度的 val, 空间复杂度为 O(n)
```

方法二: 双指针/原地反转

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

function reverseList(head: ListNode | null): ListNode | null {
    let current = head;
    let prev: ListNode | null = null;
    while (current) {
        const tempNext = current.next;
        current.next = prev;
        prev = current;
        current = tempNext;
    }
    return prev;
};
// 时间复杂度, 遍历一遍链表, O(n)
// 空间复杂度, 原地反转, O(1)
```

方法三: 正常递归

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

function reverseList(head: ListNode | null): ListNode | null {
    if (!head || !head.next) return head;  // 递归出口, 最后作为 newHead 层层上传

    const newHead = reverseList(head.next)  // 递归到底, 就获得了上面的 head, 然后原封不动层层上传

    head.next.next = head;  // 反转链接, 让当前节点的下一个节点的子节点, 变成当前节点
    head.next = null;  // 当前节点的下一个节点为空, 如果还有, 会在下一个递归中被覆盖, 如果没有, 链表的结尾确实应该为 null

    return newHead;  // 传递新的链表头节点


};

// 时间复杂度, 每个节点遍历一遍, 内部操作常数级, 所以 O(n)
// 空间复杂度, 递归栈深度为链表长度, O(n)
```

还有一个尾递归, 没有仔细看, 数学上等价, 但是 JS/TS 对尾递归没有优化.

# 2. 删除列表中的节点

https://leetcode.cn/problems/delete-node-in-a-linked-list/submissions/676573886/

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

/**
 Do not return anything, modify it in-place instead.
 */
function deleteNode(node: ListNode | null): void {
    if (!node) return;
    // 要删除的节点不会是末尾节点, 如果可以是的话, 还要加以下边界条件
    // if (!node.next) {
    //     node = null;
    //     return;
    // }
    node.val = node.next.val;
    node.next = node.next.next;
};

// 时间复杂度 O(1)
// 空间复杂度 O(1)
```
