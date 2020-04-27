### Object

创建 Object 实例有两种方式：一是使用 new 操作符后跟 Object 构造函数。二是使用对象字面量表示法。

```
var person = new Object()

person.name = 'chen'
```

```
var person = {
  name: 'chen'
}
```

访问对象属性有两种方式：一是点表示法；二是方括号表示法（优点是可以通过变量来访问对象属性）。

```
var person = {
  name: 'chen',
  'hello world': 'hello'
}

person.name // 'chen'
person['name'] // 'chen'

var property = 'name'
person[property] // 'chen'

person['hello world'] // 'hello'
```

通常，除非必须使用变量来访问属性，否则建议使用点表示法访问对象属性。