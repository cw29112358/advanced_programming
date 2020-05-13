### BOM

1. window 对象

BOM 的核心对象是 window，它表示浏览器的一个实例。在浏览器中， window 对象有双重角色，它既是通过 JS 访问浏览器窗口的一个接口，又是 ECMAScript 规定的 Global 对象。这意味着在网页中定义的任何一个对象、变量和函数，都以 window 作为其 Global 对象。

弹出窗口属性参考 p200

+ 全局作用域

所有在全局作用域中声明的变量、函数都会成为 window 对象的属性和方法

> 注意：全局变量不能通过 delete 操作符删除，而直接在 window 对象上定义的属性可以。尝试未声明的变量会抛出错误，但是通过查询 window 对象，可以知道某个可能未声明的变量是否存在。

+ 窗口关系及框架

如果页面中包含框架，则每个框架都拥有自己的 window 对象，并且保存在 frames 集合中。在 frames 集合中，可以通过数值索引或者框架名称来访问相应的 window 对象。每个 window 对象都有一个 name 属性，其中包含框架的名称。

top 对象始终指向最高（最外层的框架），也就是浏览器窗口。使用它可以确保在一个框架中正确地访问另一个框架。因为对于在一个框架中编写地任何代码来说，其中地 window 对象指向地都是那个框架地特定实例，而非最外层框架。

与 top 相对的另一个 window 对象是 parent。顾名思义，parent 对象始终指向当前框架地直接上层框架。在某些情况下，parent 有可能等于 top；但在没有框架地情况，parent 一定等于 top(此时它们都等于 window)

> 注意：除非最高层窗口是通过 window.open() 打开的，否则其 window 对象地 name 属性不会包含任何值。

与框架有关的最后一个对象是 self，它始终指向 window；self 和 window 可以互换使用，引入 self 对象的目的只是为了与 top 和 parent 对象对应起来，因此，他不格外包含其他值。

所有这些对象都是 window 对象的属性，可以通过 window.parent 和 window.top 等形式来访问。同时，这也意味着可以将不同层次的 window 对象连缀起来，如：window.parent.parent.frames[0]

+ 窗口位置

```
var leftPos = (typeof window.screenLeft == 'number') ? window.screenLeft : window.screenX

var topPos = (typeof window.screenTop == 'number') ? window.screenTop : window.screenY
```

以上代码运用二元操作符首先确定 screenLeft 和 screenTop 是否存在，如果是，则取得这两个属性的值，如果不是，则取得 screenX 和 screenY 这两个属性的值。

注意：无法在跨浏览器的添加下取得窗口左边和上边的精确坐标值。然后，使用 moveTo() 和 moveBy() 方法倒是有可能将窗口有精确地移动到一个新的位置。这两个方法都接收两个参数，其中 moveTo() 是接收新位置地 x 和 y 坐标值，而 moveBy() 接收的是在水平和垂直方向上移动的像素数。

```
// 将窗口移动到屏幕左上角
window.moveTo(0, 0)

// 将窗口向下移动100像素
window.moveBy(0, 100)
```

这两个方法可能会被浏览器禁用，且这两个方法都不适用与框架，只能对最外层的 window 对象使用。

+ 窗口大小

innerWidth、innerHeight、outerWidth、outerHeight

注意，在不同浏览器中，上面的属性返回的结果不同，详细参考 p198

```
var pageWidth = window.innerWidth
var pageHeight = window.innerHeight

if (typeof pageWidth != 'number') {
  if (document.compatMode == 'CSS1Compat') {
    pageWidth = document.documentElement.clientWidth
    pageHeight = document.documentElement.clientHeight
  } else {
    pageWidth = document.body.clientWidth
    pageHeight = document.body.clientHeight
  }
}
```

另外，使用 resizeTo() 和 resizeBy() 方法可以调整浏览器窗口的大小。这两个方法都接收两个参数，其中 resizeTo() 接收浏览器窗口的新宽度和新高度，resizeBy() 接收新窗口和原窗口的宽度和高度之差。

```
// 调整到 100 X 100
resizeTo(100, 100)

// 调整到 200 X 150
resizeBy(100, 50)
```

注意，这两个方法也有可能被禁用，且这两个方法同样不适用框架，而只能对最外层的 window 对象使用。

+ 导航和打开窗口

使用 window.open() 方法既可以导航到一个特定的 URL，也可以打开一个新的浏览器窗口。这个方法接收4个参数：要加载的URL、窗口目标、一个特性字符串以及一个表示新页面是否取代浏览器记录中当前加载页面的布尔值。

window.open() 方法返回一个指向新窗口的引用，引用的对象与其他新窗口的对象大致相似。

调用 window.close() 方法还可以关闭新打开的窗口，但是这个方法只适用于通过 window.open() 打开的弹出窗口。对于浏览器的主窗口，如果没有得到用户的允许是不能关闭它的。弹出的窗口关闭之后，窗口的引用还在，但除了检测其 closed 属性之外，已经没有其他用处了

新创建的 window 对象有一个 opener 属性，其中包含着打开它的原始窗口对象。这个属性只在弹出窗口中的最外层 window 对象（top）中有定义，而且指向调用 window.open() 的窗口或框架。

有些浏览器会在独立的进程中运行每个标签页。当一个标签页打开另一个标签页时，如果两个 window 对象之间需要彼此通信，那么新标签就不能运行在独立的进程中。在 Chrome 中，将新创建的标签页的 opener 属性设置为 null，即表示在单独的进程中运行新标签页。将 opener 属性设置为 null 就是告诉浏览器新创建的标签页不需要与打开它的标签页进性通信，因此可以在独立的进程中运行。标签页之间的联系一旦切断，将没有办法恢复。

检查弹出窗口是否被屏蔽的方法：

```
var blocked = false

try {
  var worxWin = window.open('http://www.wrox.com')

  if (worxWin == null) {
    blocked = true
  }
} catch (ex) {
  blocked = true
}

if (blocked) {
  alter('the popup was blocked')
}
```

+ 超时调用和间歇调用

setTimeout(),setInterval()

+ 系统对话框

```
// 警告框
window.alter()

// 确认框
window.confirm()

// 提示框
window.prompt()

// 打印对话框
window.print()

// 查找对话框
window.find()
```

2. location 对象：提供了与当前窗口中加载的文档有关的信息，还提供了一些导航的功能。

location 对象既是 window 对象的属性，也是 document 对象的属性。也就是说，window.location 和 document.location 引用的是同一个对象。location 对象的用处不仅表现在它保存着当前文档的信息，还表现在将 URL 解析为独立的片段，让开发人员可以通过访问不同的属性访问这些片段。

属性参考 p207

+ 查询字符串参数

```
function getQueryStringArgs() {
  var qs = location.search.length > 0 ? location.substring(1) : '',
      args = {},
      items = qs.length ? qs.split('&) : [],
      item = null,
      name = null,
      value = null,
      i = 0,
      len = items.length

  for (i = 0; i < len; i++) {
    item = items[i].split('=')
    name = decodeURIComponent(item[0])
    value = decodeURIComponent(item[1])

    if (name.length) {
      args[name] = value
    }
  }

  return args
}
```

+ 位置操作

如果将 window.location、location.href 设置为一个 URL 的值，会以该值调用 assign 方法。

```
location.assign('http://www.worx.com')
window.location = 'http://www.worx.com'
location.href = 'http://www.worx.com'
```

另外，修改 location 的其他属性也可以改变当前加载的页面。如：hash、search、hostname、pathname、port...

location.replace() 方法：接收一个参数，即要导航到的 URL；结果虽然会导致浏览器位置改变，但不会在历史记录中生成新纪录。

location.reload()方法：重新加载当前显示页面。

```
location.reload() // 有可能从缓存中重新加载
location.reload(true) // 有可能从服务器中重新加载
```

3. navigator 对象

navigator 对象：识别客户端浏览器的事实标准

属性参考 p210

+ 检测插件

```非 IE 浏览器
function hasPlugin(name) {
  name = name.toLowerCase()

  for (var i = 0; i < navigator.plugins.length; i++) {
    if (navigator.plugins[i].name.toLowerCase().indexOf(name) > -1) {
      return true
    }
    return false
  }
}
```

```IE 浏览器
function hasIEPlugin(name) {
  try {
    new ActiveXObject(name)
    return true
  } catch (ex) {
    return false
  }
}

hasIEPlugin('ShockwaveFlash.ShockwaveFlash')
hasIEPlugin('QuickTime.QuickTime')
```

```检测所有浏览器中的 Flash
function hasFlash() {
  var result = hasPlugin('Flash')

  if (!result) {
    result = hasIEPlugin('ShockwaveFlash.ShockwaveFlash')
  }

  return result
}
```

+ 注册处理程序

详细参考 p213

4. screen 对象

screen：表明客户端的能力，其中包括浏览器窗口外部的显示器的信息，如像素宽度和高度等。

属性参考 p214

5. history 对象

history 对象保存着用户上网的记录，从窗口被打开的那一刻算起。因为 history 是 window 对象的属性，因此，每个浏览器窗口、每个标签乃至每个框架，都有自己的 history 对象与特定的 window 对象关联。

使用 go() 方法可以在用户的历史记录中任意跳转

```
// 后退一页
history.go(-1)

// 前进一页
history.go(1)

// 前进两页
history.go(2)

// 跳转到最近的 wrox.com 页面
history.go('wrox.com')
```

另外，还有两个方法：back() 和 forward()，模仿了后退和前进按钮

```
histpry.back()

history.forward()
```

history 还有一个 length 属性，保存着历史记录的数量。这个数量包括所有历史记录，即所有向后和向前的记录。对于加载到窗口、标签页或者框架中的第一个页面而言，history.length 等于 0。通过这个方法可以检测用户是否一开始就打开了你的页面。

```
if (history.length == 0) {
  // 这应该是用户打开窗口后的第一个页面
}
```