# 349. 两个数组的交集

https://leetcode.cn/problems/intersection-of-two-arrays/description/

方法一: 自己写的 哈希表 遍历

```Typescript
function intersection(nums1: number[], nums2: number[]): number[] {
    const set1: Set<number> = new Set(nums1);
    const resSet: Set<number> = new Set<number>();

    for (let num of nums2) {
        if (set1.has(num)) {
            resSet.add(num)
        }
    }
    return [...resSet];
}
// 时间复杂度: 创建 set1 O(n), n=nums1.length, 遍历 nums2 O(m), m=nums2.length. 总时间复杂度 O(m+n)
// 空间复杂度: O(m+n)
```

方法二: 力扣题解, 用时间换空间, 双指针

```Typescript
function intersection(nums1: number[], nums2: number[]): number[] {
    nums1.sort((x,y) => x - y);
    nums2.sort((x,y) => x - y);

    const len1 = nums1.length, len2 = nums2.length;
    const res: number[] = [];

    let index1 = 0, index2 = 0;
    while (index1 < len1 && index2 < len2) {
        if (nums1[index1] === nums2[index2]) {
            // 保证唯一性
            if (!res.length || nums1[index1] !== res[res.length-1]){
                res.push(nums1[index1]);
            }
            index1++;
            index2++;
        } else if (nums1[index1] < nums2[index2]) {
            index1++;
        } else {
            index2++;
        }
    }
    return res;
}
// 时间复杂度: 排序数组, O(mlogm) + O(nlogn), 双指针扫描, 最坏情况, 一次走一个, 全都走一步, O(m+n), 排序复杂度为主导项, 所以最终复杂度为 O(mlogm) + O(nlogn)
// 空间复杂度: 双指针式 O(1) 但是 sort() 需要 O(logm + logn) 的递归栈空间
```
