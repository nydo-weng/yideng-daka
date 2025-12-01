# 133. 克隆图

https://leetcode.cn/problems/clone-graph/description/

最近才开始刷图的题, 感觉有点难度, 还没有找到方法.
这是自己思考后, 用 ai 稍微看了一下思路写的第一版本, 能过所有测试的

```Typescript
/**
 * Definition for _Node.
 * class _Node {
 *     val: number
 *     neighbors: _Node[]
 *
 *     constructor(val?: number, neighbors?: _Node[]) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.neighbors = (neighbors===undefined ? [] : neighbors)
 *     }
 * }
 *
 */


function cloneGraph(node: _Node | null): _Node | null {
    if (!node) return null;
    // 先不管边, 把所有的顶点本身拷贝一份, 并且做一个 map 映射
    const oldToNew = new Map<_Node, _Node>();
    // 拷贝过程中, 需要 vistied 和 queue 做辅助
    const visited = new Set<_Node>();
    const queue: _Node[] = [node];
    let ansNode: _Node | null = null;
    while (queue.length) {
        const currentNode = queue.shift();

        visited.add(currentNode);
        const nodeCopy = new _Node(currentNode.val, []);
        oldToNew.set(currentNode, nodeCopy);
        if (!ansNode) ansNode = nodeCopy;

        for (const neighbor of currentNode.neighbors) {
            if (!visited.has(neighbor)) {
                queue.push(neighbor)
            }
        }
    }

    // 再遍历一遍旧图, 遍历过程中给新图加边
    // 可以直接遍历 visited
    for (const oldNode of visited) {
        const newNode = oldToNew.get(oldNode);
        for (const oldNeighbor of oldNode.neighbors) {
            newNode.neighbors.push(oldToNew.get(oldNeighbor));
        }
    }
    return ansNode;
};
```

现在继续看 AI 的解释, 优化一下代码.
当前方法为 二阶段 BFS, 第一阶段只复制节点, 第二阶段遍历节点把边加上

```Typescript
/**
 * Definition for _Node.
 * class _Node {
 *     val: number
 *     neighbors: _Node[]
 *
 *     constructor(val?: number, neighbors?: _Node[]) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.neighbors = (neighbors===undefined ? [] : neighbors)
 *     }
 * }
 *
 */


function cloneGraph(node: _Node | null): _Node | null {
    if (!node) return null;
    // 这里 oldToNew 这个 map, 本身也能有记录 visited 的功能
    const oldToNew = new Map<_Node, _Node>();
    const queue: _Node[] = [node];

    while (queue.length) {
        const currentNode = queue.shift();
        // 如果这个节点已经访问过, 就不再继续访问
        if (oldToNew.has(currentNode)) continue;
        oldToNew.set(currentNode, new _Node(currentNode.val));
        for (const neighbor of currentNode.neighbors) {
            if (!oldToNew.has(neighbor)) {
                queue.push(neighbor);
            }
        }
    }

    // 再遍历一遍旧图, 遍历过程中给新图加边
    for (const [oldNode, newNode] of oldToNew.entries()) {
        for (const oldNeighbor of oldNode.neighbors) {
            newNode.neighbors.push(oldToNew.get(oldNeighbor));
        }
    }
    return oldToNew.get(node);
};
// 时间复杂度 O(v + e), 遍历所有的节点和边两次
// 空间复杂度 map 保存所有的顶点 O(v), queue 最多也保存所有定点 O(v), 新的图保存所有节点和边, O(v + e). 总空间复杂度 O(v + e)
```

单阶段 BFS

```Typescript
/**
 * Definition for _Node.
 * class _Node {
 *     val: number
 *     neighbors: _Node[]
 *
 *     constructor(val?: number, neighbors?: _Node[]) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.neighbors = (neighbors===undefined ? [] : neighbors)
 *     }
 * }
 *
 */


function cloneGraph(node: _Node | null): _Node | null {
    if (!node) return null;

    const oldToNew = new Map<_Node, _Node>();
    const queue: _Node[] = [node];

    oldToNew.set(node, new _Node(node.val));

    while(queue.length) {
        const oldNode = queue.shift();
        const newNode = oldToNew.get(oldNode);

        for (const oldNeighbor of oldNode.neighbors) {
            if (!oldToNew.has(oldNeighbor)) {
                oldToNew.set(oldNeighbor, new _Node(oldNeighbor.val));
                queue.push(oldNeighbor);
            }
            // 在这里, 新顶点所有的 neighbor 都已经创建了对应的新旧顶点映射
            // 不存在的也已经建立, 所以直接加入新顶点的边种即可
            // 不用担心重复, 因为只有当一个邻居节点没有映射时才创建,
            // 并且每个节点之后被循环到一次
            newNode.neighbors.push(oldToNew.get(oldNeighbor));
        }
    }
    return oldToNew.get(node);
};
// 时间复杂度 O(v + e), 访问每个顶点和边各一次
// 空间复杂度 map 为 O(v), 新图为 O(v + e), 共 O(v + e)
```

DFS 写法

```Typescript
/**
 * Definition for _Node.
 * class _Node {
 *     val: number
 *     neighbors: _Node[]
 *
 *     constructor(val?: number, neighbors?: _Node[]) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.neighbors = (neighbors===undefined ? [] : neighbors)
 *     }
 * }
 *
 */


function cloneGraph(node: _Node | null): _Node | null {
    if (!node) return null;

    const map = new Map<_Node, _Node>();

    const dfs = (oldNode: _Node): _Node => {
        // 如果当前节点已经复制, 返回复制的新节点
        if (map.has(oldNode)) return map.get(oldNode);
        // 复制当前节点
        const newNode = new _Node(oldNode.val)
        map.set(oldNode, newNode);

        // 复制所有边
        for (const oldNB of oldNode.neighbors) {
            const newNB = dfs(oldNB);
            newNode.neighbors.push(newNB);
        }
        return newNode;
    }

    return dfs(node);
};
// 时间复杂度 O(v + e), 所有顶点和边都要被检查一遍
// 空间复杂度 调用栈最多为 顶点数 O(v), map 为 顶点数O(v), 新图 O(v + e), 总的O(v + e)
```
