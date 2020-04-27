## 变量

### 基本类型和引用类型

+ 动态的属性

定义基本类型值和引用类型值的方式是类似的：创建一个变量并为该变量赋值。但是，当这个值保存到变量中以后，对不同类型的值可以执行的操作则大相径庭。

+ 复制变量的值

如果从一个变量向另一个变量复制基本类型值，会在变量对象上创建一个新值，然后把该值复制到为新变量分配的位置上。以后这两个变量可以参与任何操作而互相不受影响。
从一个变量向另一个变量复制引用类型值，会将存储在变量对象中的值复制一份放到为新变量分配的空间中。不同的是，这个值的副本实际上是一个指针，而这个指针指向存储在堆中的一个对象。复制操作结束后，两个变量实际上将引用同一个对象。

+ 传递参数

所有函数都是按值传递的，也就是说，把函数外部的值复制给函数内部的参数，就和把值从一个变量复制到另一个变量一样。
在向函数传递基本类型的值时，被传递的值会被复制给一个局部变量（即命名参数）。
在向函数传递引用类型的值时，会把这个值在内存中的地址复制给一个局部变量，因此这个局部变量的变化会反应在函数的外部。

```
function setName(obj) {
    obj.name = 'chen'
    obj = new Object()
    obj.name = 'li
    return obj
}

var person = new Object()
var person1 = setName(person)
person.name // 'chen'
person1.name // 'li'
```

+ 检测类型

typeof操作符是确定一个变量是字符串、数值、布尔、还是 undefined 的最佳工具。如果变量是一个对象或者是 null，则 typeof 操作符会返回 'object'

instanceof可以检测引用类型的变量。栗子: person instanceof RegExp

```语法
result = variable instanceof constructor
```

Object.prototype.toString.call(variable): 目前来说是确定变量类型的最佳方式

### 基本类型

undefined,null,string,boolean,number,symbol

### 引用类型

引用类型的值是保存在内存中的对象：Object,Array,Function,Math,RegExp...