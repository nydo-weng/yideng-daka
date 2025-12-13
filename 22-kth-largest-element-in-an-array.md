# 数组中的第 K 个最大元素

https://leetcode.cn/problems/kth-largest-element-in-an-array/description/

方法 1: 直接排序 返回第 k-1 个元素, 这个方法没有考虑力扣题目中要求的复杂度

```Typescript
function findKthLargest(nums: number[], k: number): number {
    // 先不管复杂度
    nums.sort((a,b) => b - a);
    return nums[k-1];
};
// 时间复杂度: O(nlogn)
// 空间复杂度: O(1)
```

^ 这个方式面试中不会允许使用, 因为使用了 API.

方法 2: 快排, 老袁视频中也提到了这个. 最坏是 O(n^2), 正常就是 O(n)

方法 3: 用堆来解决. 老袁视频中主要想用的方法.

## 堆的理解

堆是一种 特殊的 完全二叉树  
完全二叉树是一种 二叉树, 除了最后一层的叶子节点可以不满外, 其余层的节点数都达到最大值, 并且叶子节点都集中在最后一层或倒数第二层.  
特殊的完全二叉树通常值具有以下特点的完全二叉树:

1. 完美二叉树(Perfect Binary Tree): 每一层都是满的, 即每个节点都有两个子节点, 直到最后一层的叶子节点.
2. 完全二叉树(Complete Binary Tree): 除了最后一层的叶子节点外, 其它层都是满的, 并且叶子节点都集中在最后一层或倒数第二层. **还有一个需要注意的重点: 完全二叉树是 _从左到右填满每一层, 最后一层尽量靠左_**

这些特殊的完全二叉树具有一些有趣的性质, 使得它们在某些算法和数据结构中非常有用.  
例如, 堆数据结构常常使用完全二叉树来实现, 因为完全二叉树的性质可以保证堆的高效性和简单性.  
堆能**高效, 快速的找出最大值和最小值, 时间复杂度为 O(1)**  
最大堆和最小堆是两种常见的堆数据结构, 它们都是完全二叉树的一种实现方式, 常用于解决一些优先级相关的问题.  
最大堆(Max Heap)是一种满足以下性质的二叉堆:

- 每个节点的值都大于或等于其子节点的值.
- 根节点的值是堆中最大值

最大堆和最小堆的特点使得它们适用于一些常见的应用场景, 例如:

- 堆排序(Heap Sort): 使用最大堆或最小堆实现的排序算法. [比较高频的考题]
- 优先级队列(Priority Queue): 用于维护具有优先级的元素集合, 并支持高效地插入和删除最大(或最小)元素的操作

在最大堆中, 每次插入操作都会将元素放置在堆的末尾, 然后通过一系列上浮操作将其移动到合适的为止.  
删除操作则会删除堆顶元素, 并将最后一个元素移动到堆顶, 然后通过一系列下沉操作将其移动到合适的位置.  
在最小堆中, 每次插入操作和删除操作的过程类似, 只是比较的方式相反.
每次插入都会将元素放置在堆的末尾, 然后通过一系列上浮操作将其移动到合适的位置.  
每次删除操作则会删除堆顶元素, 并将最后一个元素移动到堆顶, 然后通过一系列下沉操作将其移动到合适的位置.  
最大堆和最小堆的操作时间复杂度主要取决于堆的高度, 即 O(log N), 其中 N 是堆中元素的个数.  
因此, 它们在维护大量元素时具有高效地插入和删除操作.

## 堆的实现

用数组来储存堆  
因为堆是完全二叉树, 所以能确保每一个都有子节点  
那么在数组中, 找到子节点的方式就可以通过下标. 因为确保大家都有子节点, 所以子节点相对父节点下标的位置是确定的.

            1
           / \
          2   3
        /  \ /  \
       4   5 6   7
      / \
     8   9

[1, 2, 3, 4, 5, 6, 7, 8, 9]
0 1 2 3 4 5 6 7 8

1 (下标 0)的子节点是 2, 3, 下标开始于 1 = 2 \* 0 + 1  
2 (下标 1)的子节点是 4, 5, 下标开始于 3 = 2 \* 1 + 1  
3 (下标 2)的子节点是 6, 7, 下标开始于 5 = 2 \* 2 + 1  
4 (下标 3)的子节点是 8, 9, 下标开始于 7 = 2 \* 3 + 1

给定一个下标 i, 该位置的节点的子节点开始于下标 2i + 1, 换句话说, left child = 2i + 1, right child = 2i + 2

9 (下标 8)的父节点是 4, 下标是 3, floor((8-1)/2) = floor(7/2) = floor(3.5) = 3  
8 (下标 7)的父节点是 4, 下标是 3, floor((7-1)/2) = floor(6/2) = 3

### 需要的方法

    2

/ \
 3 4

1. push(value)  
   把给定的元素放到数组的尾部 比如原先是 [2,3,4], 插入 1, 变成[2,3,4,1], 但这样破坏了堆结构, 需要上浮这个新插入的元素, 用 bubbleUp(index)
2. bubbleUp(index)  
   帮当前节点于他的父节点作比较, 如果是 minHeap, 父节点需要比当前节点小. 所以判断父节点是否小于当前节点,
   - 如果父节点小, 那么结束上浮
   - 如果父节点大, 上浮当前节点, 也就是把父节点和当前节点互换 swap(), 然后在当前节点的新位置上再次调用 bubbleUp(index)
3. swap(i, j)  
   交换两个元素
4. pop()  
   取出最小的元素, 从数组中删除, 然后把最后一个元素提到根的位置, 然后 bubbleDown(0),
5. bublleDown(indez)  
   如果当前节点比子节点大, 与最小的子节点互换位置, 然后继续 bubbleDown(index)
6. peek()  
   查看最小值

**这是根据思路自己写, 然后用 ai 找问题继续自己改的版本**

```Javascript
class minHeap {
  // 这个只是为了测试方便, 构造函数直接接受一个数组
  // 这里就是假定这个数组是一个合法的堆
  // 如果没有这个前提, 并且想接受一个数组作为构造函数, 那么在构造函数
  // 找那个还需要验证是否是一个合法的堆, 如果不是把它调整成合法的堆
  constructor(values) {
    this.data = [...values]
  }

  first() {
    return this.data[0];
  }

  swap(i, j) {
    [this.data[i], this.data[j]] = [this.data[j], this.data[i]];
  }

  bubbleUp(index) {
    // 当前节点与父节点做比较, 如果父节点小, 停止上浮, 如果父节点大, 交换, 继续上浮
    const parent = Math.floor((index-1)/2);
    if (parent < 0) return;
    if (this.data[parent] < this.data[index]) {
      // 停止上浮
      return
    } else {
      this.swap(index, parent);
      // 交换了之后, 当前节点的 index 变成了父节点的 index
      this.bubbleUp(parent);
    }
  }

  push(value) {
    this.data.push(value);
    this.bubbleUp(this.data.length - 1);
  }

  pop() {
    if (this.data.length === 0) return null;
    const res = this.data[0];
    this.swap(0, this.data.length - 1);
    this.data.pop();
    this.bubbleDown(0);
    return res;
  }

  bubbleDown(index) {
    // 和左右子节点做比较, 如果当前节点更大, 下沉, 如果当前节点最小, 停止下沉
    const left = 2*index + 1;
    const right = 2*index + 2;
    if (left >= this.data.length) return; // 没有子节点
    if (right >= this.data.length) { // 只有左节点
      if (this.data[index] > this.data[left]) {
        this.swap(index, left);
        this.bubbleDown(left);
      }
      return;
    }
    // 左右节点都存在
    const smallChild = this.data[left] < this.data[right] ? left : right;
    if (this.data[index] > this.data[smallChild]) {
      this.swap(index, smallChild);
      this.bubbleDown(smallChild);
    }
  }

  peek() {
    return this.data[0];
  }
}

const heap = new minHeap([2,3,4,5,6])
heap
heap.push(1)
heap
heap.pop();
heap
heap.peek();
heap.pop();
heap
heap.pop();
heap
heap.pop();
heap
heap.pop();
heap
heap.pop();
heap
heap.pop();
heap
heap.pop();
heap
```

## 用 minHeap 解这道题

```Typescript
class MyMinHeap {
    private data: number[];
    constructor(data: number[] = []) {
        this.data = [...data];
    }

    swap(index1: number, index2: number) {
        [this.data[index1], this.data[index2]] = [this.data[index2], this.data[index1]];
    }

    getLeftChildIndex(index: number): number {
        return 2 * index + 1;
    }

    getRightChildIndex(index: number): number {
        return 2 * index + 2;
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
        if (this.data[index] < this.data[parentIndex]) {
            // swap 节点, 在父节点 index 上再次调用 bubbleUp
            this.swap(index, parentIndex);
            this.bubbleUp(parentIndex);
        }
    }

    bubbleDown(index: number) {
        // 三种情况, 当前节点 1. 没有子节点, 2. 只有左子节点, 3. 左右都有
        const heapSize = this.data.length;
        const leftChildIndex = this.getLeftChildIndex(index);
        const rightChildIndex = this.getRightChildIndex(index);
        // 1. 没有子节点
        if (leftChildIndex >= heapSize) {
            return;
        // 2. 只有左子节点, 比较, 如果子节点更小, 下沉, 否则停止下沉
        } else if (rightChildIndex >= heapSize) {
            if (this.data[leftChildIndex] < this.data[index]) {
                // swap, 在子节点 index 上再次调用 bubble down
                this.swap(index, leftChildIndex);
                // v 因为完全二叉树 v
                // 但是因为已经只有左节点了, 说明 heap 已经到底了, 直接 return 就可以, 不用再次调用了
                return;
            }
        // 3. 左右子节点都存在, 取更小的子节点与当前节点比较, 如果子节点更小, 下沉, 否则停止下沉
        } else {
            const smallChildIndex = this.data[leftChildIndex] < this.data[rightChildIndex] ? leftChildIndex : rightChildIndex;
            if (this.data[smallChildIndex] < this.data[index]) {
                this.swap(smallChildIndex, index);
                this.bubbleDown(smallChildIndex);
            }
        }
    }

    push(value: number) {
        this.data.push(value);
        this.bubbleUp(this.data.length - 1);
    }

    pop(): number | null {
        if (this.data.length === 0) return null;
        if (this.data.length === 1) {
            return this.data.pop();
        }
        this.swap(this.data.length - 1, 0);
        const res = this.data.pop();
        this.bubbleDown(0);
        return res;
    }

    peek(): number | null {
        return this.data.length > 0 ? this.data[0] : null;
    }

    size(): number {
        return this.data.length;
    }
}

function findKthLargest(nums: number[], k: number): number {
    const myMinHeap = new MyMinHeap();
    nums.forEach(n => {
        if (myMinHeap.size() < k) {
            myMinHeap.push(n);
        } else {
            if (myMinHeap.peek()! < n) {
                myMinHeap.push(n);
                myMinHeap.pop();
            }
        }

    })
    return myMinHeap.peek();
};
// 时间复杂度: 上浮下沉最多都走的是堆高k, n 个元素, O(logk)
// 空间复杂度: O(k)
```
