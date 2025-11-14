# 1.两数之和

https://leetcode.cn/problems/two-sum/

方法一: 暴力求解, 两次遍历

```Typescript
function twoSum(nums: number[], target: number): number[] {
    // 遍历
    for (let i = 0; i < nums.length; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            if (nums[i] + nums[j] === target) {
                return [i, j];
            }
        }
    }
    return  [];
};
// 时间复杂度 O(n^2)
// 空间复杂度 O(1)
```

方法二: 哈希表, 存数+index, diff 在表里找

```Typescript
function twoSum(nums: number[], target: number): number[] {
    const map = new Map<number, number>();

    for(const [i, num] of nums.entries()) {
        const diff = target - num;
        if (map.has(diff)) {
            return [map.get(diff), i]
        }
        map.set(num, i);
    }
    return [];
};
// 时间复杂度, 遍历一遍数组 O(n)
// 空间复杂度, 存整个数组 O(n)
```
