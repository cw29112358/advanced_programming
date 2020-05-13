### 函数表达式

定义函数有两种方式：函数声明语句和函数表达式

函数有一个 name 属性。通过这个属性可以访问到给函数指定的名字。也就是等于跟在 function 关键字后面的标识符。

函数声明：存在函数声明提升

函数表达式：不存在函数提升

1. 递归

函数递归是函数通过名字调用自身构成的情况

```
function factorial(num) {
  if (num <= 1) {
    return 1
  } else {
    return num * factorial(num - 1)
  }
}
```

解除强耦合

```
function factorial(num) {
  if (num <= 1) {
    return 1
  } else {
    return num * arguments.callee(num - 1)
  }
}
```

严格模式下可以这样定义

```
var factorial = function f(num) {
  if (num <= 1) {
    return 1
  } else {
    return num * f(num - 1)
  }
}
```

2. 闭包

闭包是指有权访问另一个函数作用域中的变量的函数。

```
function plusNum() {
  var num = 0
  return function() {
    ++num
    return num
  }
}
```

+ 闭包与变量

副作用：闭包只能取得包含环境中任何变量的最后一个值。

```
function createFunctions() {
  var result = []
  for (var i = 0; i < 5; i++) {
    result[i] = fuction() {
      return i
    }
  }
  return result
}

createFunction() // [5, 5, 5, 5, 5]
```

解决方法：

```
function createFunction() {
  var result = []
  for (var i = 0; i < 5; i++) {
    result[i] = fuction(num) {
      return fuction() {
        return num
      }
    }(i)
  }
}

createFunction() // [1, 2, 3, 4, 5]
```

+ 关于 this

this 对象是在运行时基于函数的执行环境绑定的：在全局作用域中，this 等于 window，而当函数作为某个对象的方法调用时，this 等于那个对象。

```
var name = 'window'

var obj = {
  var name = 'obj'

  getName = function() {
    return function() {
      return this.name
    }
  }
}

obj.getName()() // 'window'
```

```
var name = 'window'

var obj = {
  var name = 'obj'

  getName = function() {
    var _this = this

    return function() {
      return _this.name
    }
  }
}

obj.getName()() // 'obj'
```

+ 内存泄漏

由于闭包会携带包含它的函数作用域，因此会比其他函数占用更多的内存。而且，一不在意就会导致内存泄漏。

3. 模仿块级作用域

将函数声明包含在一对圆括号中，表示它实际上是一个函数表达式。而紧随其后的圆括号会立即调用这个函数。

```
(function() {
  // 这里是块级作用域
})()
```

以下代码是错误的：因为 JS 会将 function 关键字当作一个函数声明的开始，而函数声明后面不能跟圆括号。

```
function() {
  // 这里是块级作用域
}()
```

无论在什么地方，只需要临时创建一些变量，就可以使用私有作用域。这种做法可以减少闭包占用的内存问题，因为没有指向匿名函数的引用。只要函数一执行完毕，就可以立即销毁其作用域链了。

```
(function() {
  var now = new Date()
  if (now.getMonth() == 0 && now.getDate() == 1) {
    alter('happy new year'!h)
  }
})()
```

4. 私有变量

任何在函数中定义的变量，都可以认为是私有变量，因为不能在函数的外部访问这些变量。

如果在这个函数内部创建一个闭包，那么闭包可以通过作用域链访问外部函数的私有变量和私有方法。这些公有方法我们叫做特权方法

```
function MyObject() {
  // 私有变量
  var privateVariable = 10

  // 私有方法
  function privateFunction() {
    return false
  }

  // 特权方法
  this.publicMethod = function() {
    privateVariable++
    return privateFunction()
  }
}
```

+ 静态私有变量

> 注意：初始化一个未经声明的变量，总是会创建一个全局变量，因此 MyObject 就成了一个全局变量，能够在私有作用域之外被访问到。

> 以下方式在严格模式下会报错：给未经声明的变量赋值会报错

```
(function() {
  // 私有变量
  var privateVariable = 10

  // 私有方法
  function privateFunction() {
    return false
  }

  MyObject = new Object()

  // 特权方法
  MyObject.prototype.publicMethod = function() {
    privateVariable++
    return privateFunction()
  }
})()
```

+ 模块模式

模块模式是为单例创建私有变量和特权方法。所谓单例：指的就是只有一个实例的对象。

```
var singleton = function() {
  // 私有变量
  var privateVariable = 10

  // 私有方法
  function privateFunction() {
    return false
  }

  return {
    publicProperty: true

    publicMethod: function() {
      privateVariable++
      return privateFunction()
    }
  }
}
```

如果必须创建一个对象，并且以某种数据对其进性初始化，同时还要公开一些可以访问这些私有变量的方法，那么就可以采用模块模式。

+ 增强的模块模式

增加的模块模式适用于那些单例必须是某种类型的实例，同时还必须添加某些属性或方法对其加以增强的情况。

```
var singleton = function() {
  // 私有变量
  var privateVariable = 10

  // 私有方法
  function privateFunction() {
    return false
  }

  var obj = new CustomType()

  obj.publicProperty = true

  obj.publicMethod = function() {
    privateVariable++
    return privateFunction()
  }

  return obj
}
```