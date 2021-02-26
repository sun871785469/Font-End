- [数据类型](#数据类型)
- [弱类型语言](#弱类型语言)
- [事件流](#事件流)
- [vue 的优点是什么？](#vue 的优点是什么？)
- [什么是vue生命周期？对vue生命周期的理解？](#什么是vue生命周期？对vue生命周期的理解？)
- [vue 的双向绑定的原理是什么](#vue 的双向绑定的原理是什么)

# 数据类型

`Javascript`有7种内置数据类型。包括基本类型和对象类型

### 基本类型包括
- string(字符串)
- boolean(布尔值)
- number(数字)
- symbol(符号)
- null(空值)
- undefined(未定义)
- Object

注意

- `string`、`number`、`boolean`、`null`、`undefined`这五种统称为**原始类型**,表示不能再细分下去的基本类型
- `null`和`undefined`通常被认为特殊值，这两种类型的值唯一，就是其本身
- `symbol`是es6新增的数据类型，表示独一无二的值，通过Symbol()函数生成，由于生成的`symbol`值为最原始的值，所以不能用`new`生成。每个`symbol`都是独一无二的，所以比较两个`symbol`值总是`false`.

### 对象类型-Object

对象类型也叫引用类型,`array`和`function`是对象的子类型。对象在逻辑上是属性的无序集合，是存放各种值的容器。对象值存储的是引用地址，所以和基本类型值不可变的特性不同，对象值是可变的。

# 弱类型语言
`Javascript`是弱类型语言，在声明变量的时候并没有预先设定好类型，变量的类型也就是其值得类型，也就是说变量当前的类型由其值得类型决定。

# 事件流

定义：用户与浏览器当前页面的交互过程
三个阶段：捕获阶段、目标阶段、冒泡阶段

# vue 的优点是什么？

- 低耦合。视图（View）可以独立于 Model 变化和修改，一个 ViewModel 可以绑定到不同的"View"上，当 View 变化的时候 Model 可以不变，当 Model 变化的时候 View 也可以不变。
- 可重用性。你可以把一些视图逻辑放在一个 ViewModel 里面，让很多 view 重用这段视图逻辑。
- 独立开发。开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计，使用 Expression Blend 可以很容易设计界面并生成 xml 代码。
- 可测试。界面素来是比较难于测试的，而现在测试可以针对 ViewModel 来写。

# 什么是vue生命周期？对vue生命周期的理解？

- vue生命周期的作用是什么？

答：它的生命周期中有多个事件钩子，让我们在控制整个Vue实例的过程时更容易形成好的逻辑。

- 第一次页面加载会触发哪几个钩子？

答：第一次页面加载时会触发 beforeCreate, created, beforeMount, mounted 这几个钩子

- 生命周期

答：总共分为 8 个阶段创建前/后，载入前/后，更新前/后，销毁前/后。

创建前/后： 在 beforeCreate 阶段，vue 实例的挂载元素 el 还没有。
载入前/后：在 beforeMount 阶段，vue 实例的$el 和 data 都初始化了，但还是挂载之前为虚拟的 dom 节点，data.message 还未替换。在 mounted 阶段，vue 实例挂载完成，data.message 成功渲染。
更新前/后：当 data 变化时，会触发 beforeUpdate 和 updated 方法。
销毁前/后：在执行 destroy 方法后，对 data 的改变不会再触发周期函数，说明此时 vue 实例已经解除了事件监听以及和 dom 的绑定，但是 dom 结构依然存在

- 简单描述每个周期具体适合哪些场景？

答：生命周期钩子的一些使用方法： beforecreate : 可以在这加个loading事件，在加载实例时触发 created : 初始化完成时的事件写在这里，如在这结束loading事件，异步请求也适宜在这里调用 mounted : 挂载元素，获取到DOM节点 updated : 如果对数据统一处理，在这里写上相应函数 beforeDestroy : 可以做一个确认停止事件的确认框 nextTick : 更新数据后立即操作dom
arguments是一个伪数组，没有遍历接口，不能遍历

# vue 的双向绑定的原理是什么

- vue.js 是采用数据劫持结合发布者-订阅者模式的方式，通过 Object.defineProperty()来劫持各个属性的 setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。
具体步骤：
第一步：需要 observe 的数据对象进行递归遍历，包括子属性对象的属性，都加上 setter 和 getter 这样的话，给这个对象的某个值赋值，就会触发 setter，那么就能监听到了数据变化
第二步：compile 解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图
第三步：Watcher 订阅者是 Observer 和 Compile 之间通信的桥梁，主要做的事情是:

  在自身实例化时往属性订阅器(dep)里面添加自己
  自身必须有一个 update()方法
  待属性变动 dep.notice()通知时，能调用自身的 update() 方法，并触发 Compile 中绑定的回调，则功成身退。
第四步：MVVM 作为数据绑定的入口，整合 Observer、Compile 和 Watcher 三者，通过 Observer 来监听自己的 model 数据变化，通过 Compile 来解析编译模板指令，最终利用 Watcher 搭起 Observer 和 Compile 之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据 model 变更的双向绑定效果。
