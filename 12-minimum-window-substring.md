# 76. 最小覆盖子串

https://leetcode.cn/problems/minimum-window-substring/

自己最开始写的思路, 但是问题很多

```Typescript
function minWindow(s: string, t: string): string {
    // 最先想到的是不重复小子串问题中的 哈希表存下标的滑动窗口
    // 但是这里感觉应该存的是出现次数, 因为不能少于
    // 而 l 可以不跳跃, 通过像 set 解法中的 ++ 来移动
    if (t.length > s.length) return "";

    const tMap = new Map<string, number>();
    for (const ch of t) {
        if (tMap.has(ch)) {
            tMap.set(ch, tMap.get(ch) + 1);
        } else {
            tMap.set(ch, 1);
        }
    }

    const sMap = new Map<string, number>();
    let l = 0, r = 0;
    let ans = "";
    let action = "r"
    while (action) {
        if (action === "r") {
            r++;
            tMap.set(s[r], (tMap.get(s[r]) ?? 0) + 1);
        }
        if (action === "l") {
            l++;
            tMap.set(s[l], (tMap.get(s[l]) ?? 0) + 1);
        }
        let valid = true;
        for (const ch of tMap.keys()) {
            if (sMap.get(ch) < tMap.get(ch)) {
                valid = false;
                break;
            }
        }
        if (valid) {
            const substring = s.slice(l, r+1);
            ans = substring.length < ans.length ? substring : ans;
            // 在满足条件的情况下, 如果是当前操作是移动右指针, 切换到左指针
            if (action === "r") action = "l"
            // 如果当前操作是移动左指针, 继续移动直到不满足
            if (action === "l") action = "l"
        } else {
            // 如果当前是移动左指针, 切换到移动右指针
            if (action === "l") action = "r"

            // 不满足条件的情况下, 然后判断右指针是否到底
            if (r >= s.length) {
                action = null;
            }
        }
    }
    return ans;
    // 先一直移动右指针, 直到满足 t 的要求, 记录最优答案
    // 移动左指针, 检查是否仍然满足要求, 如果满足, 记录答案比较是否最优, 如果不满足, 下一次开始移动右指针
};
```

修改后, 可运行的版本

```Typescript
function minWindow(s: string, t: string): string {
    // 最先想到的是不重复小子串问题中的 哈希表存下标的滑动窗口
    // 但是这里感觉应该存的是出现次数, 因为不能少于
    // 而 l 可以不跳跃, 通过像 set 解法中的 ++ 来移动
    if (t.length > s.length) return "";

    // 统计 t 中每个字符需要的次数
    const tMap = new Map<string, number>();
    for (const ch of t) {
        tMap.set(ch, (tMap.get(ch) ?? 0) + 1);
    }

    // sMap 用来记录当前窗口 [l, r) 内每个字符出现次数
    const sMap = new Map<string, number>();

    let l = 0, r = 0;
    let ans = "";
    let action: "l" | "r" | null = "r";

    while (action) {
        // 移动右指针
        if (action === "r") {
            if (r >= s.length) {
                // 右指针到底了, 只能靠左指针缩小窗口
                action = "l";
            } else {
                sMap.set(s[r], (sMap.get(s[r]) ?? 0) + 1);
                r++;
            }
        }
        // 移动左指针
        if (action === "l") {
            sMap.set(s[l], (sMap.get(s[l]) ?? 0) - 1);
            l++;
        }
        // 判断当前窗口 是否满足要求
        let valid = true;
        for (const ch of tMap.keys()) {
            if ((sMap.get(ch) ?? 0) < tMap.get(ch)) {
                valid = false;
                break;
            }
        }
        // 如果满足尝试缩小窗口
        if (valid) {
            const substring = s.slice(l, r);
            if (ans === "" || substring.length < ans.length) {
                ans = substring;
            }
            // 满足时, 开始缩小左边
            action = "l";
        } else {
            // 不满足, 扩大右边
            action = "r"
            // 不满足条件的情况下, 然后判断右指针是否到底
            if (r >= s.length) {
                action = null;
            }
        }
    }
    return ans;
    // 先一直移动右指针, 直到满足 t 的要求, 记录最优答案
    // 移动左指针, 检查是否仍然满足要求, 如果满足, 记录答案比较是否最优, 如果不满足, 下一次开始移动右指针
};
```

看了 leetcode 题解并且拷打 AI 后, 发现我的写法思路已经符合最优了, 但是比如判断是否满足, 还可以优化, 如下

```Typescript
function minWindow(s: string, t: string): string {
    if (t.length > s.length) return "";

    // 统计 t 中每个字符需要的次数
    const tMap = new Map<string, number>();
    for (const ch of t) {
        tMap.set(ch, (tMap.get(ch) ?? 0) + 1);
    }

    // sMap 用来记录当前窗口 [l, r) 内每个字符出现次数
    const sMap = new Map<string, number>();
    for (const ch of tMap.keys()) sMap.set(ch, 0);

    const needCount = tMap.size;
    let haveCount = 0;

    let l = 0, r = 0;
    let ans = "";
    let action: "l" | "r" | null = "r";

    while (action) {
        // 移动右指针
        if (action === "r") {
            if (r >= s.length) {
                // 右指针到底了, 只能靠左指针缩小窗口
                action = "l";
            } else {
                const ch = s[r]
                if (sMap.has(ch)) {
                    const before = sMap.get(ch)!;
                    const after = before + 1;
                    if (after === tMap.get(ch)) haveCount++;
                    sMap.set(ch, after);
                }
                r++;
            }
        }
        // 移动左指针
        if (action === "l") {
            const ch = s[l];
            if (sMap.has(ch)) {
                const before = sMap.get(ch)!;
                const after = before - 1;
                if (before === tMap.get(ch)) haveCount--;
                sMap.set(ch, after);
            }
            l++;
        }
        // 判断是否满足
        let valid = haveCount === needCount;

        // 如果满足尝试缩小窗口
        if (valid) {
            const substring = s.slice(l, r);
            if (ans === "" || substring.length < ans.length) {
                ans = substring;
            }
            // 满足时, 开始缩小左边
            action = "l";
        } else {
            // 不满足, 扩大右边
            action = "r"
            // 不满足条件的情况下, 然后判断右指针是否到底
            if (r >= s.length) {
                action = null;
            }
        }
    }
    return ans;
    // 先一直移动右指针, 直到满足 t 的要求, 记录最优答案
    // 移动左指针, 检查是否仍然满足要求, 如果满足, 记录答案比较是否最优, 如果不满足, 下一次开始移动右指针
};

// 时间复杂度 O(n), 根据状态的变化, 左右指针各遍历一遍字符串, 但是绝对小于 2n
// 空间复杂度 O(t), 字典存放 最多为 t 的长度的字符以及对应出现的数量, 两个这样的字典
```

佳哥的做法是, 用 JS 对象来代替 map

```typescript
while (r < s.length) {
  // 这里有 count 来判断是否满足条件
  while (count === 0) {
    // 移动左指针缩小窗口
  }
}
```
