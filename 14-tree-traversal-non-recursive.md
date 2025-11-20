# 使用 JavaScript/TypeScript 实现树的先中后序遍历（不能使用递归）

## 前序

自己写出来了

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
    if (!root) return [];

    const ans: number[] = [];
    const stack: (TreeNode | null)[] = [root];

    while (stack.length) {
        const node = stack.pop();
        ans.push(node.val);
        if (node.right) stack.push(node.right)
        if (node.left) stack.push(node.left);
        // 先推右, 再推左, 然后 pop, 这样就先访问左, 并且如果左有子树, 先展开子树
    }
    return ans;
};
// 时间复杂度, O(n), 遍历一遍所有的节点
// 空间复杂度, O(h), 取决于树的高度, 最坏情况链式结构 O(n)
```

## 中序

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
    const ans: number[] = [];
    const stack: (TreeNode|null)[] = [];

    while (root || stack.length) {
        while (root) {
            stack.push(root);
            root = root.left;
        }
        root = stack.pop();
        ans.push(root.val);
        root = root.right;
    }
    return ans;
};
// 时间复杂度: O(n)
// 空间复杂度: O(h)
```

## 后序

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

function postorderTraversal(root: TreeNode | null): number[] {
    if (!root) return [];

    const ans: number[] = [];
    const stack: TreeNode[] = [];

    // 记录上一个访问节点
    let lastVisit: TreeNode | null = null
    while (root || stack.length) {
        while (root) {
            stack.push(root);
            root = root.left;
        }
        const peekNode = stack[stack.length - 1];
        if (!peekNode.right || peekNode.right === lastVisit) {
            const node = stack.pop();
            ans.push(node.val);
            lastVisit = node;
            root = null; // 防止下一次进 while 访问左子树, 因为到这里就是当前节点左右子树已经访问完了
        } else {
            // 只有当当前节点的右子树不为空, 并且没有访问, 就到这, 开始访问右子树
            root = peekNode.right;
        }
    }
    return ans;
};
// 时间复杂度: O(n)
// 空间复杂度: O(h)
```
