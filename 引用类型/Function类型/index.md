### Function

函数实际上是对象。每个函数都是 Function 构造函数的实例，具有属性和方法。函数名实际上是一个指向函数对象的指针。

函数通常使用函数声明语法和函数表达式两种方式创建

```
function sum(num1, num2) {
  return num1 + num2
}

var sum = function(num1, num2) {
  return num1 + num2
}
```

Function 构造函数定义函数。但是并不推荐使用这种方式创建函数

```
var sum = new Function('num1', 'num2', 'return num1 + num2')
```

函数名仅仅是指向函数的指针,一下栗子可以很好证明

```
function sum(num1, num2) {
  return num1 + num2
}

sum(10, 10) // 20

var anotherSum = sum
anotherSum(10, 10) // 20

sum = null
anotherSum(10, 10) // 20
```

+ 没有重载

定义两个同名函数， 结果是后者会覆盖前者。

+ 函数声明与函数表达式

解析器在向指向环境中加载数据时，对函数声明和函数表达式并非一视同仁。

解析器会率先读取函数声明，使其在执行任何代码前可用（可以访问）；至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被解释执行。

+ 作为值的函数

```
function callSomeFunction(someFunction, someArgument) {
  return someFunction(someArgument)
}
```

```
function createComparisonFunction(propertyName) {
  return function(obj1, obj2) {
    var value1 = obj1[properName]
    var value2 = obj2[properName]

    if (value1 < value2) {
      return -1
    } else if (value1 > value2) {
      return 1
    } else {
      return 0
    }
  }
}
```

+ 函数的内部属性

在函数内部，有两个特殊的属性：arguments 和 this。

arguments：一个类数组，包含传入函数中的所有参数。arguments有一个 callee 的属性，这个属性是一个指针，指向拥有这个 arguments 对象的函数。

栗子：阶乘函数

```
function factorial(num) {
  if (num <= 1) {
    return 1
  } else {
    return num * factorial(num - 1)
  }
}
```

在函数有名字，并且名字以后也不会变的情况下，这样定义没有问题。但问题是这个函数的执行与函数名 factorial 紧紧耦合在一起，为了消除这个紧耦合的现象，可以这样定义：

```
function factorial(num) {
  if (num <= 1) {
    return 1
  } else {
    return num * arguments.callee(num - 1)
  }
}

var trueFactorial = factorial;

factorial = function() {
  return 0
}

trueFactorial(3) // 6
factorial(3) // 0
```

this: 引用的时函数据以执行的环境对象

```
window.color = 'red'
var o = {
  color: 'blue'
}

function sayColor() {
  return this.color
}

sayColor() // 'red'
o.sayColor = sayColor
o.sayColor() // 'blue'
```

> 注意，以上例子中，函数的名字仅仅是包含指针的对象。因此，即使是在不同的执行环境中执行，全局的 sayColor() 和 o.sayColor() 指向的仍然是堆中的同一个函数

除了 callee 外，还有一个函数对象的属性：caller。这个属性中保存这调用当前函数的函数的引用。

```
function outer() {
  inner()
}

function inner() {
  return inner.caller
}

outer() // 返回 outer 函数的源代码，因为 inner 是由 outer 函数调用的
```

松耦合写法

```
function outer() {
  inner()
}

function inner() {
  return arguments.callee.caller
}

outer() // 返回 outer 函数的源代码，因为 inner 是由 outer 函数调用的
```

> 在严格模式下运行时，访问 arguments.caller 会导致错误。在非严格模式下 arguments.caller 始终返回 undefined。定义这个属性是为了分清 arguments.caller 和函数的 caller。严格模式还有一个限制：不能为 caller 属性赋值。

+ 函数的属性和方法

每个函数都包含两个属性：length（函数希望接收的命名参数的个数） 和 prototype（保存函数所有实例方法的真正所在）。

prototype 属性是不可枚举的，所以使用 for-in 是无法访问的

每个函数都包含三个非继承而来的方法：apply()、call()、bind()

apply()：接收两个参数，第一个参数是运行函数的作用域，第二个是参数数组（数组或者arguments对象）。直接执行

call(): 第一个参数是 this，跟 apply() 的第一个参数相同，其余参数都是直接传递给函数的。直接执行

bind(): 第一个参数是 this，跟 apply() 的第一个参数相同，其余参数都是直接传递给函数的。创建一个函数的实例

```
window.color = 'red'
var o = { color: 'blue' }

function sayColor() {
  return this.color
}

sayColor() // 'red'
sayColor.call(this) // 'red'
sayColor.call(window) // 'red'
sayColor.call(o) // 'blue
```