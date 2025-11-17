# 3. 无重复字符的最长子串

https://leetcode.cn/problems/longest-substring-without-repeating-characters/description/

方法一: 看了题解思路之后的滑动窗口, 用的 map

```Typescript
function lengthOfLongestSubstring(s: string): number {
    // 用散列表 保存字符最后一次出现的位置
    // 用两个 index 记录当前窗口
    // 记录最大长度
    const strHash = new Map<string, number>();
    let l = 0, r = 0;
    let ans = 0;

    while (r < s.length) {
        const currentStr = s[r];
        if (!strHash.has(currentStr)) {
            // 如果 map 中没有该字符, l 不用变, 因为不可能重复
            // 更新 map
            strHash.set(currentStr, r)
        } else {
            // 如果 map 中有该字符
            // 判断这个字符最后一次出现的位置, 如果在窗口内
            // 更新左坐标为最后一次出现+1
            if (strHash.get(currentStr) >= l) {
                l = strHash.get(currentStr) + 1;
            }
            // 更新 map
            strHash.set(currentStr, r);
        }
        ans = ans < r - l + 1 ? r - l + 1: ans;
        r++;
    }
    return ans;
};
// 时间复杂度: 遍历整个 string, 内部都是 O(1) 操作, O(n), n 为 string 长度
// 空间复杂度: map 储存所有出现过的字符, O(m), m 为出现过的字符的个数
```

方法二: 看了佳哥题解之后的, set 双指针

```Typescript
function lengthOfLongestSubstring(s: string): number {
    // set 版本
    const strSet = new Set<string>();
    let l = 0, r = 0;
    let ans = 0;

    while (r < s.length) {
        const curStr = s[r]
        if (!strSet.has(curStr)) {
            // 如果 set 中没有, 说明没见过
            strSet.add(curStr); // 加到 set 中
            r++; // 移动右指针
            ans = ans > r - l ? ans : r - l;  // 这里不用 +1, 因为指针已经移动了
        } else {
            // 如果 set 中有, 说明见过
            strSet.delete(s[l])  // 删除左指针现在的值
            l++; // 移动左指针
        }
    }
    return ans;
};
// 时间复杂度: O(n)
// 空间复杂度: O(m), 字符集大小
```

方法三: 暴力破解

```Typescript
function lengthOfLongestSubstring(s: string): number {
    let ans = 0;
    for (let i = 0; i < s.length; i++) {
        for (let j = i; j < s.length; j++) {
            const str = s.slice(i, j+1);
            const strLen = str.length;
            const strSet = new Set(str);
            const setLen = strSet.size;
            if (strLen === setLen) {
                ans = ans > strLen ? ans : strLen;
            }
        }
    }
    return ans;
};
// 时间复杂度 O(n^3)
// 空间复杂度 O(1)
```
