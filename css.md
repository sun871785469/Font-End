# Css-Cascading Style Sheet 层叠样式表

- [Css基本概念](#css基本概念)

# Css基本概念

### 流

**流又叫文档流，是css的基本定位和布局机制。** 流是html的抽象概念，暗喻这种排列布局方式像水流一样自然自动。“流体布局”是html默认的布局机制，如果html不加css的话，布局是自上而下(块级元素比如`div`)，从左到右(内联元素比如`span`)堆砌的布局方式。
- 文档一旦脱离文档流，计算其父元素的高度时就不再计算其自身的高度
- 脱离文档流的方法
1.`float` 2.`position:absolut` 3.`position:fixed`

### 块级元素和行内元素

块级元素是指单独撑满一行的元素，如：`div、ul、li、p`。
行内元素又叫内联元素，指只占据它对应标签的边框所包含的空间，如：`span、a、em`。如果这些元素的父标签宽度足够大，则这些元素会排列在一行展示。

实际开发中我们把`display`计算值为`inline、inline-block、inline-table、table-cell`的元素叫做行内元素，而把计算值为`block`的元素叫做块级元素