## 需要经常使用的方法


### 字符串

项目 | ECMAScript 版本 | 用途 | 说明
--- | --- | --- | ---
`str.codePointAt()` | ES2015 | 返回一个字符的码点 | 能够正确处理 4 个字节储存的字符（（Unicode 码点大于`0xFFFF`的字符））
`String.fromCodePoint()` | ES2015 | 从码点返回对应字符 | 可以识别大于`0xFFFF`的字符，弥补了String.fromCharCode方法的不足
`for (let ch of str)` | ES2015 | 字符串的遍历器接口 |
`str.at()` | 提案 | 返回对应位置的字符 | 可以识别 Unicode 编号大于`0xFFFF`的字符，
`str.normalize()` | ES6 | 将字符的不同表示方法统一为同样的形式，这称为 Unicode 正规化 |
`str.includes()` | ES2015 | 返回布尔值，表示是否找到了参数字符串 | 支持第二个参数，表示开始搜索的位置
`str.startsWith()` | ES2015 | 返回布尔值，表示参数字符串是否在原字符串的头部 | 支持第二个参数，表示开始搜索的位置
`str.endsWith()` | ES2015 | 返回布尔值，表示参数字符串是否在原字符串的头部 | 支持第二个参数，表示开始搜索的位置，针对前 n 个字符
`str.repeat()` | ES2015 | 返回一个新字符串，表示将原字符串重复n次。 |
`str.matchAll()` | ES2015 |  |
`str.padStart()` | ES2017 | 返回一个新字符串，表示将原字符串重复n次。 |
`str.padEnd()` | ES2017 | 某个字符串不够指定长度，会在头部补全 |
模板字符串 | | |
`String.raw()` | ES6 | 返回一个斜杠都被转义（即斜杠前面再加一个斜杠）的字符串 |


## 数值

项目 | ECMAScript 版本 | 用途 | 说明
--- | --- | --- | ---
`Number.isFinite()` | ES2015 | 检查一个数值是否为有限的（finite），即不是`Infinity` | 如果参数类型不是数值，一律返回`false`
`Number.isNaN()` | ES2015 | 检查一个值是否为`NaN` | 如果参数类型不是数值，一律返回`false`
`Number.parseInt()` | ES2015 | 同全局方法`parseInt()` | 减少全局性方法，使得语言逐步模块化
`Number.parseFloat()` | ES2015 | 同全局方法`parseFloat()` | 减少全局性方法，使得语言逐步模块化
`Number.isInteger()` | ES2015 | 判断一个数值是否为整数 | 如果参数不是数值，返回`false`
`Number.EPSILON` | ES2015 | 常量，JavaScript 能够表示的最小精度 | 对于 64 位浮点数来说，就等于 2 的 -52 次方
`Number.MAX_SAFE_INTEGER` | ES2015 | 常量，最大安全整数 | 值为`Math.pow(2, 53) - 1`，即 9007199254740991
`Number.MIN_SAFE_INTEGER` | ES2015 | 常量，最小安全整数 | 值为`-Math.pow(2, 53) + 1`，即 -9007199254740991
`Number.Number.isSafeInteger()` | ES2015 | 判断整数是否在最大最小安全整数之间，即在`-2^53`到`2^53`之间（不含两个端点）之间 | 如果参数不是整数，一律返回`false`


### 数组

项目 | ECMAScript 版本 | 用途 | 说明
--- | --- | --- | ---
`Array.isArray` | ES5 | 判断参数是否是数组 |
`...`（扩展运算符） | ES2015 | 将数组转为用逗号分隔的参数序列 | 注意与函数的`rest`参数区分开
`Array.from()` | ES2015 | 将两类对象（类似数组的对象、可遍历的对象）转为真正的数组 | 1、可遍历的对象包括 ES6 新增的数据结构 Set 和 Map；2、该方法可以接受第二、三个参数
`Array.of()` | ES2015 | 将一组值转换为数组 | `Array.of(3, 11, 8) // [3,11,8]`
`array.copyWithin()` | ES2015 | 在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组 | 调用该方法会修改当前数组
`array.find(fn)` | ES2015 | 找出第一个符合条件的数组成员 | 类似于`some`，但是会返回匹配的元素
`array.findIndex(fn)` | ES 2015 | 找出第一个符合条件的数组索引 | 类似于`some`，但是会返回匹配的元素的索引
`array.fill()` | ES2015 | 将数组所有元素替换成给定值 | 第二、三个参数指定替换的起始结束位置
`array.entries()` | ES2015 | 遍历数组的键值对，返回遍历器对象 |
`array.keys()` | ES2015 | 遍历数组的键名，返回遍历器对象 |
`array.values() ` | ES2015 | 遍历数组的键值，返回遍历器对象 |
`array.includes()` | ES2016 | 检测数组是否包含给定的值，返回布尔值 | 类似字符串的`includes`方法

注意：

- 使用这些方法时，请详细[查阅文档](http://es6.ruanyifeng.com/#docs/array)，方法可能存在多个参数的情况
- 数组里避免使用[空位](http://es6.ruanyifeng.com/#docs/array#%E6%95%B0%E7%BB%84%E7%9A%84%E7%A9%BA%E4%BD%8D)


### 对象


### 函数

项目 | ECMAScript 版本 | 用途 | 说明
--- | --- | --- | ---
`...变量名`（`rest`参数） | ES 2015 | 获取函数的多余参数 | 仅用于函数声明时，注意与数组的扩展运算符的区别