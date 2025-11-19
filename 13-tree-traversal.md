# 使用 JavaScript/TypeScript 实现树的深度优先遍历和广度优先遍历~

## 广度优先

https://leetcode.cn/problems/n-ary-tree-level-order-traversal/?envType=problem-list-v2&envId=tree
N 叉树的层序遍历

自己写的 能过但是逻辑小乱  
又过了一遍 感觉逻辑还是挺清楚的, 但是看一下有没有更好的方法

```Typescript
/**
 * Definition for _Node.
 * class _Node {
 *     val: number
 *     children: _Node[]
 *
 *     constructor(v: number) {
 *         this.val = v;
 *         this.children = [];
 *     }
 * }
 */


function levelOrder(root: _Node | null): number[][] {
    if (!root) return [];
	  let prevLevel: (_Node | null)[] = [];
    prevLevel.push(root);

    let nextLevel: (_Node | null)[] = [];

    const ans: number[][] = [];
    let currentLevel: number[] = [];
    while (prevLevel.length) {
        const node = prevLevel.shift();
        if (node) {
            currentLevel.push(node.val)
            for (const nodeChild of node.children) {
                if (nodeChild) {
                    nextLevel.push(nodeChild);
                }
            }
        }
        if (!prevLevel.length) {
            prevLevel = nextLevel;
            nextLevel = [];
            ans.push(currentLevel);
            currentLevel = [];
        }
    }
    return ans;
};
```

用 queue + size 确实整个逻辑都很清晰

```Typescript
/**
 * Definition for _Node.
 * class _Node {
 *     val: number
 *     children: _Node[]
 *
 *     constructor(v: number) {
 *         this.val = v;
 *         this.children = [];
 *     }
 * }
 */


function levelOrder(root: _Node | null): number[][] {
    if (!root) return [];

    const ans: number[][] = [];
    const queue: _Node[] = [root];

    let levelVal: number[] = [];
    while (queue.length) {
        const size = queue.length;

        for (let i = 0; i < size; i++) {
            const node = queue.shift();
            levelVal.push(node.val);
            for (const child of node.children) {
                if (child) {
                    queue.push(child)
                }
            }
        }

        ans.push(levelVal);
        levelVal = [];
    }
    return ans;
};
// 时间复杂度 O(n), 每个节点遍历一次
// 空间复杂度 O(n), 扁平树, 几乎所有节点都在队列中
```

二叉树:  
把遍历 children 变成 left right

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

    const ans: number[][] = [];
    const queue: TreeNode[] = [root];

    let levelVal: number[] = [];
    while (queue.length) {
        const size = queue.length;

        for (let i = 0; i < size; i++) {
            const node = queue.shift();
            levelVal.push(node.val);
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }

        ans.push(levelVal);
        levelVal = [];
    }
    return ans;
};
// 时间复杂度 O(n), 每个节点遍历一次
// 空间复杂度 取决于最宽的一层, n/2, 所以是 O(n/2) = O(n)
```

## 深度优先

### 前序

前序 N 叉树
自己写的 用 AI debug 之后

```Typescript
/**
 * Definition for _Node.
 * class _Node {
 *     val: number
 *     children: _Node[]
 *
 *     constructor(val?: number, children?: _Node[]) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.children = (children===undefined ? [] : children)
 *     }
 * }
 */


function preorder(root: _Node | null): number[] {
    if (!root) return []
    const ans: number[] = [root.val];
    const children = root.children;
    for (const node of children) {
        ans.push(...preorder(node));
    }
    return ans;
};
// 时间复杂度: O(n) 每个节点递归一次
// 空间复杂度: 最坏是 O(n), 链式结构; 通用表示为树的高度 O(h)
```

前序换到二叉树, 就是不用展开 children, 用 left right

**说早期的 vue 的 dom diff 就是这么来的**

```typescript
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

  return [
    root.val,
    ...preorderTraversal(root.left),
    ...preorderTraversal(root.right),
  ];
}
// 时间复杂度: O(n) 每个节点递归一次
// 空间复杂度: O(h) 树的高度, 最坏 O(n), 最好O(logn)
```

### 后序

后序 N 叉树, 直接写

```Typescript
/**
 * Definition for node.
 * class _Node {
 *     val: number
 *     children: _Node[]
 *     constructor(val?: number) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.children = []
 *     }
 * }
 */

function postorder(root: _Node | null): number[] {
    if (!root) return [];
    const ans: number[] = [];
    const children = root.children;
    for (const node of children) {
        ans.push(...postorder(node));
    }
    ans.push(root.val);
    return ans;
};
// 时间复杂度: O(n) 每个节点递归一次
// 空间复杂度: O(h) 树的高度, 最坏 O(n), 最好 O(logn), 多叉树是 log 以 k 为底 n, k 是子节点数量
```

后序 二叉树

```typescript
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
  return [
    ...postorderTraversal(root.left),
    ...postorderTraversal(root.right),
    root.val,
  ];
}
// 时间复杂度: O(n) 每个节点递归一次
// 空间复杂度: O(h) 树的高度, 最坏 O(n), 最好 O(logn)
```

### 中序

N 叉树没有, 二叉树有  
在二叉树中比较重要, 因为严格递增

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
// 时间复杂度: O(n) 每个节点递归一次
// 空间复杂度: O(h) 树的高度, 最坏 O(n), 最好 O(logn)
```
