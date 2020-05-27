- [数据类型](#数据类型)

# 数据类型

`Javascript`有7种内置数据类型。包括基本类型和对象类型

基本类型包括
- string(字符串)
- boolean(布尔值)
- number(数字)
- symbol(符号)
- null(空值)
- undefined(未定义)
注意
    - `string`、`number`、`boolean`、`null`、`undefined`这五种统称为**原始类型**,表示不能再细分下去的基本类型
    - `null`和`undefined`通常被认为特殊值，这两种类型的值唯一，就是其本身
    - `symbol`是es6新增的数据类型，表示独一无二的值，通过Symbol()函数生成，由于生成的`symbol`值为最原始的值，所以不能用`new`生成。每个`symbol`都是独一无二的，所以比较两个`symbol`值总是`false`.
