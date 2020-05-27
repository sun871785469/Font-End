# Css-Cascading Style Sheet 层叠样式表

- [Css基本概念](#css基本概念)
- [Css权重](#css权重)
- [盒模型](#盒模型)
- [BFC](#BFC)
- [Float浮动](#Float浮动)
- [清除浮动](#清除浮动)
- [层叠上下文](#层叠上下文)
- [雪碧图](#雪碧图)
- [如何解决不同浏览器的样式兼容性问题？](#如何解决不同浏览器的样式兼容性问题？)
- [css优化、提升性能的方式有哪些](#css优化、提升性能的方式有哪些)
- [css加载方式有几种](#css加载方式有几种)
- [ @import 有什么作用？如何使用？](# @import 有什么作用？如何使用？)

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

# Css权重

先来道面试题看一下
````
// 假设下面样式都作用于同一个节点元素`span`，判断下面哪个样式会生效
body#god div.dad span.son {width: 200px;}
body#god span#test {width: 250px;}`
````
css权重列表如下

|权重值| 选择器|
| ------------------------------- | ----------------------------------------------------------- |
| 1,0,0,0| 内联样式:styele=""| 
| 0,1,0,0| id选择器#idName{} |
| 0,0,1,0| 类、伪类、属性选择器 .className{}/:hover{}/[type="text"]{}|
| 0,0,0,1| 标签、伪元素 div{}/::after{}|
| 0,0,0,0| 通用选择器( * )、子选择器(>)、相邻选择器(+)、同胞选择器(~)|

不能想当然的认为10个类选择器就等于1个id选择器，css权重进制在ie6中为256，后来扩大到了65536，而现代浏览器则更大，所以可以认为**低等级的选择器数量再多也不会大于高等级的选择器权重。**
所以上面那道题再第二等级比较时就结束了，id选择器大于class选择器，**假如上下两行样式一样的话，下面的样式覆盖上面的样式**，但是无论什么样式，**遇到`!important`都是取`!important`的属性值**

# 盒模型

元素的内在盒子是由`margin box、boder box、padding box、content box`组成，这四个盒子由外到内构成了盒模型

IE盒模型：`box-sizing：border-box`,元素宽高为`padding+border+content`宽高的总和

w3c标准盒模型：`box-sizing:content-box`，宽高为`content`宽高

# BFC

### BFC (Block Formatting Context) 块级格式化上下文，是一个独立的区域。
- BFC本身不会发生`margin`重叠
- BFC可以彻底解决子元素浮动带来的的高度坍塌和文字环绕问题。
- 计算宽高度时连浮动元素也计算在内

### BFC创建的方法
- 根元素，即html
- float不为none
- 绝对定位元素(`absolute/fixed`)
- `display`为`flex/inline-block/table-caption/table-cell/grid/flow-root`
- `overflow`不为`visible` `(hidden/scroll/auto)`

# Float浮动

`Float` 其实本身是为了文本环绕图片而设计的，浮动元素从网页的文档流脱离而出，但是却并没有脱离出文本流，位置尽量靠上，靠左或者靠右，会影响其他元素的定位
### 对自己的影响

- 形成`BFC`

### 对兄弟元素的影响

- 上面一般贴非 float 元素
- 靠边贴 float 元素或边
- 不影响其他块级元素位置
- 影响其他块级元素文本
### 对父级元素的影响

- 从布局上消失
- 高度塌陷

# 清除浮动

- 使父级成为BFC
- 伪元素清除浮动
````
.container::after {
  content: " ";
  clear: both;
  display: block;
  visibility: hidden;
  height: 0;
}
````
# 层叠上下文

CSS 中的`z-index`属性控制重叠元素的垂直叠加顺序。`z-index`只能影响`position`值不是`static`的元素。

从上到下依次为：

- 正`z-index`
- `z-index:auto/z-index:0`
- 行内元素
- 浮动元素
- 块级元素
- 负`z-index`
- 最底层为 `border/background`

### CSS3新增层叠上下文

- 元素的`opacity`值不为1，也就是透明元素；
- 元素的`transform`值不为`none`；
- 元素的`filter`值不为`none`；
- 元素的设置`-webkit-overflow-scrolling: touch`；
- `z-index`不为`auto`的弹性盒子的子元素；
- 元素的`isolation`值为`isolate`；
- 元素的`mix-blend-mode`值不为`normal`；
- 元素的`will-change`值为`opacity/transform/filter/isolation/mix-blend-mode`中的一个。

这些属性都会默认`z-index:auto`

# 雪碧图

雪碧图也叫CSS精灵， 是一种CSS图像合成技术；

通俗来说：将小图标合并在一起之后的图片称作雪碧图，每个小图标的使用需要配合background-position来获取。

优点：
- 将多张图片合并到一张图片中，可以减小图片的总大小。
- 将多张图片合并成一张图片后，下载全部所需的资源，只需一次请求。可以减小建立连接的消耗。

生成工具 https://github.com/iwangx/sprite

# 如何解决不同浏览器的样式兼容性问题？

- 重置浏览器样式，使浏览器样式保持一致
- 使用`webpack`插件`autoprefixer`自动生成浏览器前缀
- 在确定问题原因和有问题的浏览器后，使用单独的样式表，仅供出现问题的浏览器加载。这种方法需要使用服务器端渲染。
- 使用css库，如bootstrap

# css优化、提升性能的方式有哪些

首先，浏览器从最右边的选择器（即关键选择器），向左依次匹配。根据关键选择器，浏览器从DOM中筛选出元素，然后向上遍历被选元素的父元素，查看是否匹配，选择器匹配语句链越短，浏览器匹配速度越快。

- 避免使用标签和通用选择器作为关键选择器
- 多个css合并压缩，尽量减少http请求
- css雪碧图

# css加载方式有几种？

- 外部样式表(`link/@important`)
- 内部样式表(`<style></style>)
- 内联样式

# @import 有什么作用？如何使用？

@important用于将样式表导入到另一个样式表，必须在文档顶部声明样式规则
语法
````
@import url|string list-of-mediaqueries;
````
属性值：

●　url|string：url或string表示要导入的资源的位置；网址可以是相对的或绝对的。

●　media-of-mediaqueries：媒体查询列表决定了链接URL中定义的CSS规则的应用。

使用方式：
````
@import url("reset.css")
````
