# 合并 K 个升序链表

https://leetcode.cn/problems/merge-k-sorted-lists/description/

```Typescript
class MyMinHeap {
    public data: ListNode[];
    constructor(data: ListNode[] = []) {
        this.data = [...data];
    }

    swap(index1: number, index2: number) {
        [this.data[index1], this.data[index2]] = [this.data[index2], this.data[index1]];
    }

    getLeftChildIndex(index: number) {
        return 2 * index + 1;
    }

    getRightChildIndex(index: number) {
        return 2 * index + 2;
    }

    getParentIndex(index: number) {
        return Math.floor((index - 1) / 2);
    }

    bubbleUp(index: number) {
        const parentIndex = this.getParentIndex(index);
        // 当新元素入堆时， 依次与父节点比较， 如果当前节点更小即上浮， 如果当前节点已在堆顶停止
        if (index === 0) return;
        if (this.data[index].val < this.data[parentIndex].val) {
            this.swap(index, parentIndex);
            this.bubbleUp(parentIndex);
        }
    }

    bubbleDown(index: number) {
        // 当堆顶弹出后， 把堆尾部元素提到堆顶， 依次下沉
        // 3种情况， 1. 没有子节点 2. 只有左子节点 3. 左右子节点都存在
        const leftChildIndex = this.getLeftChildIndex(index);
        const rightChildIndex = this.getRightChildIndex(index);
        const heapSize = this.data.length;
        if (leftChildIndex >= heapSize) return;
        if (rightChildIndex >= heapSize) {
            // 只需要比较和左子节点相比， 当前节点比左子节点大， 下沉
            // 又因为堆的性质， 如果只有左子节点， 不需要进一步下沉， 已经到堆尾了， 没有更多的元素需要检查
            if (this.data[index].val > this.data[leftChildIndex].val) {
                this.swap(index, leftChildIndex);
            }
            return;
        }
        const smallChildIndex = this.data[leftChildIndex].val < this.data[rightChildIndex].val ? leftChildIndex : rightChildIndex;
        if (this.data[index].val > this.data[smallChildIndex].val) {
            this.swap(index, smallChildIndex);
            this.bubbleDown(smallChildIndex);
        }
    }

    push(node: ListNode) {
        this.data.push(node);
        this.bubbleUp(this.data.length - 1);
    }

    pop(): ListNode | null {
        // 1. 堆空， 返回null 2. 堆只有一个元素， 直接pop 3. 堆有一个以上元素， pop， 提堆尾到堆顶， bubbleDown
        if (this.data.length === 0) return null;
        if (this.data.length === 1) {
            return this.data.pop();
        }
        const res = this.data[0];
        this.swap(0, this.data.length - 1);
        this.data.pop();
        this.bubbleDown(0);
        return res;
    }
}

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

function mergeKLists(lists: Array<ListNode | null>): ListNode | null {
    const myHeap = new MyMinHeap();
    lists.forEach(node => {
        // 每个链表的头都推入堆
        if (node != null) {
            myHeap.push(node);
        }
    });
    if (myHeap.data.length === 0) return null;
    // 因为每个链表都是升序排列过得, 所以最小的在堆顶
    const dummy = new ListNode(0);
    let current = dummy;
    while (myHeap.data.length !== 0) {
        const node = myHeap.pop()!;
        current.next = node;
        current = node;
        if (node.next != null) {
            myHeap.push(node.next);
        }
    }
    return dummy.next;
};
// 时间复杂度: 链表头推入堆 O(k), pop() 和 push() 都是 log(k) 的操作, 对 n 个 node 都有机会操作, 所以 O(nlogk)
// 空间复杂度: 堆大小 O(k)
```
