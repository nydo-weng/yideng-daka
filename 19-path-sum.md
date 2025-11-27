# 路径综合

https://leetcode.cn/problems/path-sum/description/

方法一: 递归, 自己一遍就写出来了

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

function hasPathSum(root: TreeNode | null, targetSum: number): boolean {
    if (!root) return false;
    if (root.val === targetSum && !root.left && !root.right) return true;
    return hasPathSum(root.left, targetSum - root.val) || hasPathSum(root.right, targetSum - root.val);
};
// 时间复杂度, O(n) 遍历所有节点
// 空间复杂度, 递归栈深度, 树高度O(h), 最坏 O(n), 最好 O(logn)
```

方法二 迭代

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

function hasPathSum(root: TreeNode | null, targetSum: number): boolean {
    if (!root) return false;
    const stack: Array<[TreeNode, number]> = [[root, root.val]];

    while (stack.length) {
        const [node, sum] = stack.pop();

        if (!node.left && !node.right && sum === targetSum) return true;

        if (node.right) stack.push([node.right, node.right.val + sum]);

        if (node.left) stack.push([node.left, node.left.val + sum]);
    }
    return false;
};
// 时间复杂度, 遍历所有节点 O(n)
// 空间复杂度, 树高度, O(h)
```
