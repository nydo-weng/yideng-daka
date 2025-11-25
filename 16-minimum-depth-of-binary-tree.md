# 二叉树的最小深度

https://leetcode.cn/problems/minimum-depth-of-binary-tree/description/

方法一: 递归, 自己第一次写的有问题, 找到坑之后, 自己改出来的一版

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

function minDepth(root: TreeNode | null): number {
    if (!root) return 0;
    let depth: number | null = null;
    // 如该节点没有左节点, 走右节点去叶节点
    if (!root.left) depth = minDepth(root.right)
    // 如该节点没有右节点, 走右节点去叶节点
    if (!root.right) depth = minDepth(root.left)

    if (depth) {
        return depth + 1;
    } else {
        const leftDepth = minDepth(root.left);
        const rightDepth = minDepth(root.right);
        return leftDepth < rightDepth ? leftDepth + 1 : rightDepth + 1;
    }
};
```

看了题解后优化了写法

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

function minDepth(root: TreeNode | null): number {
    if (!root) return 0;

    // 如该节点没有左节点, 走右节点去叶节点
    if (!root.left) return minDepth(root.right) + 1;
    // 如该节点没有右节点, 走右节点去叶节点
    if (!root.right) return minDepth(root.left) + 1;

    return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
};
// 时间复杂度 O(n), 遍历所有节点
// 空间复杂度 O(h), 最坏链形 O(n), 最好 O(logn)
```

方法二: BFS + queue, 这个是这一题的最优解

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

function minDepth(root: TreeNode | null): number {
    if (!root) return 0;
    const queue: TreeNode[] = [root];
    let depth = 0;
    while (queue.length) {
        const size = queue.length;
        depth++;
        for (let i = 0; i < size; i++) {
            const node = queue.shift();
            if (!node.left && !node.right) return depth;
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }
    }
};
// 时间复杂度 O(n), 最坏遍历所有节点
// 空间复杂度 O(w), 树的最大宽度, 最坏完全二叉树 O(n/2) = O(n)
```
