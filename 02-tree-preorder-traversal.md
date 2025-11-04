# 利用栈进行二叉树前序遍历[leetcode 题号 144]

请在小打卡准确描述实现代码时间复杂度和空间复杂度~

1. 递归解法

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

function preorderTraversal(root: TreeNode | null): number[] {
  // 当前 node 为 null 时, 停止递归
  if (root === null) return []
  // 前序遍历, 跟节点先遍历
  return [root.val, ...preorderTraversal(root.left), ...preorderTraversal(root.right)]
};
```

**时间复杂度**: 每个节点都被访问一次, 每次访问都是常数操作, 所以 O(n)  
**空间复杂度**: 两部分, 1. 结果数组, 存储所有节点 O(n), 2. 递归调用栈, 最坏情况递归深度为 n 所以 O(n). 因此, 总体空间复杂度 O(n)

2. 用栈的解法

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

function preorderTraversal(root: TreeNode | null): number[] {
    let stack: (TreeNode|null)[] = [root];
    let ans: number[] = [];
    while (stack.length !== 0) {
        const currentNode: TreeNode | null = stack.pop();
        if (currentNode === null) continue;
        ans.push(currentNode.val)
        // 后进先出, 先推右 node, 再推左 node, 先 expand 左 node
        stack.push(currentNode.right);
        stack.push(currentNode.left);
    }
    return ans;
};
```

**时间复杂度**: 每个节点都被访问一次, 每次访问都是常数操作, 所以 O(n)  
**空间复杂度**: 两部分, 1. 结果数组, 存储所有节点 O(n), 2. 辅助栈, 纯链状的树->O(1), 每次读下一个节点上一个节点已经出栈, 栈大小不超过 1; 平衡二叉树, O(logn); 最坏情况, 向左下遍历时, 每个节点都有一个右节点, 栈大小最坏为数的深度, 1/2 n, 即 O(n). 总的来说, 空间复杂度还是 O(n).
