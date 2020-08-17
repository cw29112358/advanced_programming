## DOM

DOM 是针对 HTML 和 XML 文档的一个 API（应用程序编程接口）

### 节点层次

文档节点是每个文档的根节点。

1. Node 类型

nodeType 属性，用于表明节点的类型。

nodeName 和 nodeValue 属性，保存着节点的具体信息。

childNodes 属性，保存着一个 NodeList 对象。NodeList 对象是一种类数组对象，用于保存一组有序的节点，可以通过方括号或者 item 语法来访问这些节点，这个对象也有 length 属性。但它不是 Array 的实例。可以通过 Array.prototype.slice() 方法来转换成数组。

```
function convertToArray(nodes) {
  var array = null
  try {
    array = Array.prototype.slice.call(nodes, 0)
  } catch (ex) {
    array = new Array()

    for (var i = 0, len = nodes.length; i < len; i++) {
      array.push(nodes[i])
    }
  }
  return array
}
```

parentNode 属性，保存着文档数中该节点的父节点

previousSibling、nextSibling 属性，保存着文档树中该节点的同胞节点。

firstChild、lastChild 属性，保存着文档树中该节点的第一个子节点和最后一个子节点。

hasChildNodes() 方法，该方法在节点包含一个或多个子节点的情况下返回 true。

ownerDocument 属性，保存着表示整个文档的文档节点。

appendChild() 方法，向 childNodes 列表的末尾添加一个节点，返回新增的节点。如果传入的节点已经是文档的一部分了，那么结果是将该节点从原来的位置转移到新的位置。

insertBefore() 方法，把节点放在 childNodes 列表中某个特定的位置。接收两个参数：要插入的节点和作为参照的节点。返回插入的节点。如果参照节点是 null，则 insertBefore() 和 appendChild() 执行相同的操作。

replaceChild() 方法，接收两个参数：要插入的节点和要替换的节点。要替换的节点将由这个方法返回并从文档树中移除，同时由要插入的节点占据其位置。

removeChild() 方法，移除某个节点。接收一个参数：要移除的节点。

cloneNode() 方法，创建调用这个方法的节点的一个完全相同的副本。接收一个布尔值参数，表示是否执行深复制。参数为 true，复制节点及其整个子节点数；参数为 false，只复制节点本身，复制后返回的节点文本属于文档所有，但并没有为它指定父节点。

normallise() 方法，处理文档树中的文本节点。详细参考 p272

2. Document 类型

表示文档。在浏览器中，document 对象是 HTMLDocument 的一个实例，表示整个 HTML 页面。而且 document 对象是 window 对象的一个属性，因此可以将其作为一个全局对象来访问。

+ 文档的子节点

Document 的子节点可以是 DocumentType、Element、Comment 和 ProcessingInstruction。

documentElement 属性，始终指向 HTML 页面中的 \<html> 元素。

childNodes 属性，访问文档列表元素

body 属性，指向 \<body> 元素

doctype 属性，指向 \<!DOCTYPE> 元素。但是浏览器对改属性的支持不一致。

+ 文档信息

title 属性，保存这 \<title> 元素中的文本。

URL 属性，包含页面完整的 URL(即地址栏中显示的 URL)

domain 属性，包含着页面的域名。由于安全方面的限制，不能将这个属性设置为 URL 中不包含的域。当页面中包含来自其他子域的框架和内嵌框架时，由于跨域安全限制，来自不同子域的页面无法通过 JavaScript 通信，通过将每个页面的 document.domain 设置为相同的值，这些页面就可以互相访问对方包含的 JS 对象了。还有一个限制：即域名如果一开始为‘松散的’，那么不能将它设置为‘紧绷的’。

referrer 属性，保存着链接到当前页面的那个页面的 URL。在没有页面来源的情况下，referrer 属性中可以包含空字符串。

+ 查找元素

getElementById() 方法，接收一个参数：要取得的元素的 ID。如果找到相应的元素，则返回该元素，如果不存在带有相应 ID 的元素，则返回 null。如果文档中多个元素的 ID 值相同，则只会返回文档中第一次出现的元素。

getElementsByTagName() 方法，接收一个参数：要取得的元素的标签名，返回的是包含零或多个元素的 nodeList。这个 nodeList 对象还有一个特殊的方法：namedItem()

```
<img src='img.png' name='myImage'>

var images = document.getElementsByTagName('img')

var myImage = images.namedItem('myImage')
// or
var myImage = images['myImage'] // 调用 namedItem() 方法
// or
var myImage = images[0] // 调用 item() 方法
```

```
// 取得文档中的所有元素
var allElements = document.getElementsByTagName(*)
```

getElementsByName() 方法，返回带有给定 name 特性的所有元素

+ 特殊集合

document.anchors(): 包含文档中所有带 name 特性的 \<a> 元素

document.applets(): 包含文档中所有 \<applet> 元素

document.forms(): 包含文档中所有 \<form> 元素

document.images(): 包含文档中所有 \<img> 元素

document.links(): 包含文档中所有带 href 特性的 \<a> 元素

+ DOM 一致性检测

详细参考 p259

+ 文档写入



3. Element 类型

4. Text 类型

5. Comment 类型

6. CDATASection 类型

7. DocumentType 类型

8. DocumentFragment 类型

9. Attr 类型

### DOM 操作技术

1. 动态脚本

2. 动态样式

3. 操作表格

4. 使用 NodeList