# 二叉树的最大深度

https://leetcode.cn/problems/maximum-depth-of-binary-tree/description/

方法一: DFS 递归, 自己写出来了, 一次过没 bug, 并且也算是实践中的最优解

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

function maxDepth(root: TreeNode | null): number {
    if (!root) return 0;
    const leftDepth = maxDepth(root.left);
    const rightDepth = maxDepth(root.right);
    const maxD = leftDepth > rightDepth ?  leftDepth : rightDepth;
    return maxD + 1;
};
// 时间复杂度 O(n), 遍历所有节点
// 空间复杂度 O(h), 最坏 O(n), 链式结构
```

方法二: BFS 迭代 + queue

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

function maxDepth(root: TreeNode | null): number {
    if (!root) return 0;
    let ans = 0;
    const queue: TreeNode[] = [root];

    while (queue.length) {
        const size = queue.length;
        for (let i = 0; i < size; i++) {
            const node = queue.shift();
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }
        ans++;
    }
    return ans;
};
// 时间复杂度 O(n) 遍历所有节点
// 空间复杂度 O(w) 等于最宽的 O(n/2) O(n)
```

方法三: dfs 迭代 + stack

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

function maxDepth(root: TreeNode | null): number {
    if (!root) return 0;
    let ans = 0;
    const stack: [TreeNode, number][] = [[root, 1]];

    while (stack.length) {
        const [node, currentDepth] = stack.pop();
        ans = ans > currentDepth ? ans : currentDepth;
        if (node.right) stack.push([node.right, currentDepth + 1]);
        if (node.left) stack.push([node.left, currentDepth + 1]);
    }
    return ans;
};
// 时间复杂度 O(n)
// 空间复杂度 O(h)
```
