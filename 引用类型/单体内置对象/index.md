### 单体内置对象

由 ECMAScript 实现提供的、不依赖于宿主环境的对象，这些对象在 ECMAScript 程序执行之前就已经存在了。

Object,Array,String...

Global,Math...

1. Global

简单来说，不属于其他任何对象的属性和方法，最终都是它的属性和方法。

+ URI 编码方法

encodeURI(),encodeURIComponent(),decodeURI(),decodeURIComponent()

+ eval()方法：完整的 ECMAScript 解析器。接收一个参数，即要执行的 ECMAScript 字符串

+ Global 对象的属性

undefined, NaN, Infinity, Object, Array, Function, Boolean, String, Number

+ window 对象

Web 浏览器的全局对象一般指 window 对象。因此，在全局作用域中声明的所有变量和函数，都会成为 window 对象的属性。

2. Math 对象：用于保存数学公式和信息

