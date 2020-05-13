## 客户端检测

### 能力检测

能力检测又称特性检测：目标不是识别特定的浏览器，而是识别浏览器的能力。

```
function getElement(id) {
  if (document.getElementById) {
    document.getElementById(id)
  } else if (document.all) {
    document.all[id]
  } else {
    throw new Error('No way to retrieve element')
  }
}
```

注意点：

+ 首先检测达成目的的最常用特性，这样可以保证代码最优化，因为大多数形况下都可以避免测试多个条件

+ 必须测试实际用到的特性。一个特性存在，不一定意味着另一个特性存在

1. 更可靠的能力检测

详细参考 p218

2. 能力检测，不是浏览器检测

检测某个或某几个特性并不能够确定浏览器。

详细参考 p220

### 怪癖检测

目的是识别浏览器的特殊行为。确定浏览器存在什么缺陷，这通常需要运行一小段代码，以确定某一特性不能正常工作。

### 用户代理检测

通过检测用户代理字符串来确定实际使用的浏览器。

详细参考 p221-p246