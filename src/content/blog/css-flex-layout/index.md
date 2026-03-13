---
title: CSS Flex 布局
publishDate: 2025-11-24 20:00:00
description: 'CSS 中最强大和最常用的布局方式之一'
tags:
  - CSS
  - Flex
heroImage: { src: './thumbnail.webp' }
language: 'zh-cn'
---

<style>
.item {
  font-size: 30px;
  color: white;
  background-color: cornflowerblue;
  width: 80px;
  height: 80px;
  border: 2px solid cyan;
}

.item.growable {
  flex-grow: 1;
}

.item.unshrinkable {
  flex-shrink: 0;
}

.container {
  border: 2px solid red;
  margin: 20px auto 20px;
  width: 95%;
  max-width: 95%;
  opacity: 80%;
}

.container-higher {
  height: 320px;
}

.padding-bottom {
  padding-bottom: 40px;
}

.container-wrap {
  flex-wrap: wrap;
}

.gap20px {
  gap: 20px;
}

.gap40px {
  gap: 40px;
}

.gap80px {
  gap: 80px;
}

.container-height-auto {
  height: auto;
}

.flex {
  display: flex;
}

:root {
  --shrink-from: 400px;
  --shrink-to:   220px;
  --grow-from: 300px;
  --grow-to:   420px;
}

@media (max-width: 400px) {
  :root {
    --shrink-from: 95%;
    --shrink-to:   55%;
  }
}

@media (max-width: 420px) {
  :root {
    --grow-to: 95%;
  }
}

@keyframes breathing-shrink {
  from { width: var(--shrink-from); }
  to   { width: var(--shrink-to); }
}

@keyframes breathing-grow {
  from { width: var(--grow-from); }
  to   { width: var(--grow-to); }
}

.container-breathing-shrink {
  animation: breathing-shrink 1s infinite alternate ease-in-out;
}

.container-breathing-grow {
  animation: breathing-grow 1s infinite alternate ease-in-out;
}

.arrow-wrapper {
  width: 90%;
  margin: 24px auto 0;
  display: flex;
  align-items: center;
  gap: 12px;
}

.label {
  color: #f7d000;
  font-size: 20px;
  font-weight: bold;
  white-space: nowrap;
  flex-shrink: 0;
}

.arrow-line {
  flex: 1;
  height: 2px;
  background: #f7d000;
  position: relative;
}

.arrow-line::after {
  content: "";
  position: absolute;
  right: -8px;
  top: 50%;
  transform: translateY(-50%);
  width: 0;
  height: 0;
  border-top: 6px solid transparent;
  border-bottom: 6px solid transparent;
  border-left: 8px solid #f7d000;
}
</style>

## flex 布局

### 块级元素和行内元素

块级元素：独占一行（前后会自动换行），自动填满父容器的宽度。它们的 `display` 属性为 `block`。
行内元素：与其他行内元素排在一行内，宽高由内容决定。它们的 `display` 属性为 `inline`。

大部分 html 元素都是块级元素或行内元素。

下面是一个例子，一个 container 中装了 3 个 item。

```html
<div class="container">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>
```

这里的 item 都是块级元素，它们都独占一行，自上而下排列。

为了方便展示，示例中的 container 容器使用 <span style="color:red;">红色边框</span>，其中的 item 项目使用 <span style="color:cornflowerblue;">蓝色方块</span>。

<div class="container padding-bottom">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>

### flex 容器和 flex 元素

在容器元素的 CSS 中添加 `display: flex`，它就会变为 flex 容器。

flex 容器的直接子项目都是 flex 元素。

```css
.container {
  display: flex;
}
```

可以看到，flex 容器中的 item 项目会自动排列在一行内。

<div class="container flex padding-bottom">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>

### flex 容器的主轴

flex 布局的**主轴**默认为水平方向，主轴方向由 `flex-direction` 属性控制，默认为 `row`。

```css
.container {
  display: flex;
  flex-direction: row;
}
```

<div class="container">
  <div class="flex" style="justify-content:start;">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
  </div>
  <div class="arrow-wrapper">
    <span class="label start">Start</span>
    <div class="arrow-line"></div>
    <span class="label end">End</span>
  </div>
</div>

如果将主轴方向 `flex-direction` 设为 `row-reverse`（翻转），那么元素则会反方向排列。

```css
.container {
  display: flex;
  flex-direction: row-reverse;
}
```

<div class="container flex padding-bottom" style="flex-direction:row-reverse;">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>

## 沿轴线方向对齐

### 沿主轴对齐

flex 容器内的元素会沿着主轴方向排列，在 flex 容器上使用 `justify-content` 属性可以控制容器内元素在**主轴**上的位置。`justify-content` 有以下几个常用的取值。

1. `start`：对齐主轴起点，这是默认行为。

```css
.container {
  display: flex;
  justify-content: start;
}
```

<div class="container">
  <div class="flex" style="justify-content:start;">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
  </div>
  <div class="arrow-wrapper">
    <span class="label start">Start</span>
    <div class="arrow-line"></div>
    <span class="label end">End</span>
  </div>
</div>

1. `end`：对齐主轴终点。

```css
.container {
  display: flex;
  justify-content: end;
}
```

<div class="container">
  <div class="flex" style="justify-content:end;">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
  </div>
  <div class="arrow-wrapper">
    <span class="label start">Start</span>
    <div class="arrow-line"></div>
    <span class="label end">End</span>
  </div>
</div>

3. `center`：在主轴上居中。

```css
.container {
  display: flex;
  justify-content: center;
}
```

<div class="container">
  <div class="flex" style="justify-content:center;">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
  </div>
  <div class="arrow-wrapper">
    <span class="label start">Start</span>
    <div class="arrow-line"></div>
    <span class="label end">End</span>
  </div>
</div>


4. `space-between`：均匀分配每个 item 的间隙（item 之间）。

```css
.container {
  display: flex;
  justify-content: space-between;
}
```

两侧不留间隙，只在 item 之间均匀分配。

<div class="container flex padding-bottom" style="justify-content:space-between;">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>

5. `space-around`：均匀分配每个 item 两侧的空间（周围）。

```css
.container {
  display: flex;
  justify-content: space-around;
}
```

item 之间的间隙是侧边的两倍。

<div class="container flex padding-bottom" style="justify-content:space-around;">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>


6. `space-evenly`：均匀分配每个 item 的间隙（包括侧边的）。

```css
.container {
  display: flex;
  justify-content: space-evenly;
}
```

两侧留出间隙，所有间隙都均匀分配。

<div class="container flex padding-bottom" style="justify-content:space-evenly;">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>

7. `gap`：手动设置间隙

手动指定 flex 元素之间的间隙。

```css
.container {
  display: flex;
  gap: 20px;
}
```

<div class="container flex padding-bottom" style="gap:20px;">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>

### 沿交叉轴对齐

交叉轴垂直于主轴，使用 `align-items` 属性可以调整元素在交叉轴上的对齐方式。

为了方便展示纵向对齐，这里增加了 <span style="color:red;">容器边框</span> 的高度。

1. 对齐交叉轴起点

`align-items` 的默认值也是 `start`。

```css
.container {
  display: flex;
  align-items: start;
}
```

<div class="container flex container-higher">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>

2. 对齐交叉轴终点

```css
.container {
  display: flex;
  align-items: end;
}
```

<div class="container flex container-higher" style="align-items:end;">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>

3. 居中对齐

```css
.container {
  display: flex;
  align-items: center;
}
```

<div class="container flex container-higher" style="align-items:center;">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>

同时使用 `justify-content: center` 和 `align-items: center`，可以轻松让元素水平垂直居中。

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

<div
  class="container flex container-higher"
  style="justify-content:center;align-items:center;"
>
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>

### 旋转主轴方向

设置 `flex-direction` 为 `column`，可以将主轴方向设为纵向，交叉轴方向同时变为横向。

```css
.container {
  display: flex;
  flex-direction: column;
}
```

<div
  class="container flex container-higher"
  style="flex-direction:column;"
>
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>

由于现在的交叉轴是横向的，所以此时要让元素水平居中，应该调整 `align-content` 而不是 `justify-content`。

```css
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
```

<div
  class="container flex container-higher"
  style="flex-direction:column;align-items:center"
>
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>

而 `justify-content` 现在调整的是纵向的主轴，即垂直方向的对齐方式。

```css
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
}
```

<div
  class="container flex container-higher"
  style="flex-direction:column;justify-content:center"
>
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>

## 多行布局

### flex 自动换行

给 flex 容器使用 `flex-wrap` 属性，可以控制 flex 元素是否自动换行。

`flex-wrap` 的默认值为 `nowrap`，当容器空间不足（比如手机屏幕，横向空间较小）时，会压扁（收缩）其中的元素。如果元素无法被压缩，会直接溢出容器。

```css
.container {
  display: flex;
  gap: 40px;
  flex-wrap: nowrap;
}
```

如果你正在使用电脑浏览这个页面，你可以试试拖动浏览器窗口右侧，改变页面宽度，观察下面的元素会发生什么。

<div class="container flex padding-bottom" style="gap:40px">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
  <div class="item">4</div>
  <div class="item">5</div>
  <div class="item">6</div>
  <div class="item">7</div>
  <div class="item">8</div>
  <div class="item">9</div>
</div>

将 `flex-wrap` 设为 `wrap`，元素会自动在空间不足时换行。

```css
.container {
  display: flex;
  flex-wrap: wrap;
  gap: 40px;
  max-width: 400px; /* 限制宽度方便展示换行 */
}
```

<div
  class="container flex container-wrap gap40px padding-bottom"
  style="max-width:400px;"
>
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
  <div class="item">4</div>
  <div class="item">5</div>
  <div class="item">6</div>
  <div class="item">7</div>
  <div class="item">8</div>
  <div class="item">9</div>
</div>

### 多行布局下的对齐方式

就像主轴上有 `justify-content` 属性一样，交叉轴上也有 `align-content`属性，它可以控制交叉轴上整体的对齐方式。

现在有以下示例，在一个 flex 容器中装了 9 个元素，让它们自动换行，现在不使用 `gap`。

```css
.container {
  display: flex;
  flex-wrap: wrap;
  max-width: 320px;
  height:450px;
}
```

<div
  class="container flex container-wrap"
  style="max-width:320px;height:450px;"
>
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
  <div class="item">4</div>
  <div class="item">5</div>
  <div class="item">6</div>
  <div class="item">7</div>
  <div class="item">8</div>
  <div class="item">9</div>
</div>

`align-content` 默认为 `stretch`，所以可以发现即使不使用 `gap`，元素在垂直方向上依然有间隙。

`align-content` 同样也有 `start`、`end` 和 `center` 取值，让元素整体在交叉轴上对齐。比如，下面使用 `align-content: center` 让元素在交叉轴上居中对齐。

```css
.container {
  /* ... */
  align-content: center;
}
```

<div
  class="container flex container-wrap"
  style="max-width:320px;height:450px;align-content:center"
>
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
  <div class="item">4</div>
  <div class="item">5</div>
  <div class="item">6</div>
  <div class="item">7</div>
  <div class="item">8</div>
  <div class="item">9</div>
</div>

前面说过，`align-items` 也是用来调整交叉轴上的对齐方式的，那么它和 `align-content` 的区别是什么呢？

尝试用 `align-items` 替代 `align-content`：

```css
.container {
  /* ... */
  align-items: center;
}
```

<div
  class="container flex container-wrap"
  style="max-width:320px;height:450px;align-items:center"
>
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
  <div class="item">4</div>
  <div class="item">5</div>
  <div class="item">6</div>
  <div class="item">7</div>
  <div class="item">8</div>
  <div class="item">9</div>
</div>

```css
.container {
  /* ... */
  align-items: end;
}
```

<div
  class="container flex container-wrap"
  style="max-width:320px;height:450px;align-items:end"
>
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
  <div class="item">4</div>
  <div class="item">5</div>
  <div class="item">6</div>
  <div class="item">7</div>
  <div class="item">8</div>
  <div class="item">9</div>
</div>

```css
.container {
  /* ... */
  align-items: start;
}
```

<div
  class="container flex container-wrap"
  style="max-width:320px;height:450px;align-items:start"
>
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
  <div class="item">4</div>
  <div class="item">5</div>
  <div class="item">6</div>
  <div class="item">7</div>
  <div class="item">8</div>
  <div class="item">9</div>
</div>

结合上面三个例子不难看出，`align-items` 控制的 `start`、`end` 和 `center` 让元素对齐了它们各自的交叉轴。

其实在 `wrap` 自动换行后，每一行（如果设置了纵向主轴则是列）元素都有它们自己独立的主轴和交叉轴。此时 `align-items` 调整的是各行元素在自身的交叉轴上的对齐方式，而 `align-content` 调整的是多行元素整体在交叉轴上的对齐方式。

所以，如果要将元素完全居中对齐，我们应该在 flex 容器上设置以下三个 `center` 值：

```css
.container {
  /* ... */
  justify-content: center;
  align-items: center;
  align-content: center;

  /* 这里适当添加 gap，看起来更美观 */
  gap: 10px;
}
```

<div
  class="container flex container-wrap"
  style="max-width:320px;
    height:450px;
    justify-content:center;
    align-items:center;
    align-content:center;
    gap:12px;"
>
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
  <div class="item">4</div>
  <div class="item">5</div>
  <div class="item">6</div>
  <div class="item">7</div>
  <div class="item">8</div>
  <div class="item">9</div>
</div>

## 拉伸和收缩元素

### 自动拉伸

如果想要让元素自动拉伸以填满容器的剩余空间，可以使用 `flex-grow` 属性。

`flex-grow` 的默认值为 `0`，即禁用自动拉伸。

```css
.item {
  flex-grow: 0;
}
```

<div class="container flex padding-bottom container-breathing-grow gap20px">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>

当 `flex-grow` 设为 `1` 时，元素将会自动拉伸以填满容器空间。注意 `flex-grow` 属性是加在 item 元素身上的，而不是 flex 容器上。

```css
.item {
  flex-grow: 1;
}
```

<div class="container flex padding-bottom container-breathing-grow gap20px">
  <div class="item growable">1</div>
  <div class="item growable">2</div>
  <div class="item growable">3</div>
</div>

也可以指定拉伸其中某个元素，其他不拉伸，比如只在第二个元素上添加 `flex-grow: 1`。

```css
.item {
  flex-grow: 0;
}

#item-2 {
  flex-grow: 1;
}
```

<div class="container flex padding-bottom container-breathing-grow gap20px">
  <div class="item">1</div>
  <div class="item growable">2</div>
  <div class="item">3</div>
</div>

### 自动收缩

flex 容器中的元素在空间不足时会自动收缩，自动收缩由 `flex-shrink` 属性控制，其默认值为 `1`，即：

```css
.item {
  flex-shrink: 1;
}
```

注意 `flex-shrink` 属性是加在元素身上的，而不是 flex 容器上。

<div class="container flex padding-bottom container-breathing-shrink gap20px">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>

如果要禁用自动压缩，可以设置 `flex-shrink: 0`。

```css
.item {
  flex-shrink: 0;
}
```

<div class="container flex padding-bottom container-breathing-shrink gap20px">
  <div class="item unshrinkable">1</div>
  <div class="item unshrinkable">2</div>
  <div class="item unshrinkable">3</div>
</div>

### 按比例拉伸或收缩

在同时存在多个 item 时，`flex-grow` 和 `flex-shrink` 的数值是有意义的，通过给它们设置不同的值，可以实现按比例拉伸或收缩。

```html
<div class="container">
  <div class="item" style="flex-grow:1;">1</div>
  <div class="item" style="flex-grow:3;">2</div>
  <div class="item" style="flex-grow:5;">3</div>
</div>
```

这样每个元素被拉伸的空间比例为 1:3:5。注意：这并不是说元素占用的空间是 1:3:5，而是它们被拉伸的空间是按这个比例分配的。

<div class="container flex padding-bottom gap20px" style="width:90%;">
  <div class="item" style="flex-grow:1;">1</div>
  <div class="item" style="flex-grow:3;">2</div>
  <div class="item" style="flex-grow:5;">3</div>
</div>

按比例收缩同理，数值越大被压缩的越多。

```html
<div class="container">
  <div class="item" style="flex-shrink:1;">1</div>
  <div class="item" style="flex-shrink:3;">2</div>
  <div class="item" style="flex-shrink:5;">3</div>
</div>
```

<div class="container flex padding-bottom gap20px" style="width:220px;">
  <div class="item" style="flex-shrink:1;">1</div>
  <div class="item" style="flex-shrink:3;">2</div>
  <div class="item" style="flex-shrink:5;">3</div>
</div>