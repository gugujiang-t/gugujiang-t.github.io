---
sidebar-position: 1
---

# 选择器

## 优先级

内联样式 > id 选择器 > 类/伪类/属性选择器 > 标签/伪元素选择器选择器 > 关系选择器 > 通配符

## 盒模型

控制的属性：`box-sizing` ，默认值：`content-box`，怪异盒模型对应的值：`border-box`

## margin 重叠

垂直方向上的外边距会重叠哦

## BFC 块级格式化上下文

一块块级区域，内部的元素渲染不会影响到 BFC 外面的元素。

一个 BFC 的功能：

- 避免 margin 的垂直外边距重叠
- 解决浮动导致的高度塌陷
  - 方法：不浮动的元素用 BFC 包起来
  - 应用：双栏布局

形成 BFC 的方法：

1. overflow: hidden(不是 visible 就行)
2. position: absolute/fixed
3. display: flex / inline-block /table-cell

## clearfix

clearfix 的意思：如果一个元素里只有浮动元素，那它的高度会是 0（高度塌陷）。如果你想要它自适应地包含所有浮动元素，那就需要清楚子元素的浮动。

1. 伪元素

```css
#clearfix::after {
    content:'',
    display: table,
    clear: both
}
```

**clear 属性**:

- left: 元素被向下移动以清楚左浮动
- right: 元素被向下移动以清除右浮动。
- both: 元素被向下移动以清除两边浮动。

2. display 的 flow-root 属性

```css
#container {
  display: flow-root;
}
```
