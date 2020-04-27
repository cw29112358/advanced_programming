### Array

创建数组有两种方式：一是使用 Array 构造函数；二是使用数组字面量表示法。

```
var color = new Array() // 创建空数组
var color1 = new Array(20) // 创建长度为20，且每个项的值都为 '' ，的数组
var color2 = new Array('red') // 创建长度为1，第一项的值为 'red' 的数组

// 省略 new 操作符
var color3 = Array(20)
var color = Array('red')
```

```
var color = []
var color1 = [1, 2]
```

在读取和设置值时，要使用方括号比你高提供相应值的基于 0 的数字索引。

```
var color = ['red']

color[0] // 'red'
color[1] = 'blue'
color // ['red', 'blue']
```

+ 检测数组

```
var arr = []

typeof arr // 'object'
arr instanceof Array // true
Array.isArray(arr) // true
Object.prototype.toString.call(arr) // '[object Array]'
```

+ 转换方法

```
var person1 = {
  toLocaleString: function() {
    return 'chen'
  },
  toString: function() {
    return 'li'
  }
}

var person2 = {
  toLocaleString: function() {
    return 'yang'
  },
  toString: function() {
    return 'bai'
  }
}

var people = [person1, person2]

people // [{ toLocaleString: function() { return 'chen' }, toString: function() { return 'li' } }, { toLocaleString: function() { return 'yang' }, toString: function() { return 'bai' } }]

people.valueOf() // [{ toLocaleString: function() { return 'chen' }, toString: function() { return 'li' } }, { toLocaleString: function() { return 'yang' }, toString: function() { return 'bai' } }]

people.toString() // li,bai

people.toLocaleString() // chen,yang
```

+ 会改变原始数组的方法

push()
copyWithin()
fill()
pop()
reverse()
shift()
sort()
splice()
unshift()

+ 不会改变原始数组的方法

concat()
filter()
forEach()
join()
map()
reduce()
slice()