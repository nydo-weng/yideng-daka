# 图的广度优先和深度优先遍历

深度优先遍历, 记忆优化

```Typescript
type Graph = Record<string, string[]>;
// 深度优先遍历, 记忆化优化
function dfsMemoization(graph: Graph, start: string): void {
    const visited = new Set<string>();

    function dfs(node: string): void {
        if (visited.has(node)) return;
        visited.add(node);
        console.log(node);

        for (const neighbor of graph[node]) {
            dfs(neighbor);
        }
    }

    dfs(start);
}
// 时间复杂度: O(V + E), 顶点数 + 边数, 因为每个节点最多被访问一次, 并且每个边也只会被检查一次
// 空间复杂度: O(v), visited 储存所有顶点 v 个, 调用栈在链式结构时深度也是 v

const graph1: Graph = {
    A: ['B', 'C'],
    B: ['D', 'E'],
    C: ['F'],
    D: [],
    E: [],
    F: [],
}
console.log("graph1")
dfsMemoization(graph1, "A");

const graph2: Graph = {
    A: ['B', 'C'],
    B: ['A', 'D'],
    C: ['A', 'F'],
    D: ['B', 'E', 'H', 'G'],
    E: ['F', 'D', 'H'],
    F: ['C', 'E'],
    G: ['D'],
    H: ['D', 'E'],
};
    //     C -------- F
    //    /            \
    //   /              \
    //  A                E
    //   \              / \
    //    \            /   \
    //     B -------- D ---- H
    //                \
    //                 \
    //                  G
console.log("graph2")
dfsMemoization(graph2, "B");
```

广度优先遍历, 优先队列优化

```Typescript
type Graph = Record<string, string[]>;
// 广度优先遍历 使用优先级队列优化
function bfsPriorityQueue(graph: Graph, start: string): void {
    const visited = new Set<string>();
    const queue: string[] = [];
    queue.push(start);

    while(queue.length) {
        const node = queue.shift()!;  // ! 表示 Non-null assertion, 非空断言, 编码者保证这里不会是 undefined
        if (visited.has(node)) continue;
        visited.add(node);
        console.log(node);

        // 根据优先级排序
        // const neighbors = graph[node].sort();
        const neighbors = graph[node];
        for (const neighbor of neighbors) {
            queue.push(neighbor);
        }
    }
}
// 时间复杂度: 不考虑排序 O(V + E)
// 空间复杂度: O(V), visited 最多放入所有顶点 v 个, queue 也最多 v 个

const graph1: Graph = {
    A: ['B', 'C'],
    B: ['D', 'E'],
    C: ['F'],
    D: [],
    E: [],
    F: [],
}
console.log("graph1")
bfsPriorityQueue(graph1, "A");

const graph2: Graph = {
    A: ['B', 'C'],
    B: ['A', 'D'],
    C: ['A', 'F'],
    D: ['B', 'E', 'H', 'G'],
    E: ['F', 'D', 'H'],
    F: ['C', 'E'],
    G: ['D'],
    H: ['D', 'E'],
};
    //     C -------- F
    //    /            \
    //   /              \
    //  A                E
    //   \              / \
    //    \            /   \
    //     B -------- D ---- H
    //                \
    //                 \
    //                  G
console.log("graph2")
bfsPriorityQueue(graph2, "B");
```
