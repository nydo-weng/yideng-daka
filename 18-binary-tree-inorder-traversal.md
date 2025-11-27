# 二叉树的中序遍历

https://leetcode.cn/problems/binary-tree-inorder-traversal/description/

方法一 递归

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

function inorderTraversal(root: TreeNode | null): number[] {
    if (!root) return [];
    return [...inorderTraversal(root.left), root.val, ...inorderTraversal(root.right)];
};
// 时间复杂度, 遍历所有节点 O(n)
// 空间复杂度, 递归栈高度, 树高度, 最坏链式结构 O(n), 最好 O(logn)
```

方法二 迭代, 看着题解来写的

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

function inorderTraversal(root: TreeNode | null): number[] {
    const res: number[] = [];
    const stack: TreeNode[] = [];

    while (root || stack.length) {
        while (root) {
            stack.push(root);
            root = root.left;
        }
        root = stack.pop();
        res.push(root.val);
        root = root.right;
    }
    return res;
};
// 时间复杂度 遍历所有节点 O(n)
// 空间复杂度 树的高度, 最坏 O(n), 最好 O(logn)
```
