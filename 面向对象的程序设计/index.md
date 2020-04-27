## 面向对象

面向对象的语言有一个标志，那就是他们都有类的概念，而通过类可以创建任意多个具有相同属性和方法的对象。

> 对象是无序属性的集合，其属性可以包含基本值、对象或者函数。

### 对象的类型

#### 属性类型
+ 数据属性：只有在内部才用的特性，描述了属性的各种特性
  + **[[Configurable]]:** 表示能否通过delete删除属性，从而重新定义属性；能都修改属性的特性；或者能否把属性修改为访问器属性。对于直接在对象上定义的属性，默认值为：true
  + **[[Enumerable]]:** 表示能否通过for-in循环返回属性。对于直接在对象上定义的属性，默认值为true
  + **[[Writable]]:** 表示能否修改属性的值。对于直接在对象上定义的属性，默认值为true
  + **[[Vable]]:** 包含这个属性的数据值。读取属性值的时候在这个位置读取，写入属性值的时候把新值保存在这个位置。默认值为undefined
  
  要修改属性的默认特性，必须使用Object.defineProperty()方法。这个方法接收三个参数：属性所在的对象、属性的名称和一个描述符对象。其中，描述符对象的属性必须是configerable、enumerable、writable、value。设置其中的一或多个值，可以修改对应的特性值

+ 访问器属性：不包含数据值，它们包含一对儿getter和setter函数（不是必须的）。在读取属性时会调用getter函数，这个函数负责返回有效的值。在写入访问器属性时，会调用seetter函数，这个函数负责决定如何处理数据
  + **[[Configurable]]:** 表示能否通过delete删除属性，从而重新定义属性；能都修改属性的特性；或者能否把属性修改为访问器属性。对于直接在对象上定义的属性，默认值为：true
  + **[[Enumerable]]:** 表示能否通过for-in循环返回属性。对于直接在对象上定义的属性，默认值为true
  + **[[getter]]:** 在读取属性时调用的函数。默认值为undefined
  + **[[setter]]:** 在写入属性时调用的函数。默认值为undefined
  
访问器属性不能直接定义，必须使用 **Object.defineProperty()** 来定义。
两个非标准方法：**__defineDetter__** 和 **__defineSetter__**

#### 定义多个属性

**Object.defineProperties()** 方法可以通过描述符一次定义多个属性。这个方法接收两个对象参数：第一个对象是要添加和修改器属性的对象第二个对象的属性与第一个对象中要添加或修改的属性一一对应

```
var book = {}
Object.defineProperties(book, {
    _year: {
        value: 2020,
    },
    edition: {
        value: 1
    },
    year: {
        get: function() {
            return this._year;
        },
        set: function(newValue) {
            if (newValue > 2020) {
                this._year = newValue;
                this.edition = newValue - 2020;
            }
        }
    }
})
```

#### 读取属性的特性

**Object.getOwnPropertyDescriptor(),** 可以取得给定属性的描述符。这个方法接收两个参数：属性所在的对象和读取其描述符的属性名称。返回值是一个对象，如果是访问器属性，这个对象的属性有：configurable、enumerable、get、set。如果是数据属性，这个对象的属性有：configurable、enumerable、writable、value

在实例对象上调用该方法会取得实例对象的属性，在原型对象上调用该方法会取得原型对象的属性

#### 创建对象

+ 创建实例，为实例添加属性和方法
```
var obj = new Object()
obj.name = 'chen'
```

+ 对象字面量
```
var obj = { name: 'chen' } 
```

+ 工厂模式
```
function createPerson(name) {
    var person = new Object()
    person.name = name
    person.sayname = function() {
        alter(this.name)
    }
    return person
}

var person1 = createPerson('chen')
var person2 = createPerson('li')
```

+ 构造函数模式
```
function Person(name) {
    this.name = name
    this.sayName = function() {
        alter(this.name)
    }
}

var person1 = new Person('chen')
var person2 = new Person('li')
```

**new 一个实例经历的过程**
  1. 创建一个新的对象
  2. 将构造函数的作用域赋值给新的对象（因此this就指向了这个新的对象）
  3. 执行构造函数中的代码
  4. 返回新对象

> 将构造函数当作函数

构造函数和其他函数的唯一区别就在于他们的调用方式不同。任何函数，只要通过 new 操作符来调用，那它就可以作为构造函数；任何函数，不通过 new 操作符来调用，那它跟普通函数页没啥区别

```
function Person(name) {
    this.name = name
    this.sayname = function() {
        alter(this.name)
    }
}

<!--当作构造函数使用-->
var person1 = new Person('chen')
person1.sayname() // 'chen'

<!--当作普通函数调用-->
Person('chen')
window.sayname() // 'chen'

<!--在另一个对象的作用域中调用-->
var person2 = new Object()
Person.call(person2, 'chen')
person2.sayname() // 'chen'
```

> 构造函数的问题

每个方法都要在每个实例上重新创建一遍

```
<!--这样写更容易理解-->
function Person(name) {
    this.name = name
    this.sayname = new Function('alter(this.name)')
}

<!--因此，不同实例上的同名函数是不相等的-->
person1.sayname === person2.sayname // false
```

然而，创建两个完成同样任务的Function实例的确没有必要，况且有this对象在，根本不用在执行代码前就把两个函数绑定在特定的对象上面。因此可以把函数定义转移到构造函数外部来解决这个问题。在构造函数内部，我们将sayname设置成等于全局的sayname函数。这样一来，由于sayname包含的是一个指向函数的指针，因此，创建出的对象共享了同一个sayname函数。这也引发了另一个问题，在全局作用域中的函数实际上只能被某一个对象调用，这让全局作用域有点名不副实，而且如果对象中需要定义很多方法，那么就要定义很多全局函数，那这个构造函数就毫无封装性可言了。

```
function Person(name) {
    this.name = name
    this.sayname =sayname
}

function sayname() {
    alter(this.name)
}
```

+ 原型模式
```
function Person() {}
Person.prototype.name = 'chen'
Person.prototype.sayname = function() {
    alter(this.name)
}
var person1 = new Person()
var person2 = new Person()
person1.sayname() // 'chen'
person2.sayname() // 'chen'
person1.sayname === person2.sayname // true
```

与构造函数不同的是，新对象的所有属性和方法是由所有实例共享的。

1. 理解原型对象
  无论什么时候只要创建了一个新函数，就会根据一定规则为该函数创建一个 **prototype** 属性，这个属性指向函数的原型对象。在默认情况下所有原型对象都会自动获得一个 **constructor** （构造函数）属性，这个属性包含一个指向 **prototype** 属性所在函数的指针。
  创建了自定义构造函数之后，其原型对象默认只会取得 **constructor** 属性；至于其他属性和方法，则都是通过Object继承而来的。当调用构造函数创建一个新实例时，该实例内部将包含一个指针（**__proto__**），指向构造函数的原型对象。

**isPrototypeOf()：** 如果原型对象是指向调用isPrototype()方法的对象，那么返回true

```
Person.prototype.isPrototypeOf(person1) // true
Person.prototype.isPrototypeOf(person2) // true
```

**Object.getPrototypeOf():** 取得实例对象的原型对象

```
Object.getPrototypeOf(person1) === Person.prototype // true
Object.getPrototypeOf(person2.name) // 'chen'
```

**hasOwnProperty():** 检测该属性是存在于实例对象中还是存在于原型对象中，如果存在于实例对象上就返回true，如果存在于原型上就返回false



> 原型链的概念
> 每当代码读取某个对象的某个属性时，都会执行一次搜索，目标是具有给定名字的属性。搜索首先从对象实例开始。如果在实例中找到了具有给定名字的属性，则返回该属性的值；如果没有找到，则继续搜索指针指向的原型对象，在原型对象中查找具有给定名字的属性；如果在原型对象中找到了这个属性，则返回该属性值；如果未找到，按这个逻辑找上去，一直找到倒数第二层Object，末尾null。如果还未找到，则返回undefined

> 每当给实例对象添加一个属性时，这个属性会覆盖原型对象中保存的同名属性；换句话说，添加这个属性只会阻止我们访问原型中的那个属性，但不会修改原型中那个属性的值，即使将这个值设为null。不过，使用delete操作符，可以完全删除该属性，从而让我们能够重新访问原型中的那个属性。

2. 原型与 in 操作符

  单独使用 in 操作符：单独使用时in操作符会在通过对象能够访问给定属性时返回true,不管属性存在于实例对象还是原型对象
  
  for-in：在使用for-in循环时，返回的是所有能够通过对象访问的、可枚举的（enumerable为true）属性，其中既包括存在于实例中的属性，也包括存在于原型中的属性。更包括屏蔽了原型对象中不可枚举属性（enumerable）的实例属性。
  
**Object.keys():** 返回对象上所有可枚举的实例属性，这个方法接收一个对象作为参数，返回一个包含所有可枚举属性的字符串数组

**Object.getOwnPropertyNames():** 返回实例对象上所有实例属性，不管该属性是否可枚举

Object.keys()和Object.getOwnPropertyNames()方法都可以用来替换for-in循环

3. 更简单的原型语法

为减少不必要的输入，也为从视觉上更好地封装原型的功能，常见的作法是用一个包含所有属性和方法的对象字面量来重新整个原型

```
function Person() {}
Person.prototype = {
    name: 'chen',
    sayname: function() {
        alter(this.name)
    }
}
```

注意：上述方法的 constructor 属性不再指向 Person 函数。因为每创建一个函数，同时创建了它的 prototype 对象，这个对象也会自动获得 constructor 属性，上述写法覆盖了构造函数的原型对象，因此 Person 的 constructor 属性也就变成了新对象的 constructor 属性（指向 Object 构造函数），不再指向 Person 函数了。

```
var person1 = new Person()

person1 instanceof Object // true
person1 instanceof Person // true
person1.constructor === Person // false
person1.constructor === Object // true
```

如果 constructor 真的很重要，可以将 constructor 设置回指定函数

```
function Person() {}
Person.prototype = {
    constructor: Person,
    name: 'chen',
    sayname: function() {
        alter(this.name)
    }
}
```

但是这方式会使 constructor 属性的 enumerable 设置为默认值 true。而默认情况下，原生的constructor属性是不可枚举的。这种情况我们可以通过 Object.defineProperty() 改写成以下形式

```
function Person() {}

Person.prototype = {
    name: 'chen',
    sayname: function() {
        alter(this.name)
    }
}

Object.defineProperty(Person, 'constructor', {
    enumerable: false,
    value: Person,
})
```

4. 原型的动态性

尽管可以随时为原型添加属性和方法，并且修改能够立即在所有对象实例中体现出来，但如果是重写整个原型对象那么情况就不一样了。我们知道，调用构造函数时会为实例添加一个指向最初原型的指针，而把原型修改为另外一个对象就等于切断了构造函数与最初原型之间的联系。请记住实例中的指针仅指向原型，不指向构造函数。

5. 原生对象的原型

原型模式的重要性不仅体现在创建自定义类型方面，就连所有原生的引用类型，都是采用这种模式创建的。所谓原生引用类型（Object、Array、String、等等）都在其构造函数的原型上定义了属性和方法。

通过原生对象的原型，不仅可以取得多有默认方法的引用，而且也可以定义新方法。

```
String.prototype.startsWith = function(text) {
    return this.indexOf(text) === 0
}
```
不推荐在产品化的程序中修改原生对象的原型。

6. 原型对象的问题

原型中所有属性是被很多实例共享的，这种共享对于函数非常适合。对于那些包含基本值的属性也说得过去，毕竟，通过实例上添加一个同名属性，可以覆盖原型对象中对应的属性。然后，对于包含引用类型值的属性来说，问题就比较突出了。

```
function Person() {}

Person.prototype = {
    friends: ['li', 'bai']
}

var person1 = new Person()
var person2 = new Person()

person1.friends === person2.friends // true

person1.friends.push('yang')

person1.friends // ['chen', 'li', 'zhao']
person2.friends // ['chen', 'li', 'zhao']
person1.friends === person2.friends // true
```

+ 组合使用构造函数模式和原型模式

构造函数模式用于定义实例属性和方法，而原型模式用于定义共享的方法和属性。结果，每一个实例都会有自己的一份实例属性的副本，但同时又共享着原型属性和方法，最大限度地节省了内存。

这是目前使用最广泛，认同度最高的一种自定义类型的方法。

+ 动态原型模式

把所有信息都封装在构造函数中，而通过在构造函数中初始化原型（仅在必要情况下），又保持了同时使用构造函数和原型的优点。

```
function Person(name) {
    this.name = name
    if (typeof this.sayname != 'function') {
        Person.prototype = function() {
            alter(this.name)
        }
    }
}
```

+ 寄生构造函数模式

这种模式的基本思想就是创建一个函数，该函数的作用仅仅是封装创建函数的代码，然后再返回新创建的对象

返回的对象与构造函数或者与构造函数的原型属性之间没有关系；也就是说，构造函数返回的对象与构造函数外部创建的对象没有什么不同。为此，不能依赖于 instanceof 操作符来确定对象类型。

```
function SpecialArray() {
    var newArray = new Array()
    newArray.push.call(newArray, arguments)
    newArray.toPipedString = function() {
        return this.join('|')
    }
    return newArray
}
```

+ 稳妥的构造函数模式

稳妥构造函数采用与寄生构造函数类似的模式，担忧两点不同：一是新创建对象的实例方法不使用this；二是不使用 new 操作符调用构造函数

```
function Person(name) {
    var newPerson = new Object()
    newPerson.sayname = function() {
        alter(name)
    }
    return newPerson
}

var person1 = Person('chen')
person1.sayname() // 'chen'
```

其中变量 person1 中保存的是一个稳妥对象，而除了调用sayname()方法之外，没有别的方式可以访问其数据成员。即使有其他代码可以给这个对象添加方法和属性，但也不可能有别的办法访问传入到构造函数中的原始数据。

## 继承

+ 原型链继承

利用原型让一个引用类型继承另一个引用类型的属性和方法。

```
function SuperType() {
    this.property = true
}

SuperType.prototype.getSuperValue = function() {
    return this.property
}

function SubType() {
    this.subProperty = false
}

SubType.prototype = new SuperType()

SubType.prototype.getSubValue = function() {
    return this.subProperty
}

var instance = new SubType()
instance.getSuperValue() // true
```

注意点：
1. 别忘了默认的原型：所有引用类型默认继承了 Object。

2. 确定原型和实例的关系：

```
instance instanceof Object // true
instance instanceof SuperType // true
instance instanceof SubType // true

Object.prototype.isPrototypeOf(instance) // true
SuperType.prototype.isPrototypeOf(instance) // true
SubType.prototype.isPrototypeOf(instance) // true
```

3. 谨慎地定义方法：子类型有时候需要重写超类型中的某个方法，或者需要添加超类型中不存在的某个方法。不管怎样，给原型添加方法的代码替换原型语句之后。

```
function SuperType() {
    this.property = true
}

SuperType.prototype.getSuperValue = function() {
    return this.property
}

function SubType() {
    this.subProperty = false
}

SubType.prototype = new SuperType()

SubType.prototype.getSubValue = function() {
    return this.subProperty
}

SubType.prototype.getSuperValue = function() {
    return false
}

var instance = new SubType()
instance.getSuperValue() // false
```

还有需要注意的地方，即在通过原型链实现继承时，不能使用对象字面量创建原型方法。因为这样做就会重写原型链。

```
function SuperType() {
    this.property = true
}

SuperType.prototype.getSuperValue = function() {
    return this.property
}

function SubType() {
    this.subProperty = false
}

SubType.prototype = new SuperType()

SubType.prototype = {
    getSubValue: function() {
        return this.subProperty
    }
}

var instance = new SubType()
instance.getSuperValue() // error
```

4. 原型链的问题

最大的问题是来自包含引用类型值的问题。

```
function SuperType() {
    this.colors = ['red']
}

function SubType() {}

SubType.prototype = new SuperType()

var instance1 = new SubType()
instance1.colors.push('blue')
instance.colors // ['red', 'blue']
var instance2 = new SubType()
instance2.colors = ['red', 'blue']
```

第二个问题是，在创建子类型实例时，不能在超类型的构造函数中传递参数。实际上，应该说是没有办法在不影响所有对象实例的情况下，给超类型的构造函数传递实例

+ 借助构造函数继承

在子类型的构造函数内部调用超类型的构造函数。

```
function SuperType() {
    this.colors = ['red']
} 

function SubType() {
    SuperType.call(this)
}

var instance1 = new SubType()
instance1.colors.push('blue')
instance1.colors // ['red', 'blue']

var instance2 = new SubType()
instance2.colors // ['red']
```

优势: 传递参数

```
function SuperType(name) {
    this.name = name
}

function SubType() {
    SuperType.call(this, 'chen')
    this.age = 28
}

var instance = new SubType()

instance.name // 'chen'
instance.age // 28
```

问题: 属性和方法都在构造函数中定义，因此，函数复用就无从谈起了。而且，超类型的原型中定义的属性和方法，对子类型而言也是不可见的。

+ 组合继承

将原型链和借用构造函数组合在一块，做到使用原型链实现对原型属性和方法的继承，通过借用构造函数来实现实例属性的继承。

```
function SuperType(name) {
    this.name = name
    this.colors = ['red']
}

SuperType.prototype.sayname = function() {
        alter(this.name)
    }

function SubType(name, age) {
    SuperType.call(this, name)
    this.age = age
}

SubType.prototype = new SuperType()
SubType.prototype.constructor = SubType
SubType.prototype.sayage = function() {
    alter(this.age)
}

var instance1 = new SubType('chen', 28)
instance1.colors.push('blue')
instance1.colors // ['red', 'blue']
instance1.sayname() // 'chen'
instance1.sayage() // 28

var instance2 = new SubType('li', 27)
instance2.colors = ['red']
instance2.sayname() // 'li'
instance2.sayage() // 27
```

组合继承避免了原型链和借助构造函数的缺陷，融合了它们的优点，成为 JS 中最常用的继承模式。

缺点：无论什么情况下，都会调用两次超类型构造函数：一次是在创建子类型的时候，另一次是在子类型构造函数内部。

+ 原型式继承

借助原型可以基于已有的对象创建新对象，同时还不必因此创建自定义类型

```
function Object(o) {
    function F() {}
    F.prototype = o
    return new F()
}
```

```
var person = {
    name: 'chen',
    friends: ['li'],
}

var antherPerson = Object(person)
antherPerson.friends.push('bai')
antherPerson.friends // ['li', 'bai']

var yetAntherPerson = new Object(person)
yetAntherPerson.friends.push('yang')
yetAntherPerson.friends // ['li', 'bai', 'yang']
```

**Object.create()** 规范化了原型式继承。该方法接收两个参数：一是用作新对象原型的对象和(可选的与 **Object.defineProperty()** 方法的第二个参数格式相同：每个属性都是通过自己的描述符定义的)一个为新对象定义额外属性的对象

```
var person = {
    name: 'chen',
    friends: ['li'],
}

var antherPerson = Object.create(person, {
    name: {
        value: 'chenw'
    }
})

antherPerson.name // 'chenw'
```

+ 寄生式继承

寄生式继承的思路与寄生构造函数和工厂模式类似，即创建一个仅用于封装继承过程的函数，该函数在内部以某种方法来增强对象，最后再像是它做了所有工作一样返回对象。

```
function createAnother(original) {
    var clone = object(original)
    clone.sayHi = function() {
        alter('hi')
    }
    return clone
}

var person = {
    name: 'chen'
}

var anotherPerson = createAnpther(person)
anotherPerson.sayHi() // 'hi'
```

+ 寄生组合式继承

通过借用构造函数来继承属性，通过原型链来继承方法。其背后的基本思路是：不必为了指定子类型的原型而去调用超类型的构造函数，我们所需要的无非就是超类型原型的一个副本而已。本质上就是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型的原型。

```
function inheritPrototype(subType, superType) {
    var prototype = object(superType.prototype)
    prototype.constructor = subType
    subType.prototype = prototype
}

function SuperType(name) {
    this.name = name
    this.colors = ['red']
}

SuperType.prototype.sayname = function() {
    alter(this.name)
}

function SubType(name, age) {
    SuperType.call(this)
    this.age = age
}

inheritPrototype(SubType, SuperType)

SubType.prototype.sayage = function() {
    alter(this.age)
}
```

这个栗子地高效率体现在它只调用一次 SuperType 构造函数，并且因此避免了在 SubType.prototype 上面创建不必要的、多余的属性。与此同时，原型链还能保持不变；因此，还能够正常地使用 instanceof 操作符和 isPrototypeOf() 方法