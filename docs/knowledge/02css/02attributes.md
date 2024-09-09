---
sidebar-position: 2
---

# 属性

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

## 元素居中

### 行内元素居中

水平居中：

### 块级元素居中

| 居中方式 | 行内元素                                                                       | 块级元素                                                                                                               |
| -------- | ------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| 水平居中 | text-align: center                                                             | margin: 0 auto 或者 position:absolute; top: 50%; left: 50%, transform: translate(-50%, -50%)                           |
|          |                                                                                | 容器设置： display: flex; justify-content: center 或者 display: table-cell; text-align: center; vertical-align: center |
| 垂直居中 | line-height: [盒子高度] 或多行文本：display:tabel-cell; vertical-align: center |                                                                                                                        |

## 文本溢出

### 单行溢出

text-overflow: ellipsis 显示省略号代表被裁剪的文本
white-space： nowrap 设置文字在一行显示，不能换行
over-flow: hidden

## flex: 1

等分剩余空间

flex: 1 相当于`flex-grow: 1; flex-shrink: 1; flex-basis: 0%;`

- flex-basis: flex 元素在主轴上的初始大小，决定了 contet-box 的尺寸，默认值`auto`(含义是 "参照我的 width 和 height 属性"),
- flex-grow：主尺寸的 flex 增长系数，默认值 0。规定了 flex-grow 项在 flex 容器中分配剩余空间的相对比例。剩余空间是 flex 容器的大小减去所有 flex 项的大小加起来的大小。如果所有的兄弟项目都有相同的 flex-grow 系数，那么所有的项目将剩余空间按相同比例分配，否则将根据不同的 flex-grow 定义的比例进行分配。
