20. 有效的括号

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。左括号必须以正确的顺序闭合。每个右括号都有一个对应的相同类型的左括号。

```typescript
function isValid(s: string): boolean {
  let helpStack: string[] = [];
  const map: { [key: string]: string } = { '(': ')', '{': '}', '[': ']' };

  for (let i = 1; s.length - i >= 0; i++) {
    const current = s[s.length - i];
    // 辅助栈为空, 推入辅助栈;
    if (!helpStack.length) {
      helpStack.push(current);
      continue;
    }
    if (map[current] === helpStack[helpStack.length - 1]) {
      helpStack.pop();
    } else {
      helpStack.push(current);
    }
  }
  return !helpStack.length;
}
```
