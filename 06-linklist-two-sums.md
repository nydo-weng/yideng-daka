# 1. 链表 - 两数相加

https://leetcode.cn/problems/add-two-numbers/description/

方法一: 标准写法, 针对低位在链表的前面, 逐位相加

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

function addTwoNumbers(l1: ListNode | null, l2: ListNode | null): ListNode | null {
    if (!l1 && !l2) return null;

    let head: ListNode|null = null;
    let prev: ListNode|null = null;

    let carry: number = 0;
    while (l1 || l2) {
        let l1val = l1 ? l1.val : 0;
        let l2val = l2 ? l2.val : 0;

        let sum = l1val + l2val + carry;

        carry = sum >= 10 ? 1 : 0;
        sum = sum % 10;

        const node = new ListNode(sum, null);

        head = head === null ? node : head;

        if (!prev) {
            prev = node;
        } else {
            prev.next = node;
            prev = node;
        }

        l1 = l1?.next;
        l2 = l2?.next;
    }
    if (carry === 1) {
        prev.next = new ListNode(1, null);
    }
    return head;
};

// 时间复杂度: 取决于长的那条链表, O(max(n, m))
// 空间复杂度: 不计算结果链表, O(1)
```

根据题解, 简写代码行数

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

function addTwoNumbers(l1: ListNode | null, l2: ListNode | null): ListNode | null {
    if (!l1 && !l2) return null;

    let head: ListNode|null = null;
    let prev: ListNode|null = null;

    let carry: number = 0;
    while (l1 || l2) {
        let l1val = l1 ? l1.val : 0;
        let l2val = l2 ? l2.val : 0;

        let sum = l1val + l2val + carry;
        carry = sum >= 10 ? 1 : 0;

        if (!head) {
            head = new ListNode(sum % 10, null);
            prev = head;
        } else {
            prev.next = new ListNode(sum % 10, null);
            prev = prev.next;
        }

        l1 = l1?.next;
        l2 = l2?.next;
    }
    if (carry === 1) {
        prev.next = new ListNode(1, null);
    }
    return head;
};
// 时间复杂度: 取决于长的那条链表, O(max(n, m))
// 空间复杂度: 不计算结果链表, O(1)
```

老袁视频解法

```Typescript
var addTwoNumbers = function (l1, l2) {
  let carry = 0;
  let res = new ListNode();
  let cur = res;
  while (l1 || l2 || carry) {
    let sum = (l1 ? l1.val : 0) + (l2 ? l2.val : 0) + carry;
    carry = sum >= 10 ? 1 : 0;
    cur.next = new LsitNode(sum % 10);
    cur = cur.next;
    l1 = l1 ? l1.next : l1;
    l2 = l2 ? l2.next : l2;
  }
  return res.next;
};
```
