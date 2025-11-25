# 二叉树的层序遍历

https://leetcode.cn/problems/binary-tree-level-order-traversal/description/

方法一 queue, 之前写过, 当复习; 这次能直接自己手敲出来

```Typescript
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     val: number
 *     left: TreeNode | null
 *     right: TreeNode | null
 *     constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.left = (left===undefined ? null : left)
 *         this.right = (right===undefined ? null : right)
 *     }
 * }
 */

function levelOrder(root: TreeNode | null): number[][] {
    if (!root) return [];

    const queue: TreeNode[] = [root];
    const res: number[][] = [];
    while (queue.length) {
        const size = queue.length;
        const levelRes: number[] = [];
        for (let i = 0; i < size; i++) {
            const node = queue.shift();
            levelRes.push(node.val);
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }
        res.push(levelRes);
    }
    return res;
};
// 时间复杂度 O(n) 遍历所有节点
// 空间复杂度 O(w), 完全二叉树宽度为O(n/2) 所以 O(n)
```

用 head 指针, 避免使用 shift(), 节省开销

```Typescript
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     val: number
 *     left: TreeNode | null
 *     right: TreeNode | null
 *     constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.left = (left===undefined ? null : left)
 *         this.right = (right===undefined ? null : right)
 *     }
 * }
 */

function levelOrder(root: TreeNode | null): number[][] {
    if (!root) return [];

    const queue: TreeNode[] = [root];
    let head = 0;
    const res: number[][] = [];
    while (head < queue.length) {
        // 第二次到这的时候, length 加上了还未遍历的 node,
        // 取和 head 的 diff, 得到当前层大小
        const levelSize = queue.length - head;
        const levelRes: number[] = [];
        // 遍历当前层大小次, 然后下一层
        for (let i = 0; i < levelSize; i++) {
            const node = queue[head++];
            levelRes.push(node.val);
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }
        res.push(levelRes);
    }
    return res;
};
// 时间复杂度 O(n) 遍历所有节点
// 空间复杂度 O(w), 完全二叉树宽度为O(n/2) 所以 O(n)
```
