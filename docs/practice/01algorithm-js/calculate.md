---
sidebar_position: 2
---

# 运算篇

## 整数除法

**🔆 注意**：如果结果为正数，使用`Math.floor()` ；如果结果为负数，则需要使用`Math.ceil()`

```javascript
let c = a / b;
c = c > 0 ? Math.floor(c) : Math.ceil(c);
```
