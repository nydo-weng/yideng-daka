# 前 K 个高频元素

https://leetcode.cn/problems/top-k-frequent-elements/description/

依然用最小堆来解决这个问题, 限制大小为 k, 堆顶是最小的元素. 如果超过 k 之后, 比较新的元素是不是比当前的小, 如果小, 不插入堆, 如果大, 插入堆, 把堆顶弹出, 重新排序堆

```Typescript
type Node = { num: number; freq: number };

class MyMinHeap {
    public data: Node[];
    constructor(data: Node[] = []) {
        this.data = [...data];
    }

    swap(index1: number, index2: number) {
        [this.data[index1], this.data[index2]] = [this.data[index2], this.data[index1]];
    }

    getLeftChildIndex(index: number): number {
        return index * 2 + 1;
    }

    getRightChildIndex(index: number): number {
        return index * 2 + 2;
    }

    getParentIndex(index: number): number {
        return Math.floor((index - 1) / 2);
    }

    bubbleUp(index: number) {
        // 边界条件, 如果已经是堆顶, 停止上浮
        if (index === 0) return;
        const parentIndex = this.getParentIndex(index);
        // 最小堆, 所以需要越高越小
        // 如果当前节点比父节点小, 上浮
        if (this.data[index].freq < this.data[parentIndex].freq) {
            // swap 节点, 在父节点 index 上再次调用 bubbleUp
            this.swap(index, parentIndex);
            this.bubbleUp(parentIndex);
        }
    }

    bubbleDown(index: number) {
        const heapSize = this.data.length;
        const leftChildIndex = this.getLeftChildIndex(index);
        const rightChildIndex = this.getRightChildIndex(index);
        // 三种情况, 当前节点 1. 没有子节点 2. 只有左子节点 3. 左右子节点都存在
        // 1. 没有子节点
        if (leftChildIndex >= heapSize) return;
        // 2. 只有左子节点
        if (rightChildIndex >= heapSize) {
            // 比较左子节点和当前节点的大小, 当前节点比左子节点大的话, 下沉
            if (this.data[index].freq > this.data[leftChildIndex].freq) {
                this.swap(leftChildIndex, index);
                // 因为只有左子节点, 说明 heap 已经到底了, 直接 return 就可以, 不需要进一步调用了
            }
            return;
        }
        // 3. 左右子节点都存在, 取更小的子节点与当前节点比较, 如果当前节点更小,
        const smallChildIndex = this.data[leftChildIndex].freq < this.data[rightChildIndex].freq ? leftChildIndex : rightChildIndex;
        if (this.data[index].freq > this.data[smallChildIndex].freq) {
            this.swap(index, smallChildIndex);
            this.bubbleDown(smallChildIndex);
        }
    }

    push(value: Node) {
        this.data.push(value);
        this.bubbleUp(this.data.length - 1);
    }

    pop(): Node | null {
        // 空堆
        if (this.data.length === 0) return null;
        // 堆只有一个元素
        if (this.data.length === 1) return this.data.pop()!;
        // 堆有两个及以上的元素
        this.swap(this.data.length - 1, 0); // 把堆顶和尾部元素互换位置
        // pop 尾部元素
        const res = this.data.pop();
        // 把堆顶 bubbleDown
        this.bubbleDown(0);
        return res;
    }

    peek(): Node | null {
        return this.data.length > 0 ? this.data[0] : null;
    }

    size(): number {
        return this.data.length;
    }
}

function topKFrequent(nums: number[], k: number): number[] {
    const map = new Map<number, number>();
    const myMinHeap = new MyMinHeap();
    nums.forEach(n => {
        map.set(n, (map.get(n) ?? 0) + 1);
    });
    map.forEach((value, key) => {
        if (myMinHeap.size() < k) {
            myMinHeap.push({num: key, freq: value});
        } else {
            if (myMinHeap.peek()!.freq < value) {
                myMinHeap.push({num: key, freq: value});
                myMinHeap.pop();
            }
        }
    });
    const res: number[] = [];
    myMinHeap.data.forEach(n => {
        res.push(n.num);
    });
    return res;
};
// 时间复杂度: 构建频次 map 为 O(n), 遍历 map（最多 n 个元素, 每次堆的 push / pop 为 O(log k), 因此为 O(n log k), 构建结果数组为 O(k), 总时间复杂度为 O(n log k).
// 空间复杂度: map 占用 O(n), 最小堆和结果数组各占用 O(k), 总空间复杂度为 O(n + k).
```
