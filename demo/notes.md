# 简书的复刻
### Day 1
> 首页导航栏`nav`的搭建。

1. 个人比较熟悉的一种导航框架：
```
     <div>
        <a>
            <img>
        </a>
        <ul>
            <li>
                <img>
                <a></a>
            </li>
            <li>
                <a></a>
            </li>
            <li>
                <form>
                    <input type="text">
                </form>
            </li>
            <li>
                <a href="#"></a>
            </li>
            <li>
                <a href="#"></a>
            </li>
            ...
        </ul>       
```
然后设置ul的display为flex，可以将所有li横向排列，这样在li里设置html标签就能铺展导航栏。

2. 收获到的知识点：
+ `positio`的`absolute`属性,是相对于父元素定位，且该父元素的`position`属性不是`static`(默认值)。

> 故为了使`input`能够居于父元素`form`中间，消除上边距影响；给`input`一个`absolute`的`position`,同时form也使用`position：absolute`。

### DAy 2
> 轮播图的制作。

search过的知识点：
+ `HTML DOM setInterval()`方法
+ `JavaScript parseInt()` 函数
+ ` onmousemove, onmouseenter 和 onmouseover` 事件
[demo讲解三者区别](http://www.runoob.com/try/try.php?filename=tryjsref_onmousemove_over_enter "RUNOOB.COM")

> 轮播图的实现
1. `js`代码注释已写在`app.js`。 
2. 遇到的问题：
  + 最初`.tab`写在`.rollpic`外部，鼠标悬停事件在底层`button`上作用不到，便将其写入内部，更改CSS样式
  + 在底层`.tab`上选择序列后移走鼠标，会有图片播放错乱出现。于是在`.tab`鼠标悬浮事件的函数末尾加入当前索引赋值给滚动播放做记录的`flag`。

### Day 3
**Section 1.**
> 主页面的完善

**遇到的问题 & 学到的新知识：**
+ `a.degree:hover .append{}`的`CSS`样式不起作用。
**原因：**`.append`是`.degree`的兄弟元素，上面写法是父子选择器。
**解决：**`a.degree:hover+.append{}`[‘+’是相邻选择器]。
+ 一个块区的内部元素的定位：
**套路：**可以设置其父元素`position:relative`CSS样式，再给自己设置`position:absolute`。
**原理：**`absolute`是相对于`position`属性值不为`static`（默认值）的父元素定位。
+ 导航栏随浏览窗口变小的布局错乱：
**原因：**固定宽度+百分比宽度会导致页面过宽引发换行。
**解决：**将`width、margin、padding`都设置为百分比。
+ CSS属性声明顺序。
推荐的样式编写顺序：
  1. Positioning
  2. Box model
  3. Typographic
  4. Visual
  > 举个栗子：
  ```
  .declaration-order {
  /* Positioning */
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;

  /* Box-model */
  display: block;
  float: right;
  width: 100px;
  height: 100px;

  /* Typography */
  font: normal 13px "Helvetica Neue", sans-serif;
  line-height: 1.5;
  color: #333;
  text-align: center;

  /* Visual */
  background-color: #f5f5f5;
  border: 1px solid #e5e5e5;
  border-radius: 3px;

  /* Misc */
  opacity: 1;
  }
  ```
+ 变量声明符`let`和`var`的区别：
  > 1. `let`和`var`的区别体现在作用域上。`var`的作用域被规定为一个函数作用域，而`le`t则被规定为块作用域，块作用域要比函数作用域小一些，但是如果两者既没在函数中，也没在块作用域中定义，那么两者都属于全局作用域。
  > 2. **全局作用域**: 被`let`声明的变量不会作为全局对象`window`的属性，而被`var`声明的变量却可以。
  > 3. **函数作用域**: `var` 和 `let` 在函数作用域中声明一个变量，两个变量的意义是相同的。
  > 4. **块作用域**: [栗子] `let`只在`for()`循环中可用，而 `var`是对于包围`for()`循环的整个函数可用。
  > 5. **重新声明**：`var`允许在同一作用域中声明同名的变量，而`let`不可以。
  > 6. **总结**：为了降低变量污染的风险，在块作用域中使用`let`来代替`var`，这样不会污染块作用域的外部作用域，降低 `bug`率，使代码更安全。
+ **闭包**：
  > 嵌套的函数可以访问在其外部声明的变量。
  > `JavaScript`中的函数会形成闭包。 闭包是由函数以及创建该函数的词法环境组合而成。这个环境包含了这个闭包创建时所能访问的所有局部变量。

  栗子1：
  ```
  function makeFunc() {
    var name = "Mozilla";
    function displayName() {
        alert(name);
    }
    return displayName;
  }

  var myFunc = makeFunc();
  myFunc();
  ```
  > 当` myFunc `被调用时，`name `仍可被访问，其值` Mozilla `就被传递到`alert`中。

  栗子2：
  ```
  function makeAdder(x) {
  return function(y) {
    return x + y;
  };
  }

  var add5 = makeAdder(5);
  var add10 = makeAdder(10);

  console.log(add5(2));  // 7
  console.log(add10(2)); // 12
  ```
  > 定义了` makeAdder(x) `函数，它接受一个参数` x `，并返回一个新的函数。返回的函数接受一个参数` y`，并返回`x+y`的值。从本质上讲，`makeAdder `是一个函数工厂 — 他创建了将指定的值和它的参数相加求和的函数。`add5 `和` add10` 都是闭包。

  栗子3：(在循环中创建闭包：一个常见错误)
  ```
  function showHelp(help) {
    document.getElementById('help').innerHTML = help;
  }

  function setupHelp() {
    var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

    for (var i = 0; i < helpText.length; i++) {
      var item = helpText[i];
      document.getElementById(item.id).onfocus = function() {
        showHelp(item.help);
      }
    }
  }

  setupHelp();
  ```
  > 赋值给 onfocus 的是闭包。这些闭包是由他们的函数定义和在 setupHelp 作用域中捕获的环境所组成的。这三个闭包在循环中被创建，但他们共享了同一个词法作用域，在这个作用域中存在一个变量item。当onfocus的回调执行时，item.help的值被决定。由于循环在事件触发之前早已执行完毕，变量对象item（被三个闭包所共享）已经指向了helpText的最后一项。

  改进：
  1. 使用了匿名闭包
  ```
  function showHelp(help) {
    document.getElementById('help').innerHTML = help;
  }

  function setupHelp() {
    var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

    for (var i = 0; i < helpText.length; i++) {
      (function() {
        var item = helpText[i];
        document.getElementById(item.id).onfocus = function() {
          showHelp(item.help);
        }
      })(); // 马上把当前循环项的item与事件回调相关联起来
    }
  }

  setupHelp();
  ```
  2. 避免使用过多的闭包，可以用let关键词：
  ```
  function showHelp(help) {
    document.getElementById('help').innerHTML = help;
  }

  function setupHelp() {
    var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

    for (var i = 0; i < helpText.length; i++) {
      let item = helpText[i];
      document.getElementById(item.id).onfocus = function() {
        showHelp(item.help);
      }
    }
  }

  setupHelp();
  ```

**Section 2.**
> 瀑布流的学习

+ 查阅的知识：
  + `document.creatElement`
  + `appendChild()`

### Day 4
> 响应式样式设计

遇到的问题：
+ 宽度的不改变
**原因：**在总体样式`style.css`里写了`width`后再在`@media`里写就不会覆盖而改变
**解决：**`style.css`不设置`width`，在`@media`处设置不同`width`
> 今天在举办班级活动，外加写明天要交的作业，不同设备的适配又是第一次接触，就...总之明天加油吧！

### Day 5
>设备适应的完善（昨天是`section`今天是`nav`）

学到的知识：
+ 将元素两边的留白，设置为随页面宽度等比缩放是`margin:auto`或者`margin-left:auto`+`margin-right:auto`。这便要求该部分需要一个固定长度的父元素，该部分`width`百分比设置。
+ 最初写导航栏都是从左往右排列，通过开发者工具学习**简书**的导航栏，发现：
  + 居于导航栏右边的部分可以写在主部分`list`之前，然后利用`float:right`使之位于第一行右侧。
  + `float:right`和`float:left`相似，如果前一块元素使用了`float:right / float:left`，都是基于前一个 左 / 右 浮动。

> 夜间模式的编写

+ 问题：
  1. 悬浮框，比如那个二维码、夜间模式转换栏，下面的小三角是怎么实现的？怎样保证border的重合线没有颜色？
  2. 我的夜间模式样式使用`JS`实现，感觉`JS`写`CSS`样式有点麻烦,所以样式没有全部改完。有没有别的好的优雅点的办法？
  3. 关于夜间模式转换的那个悬浮框，怎样实现点击`Aa`显示，点击页面其他地方就隐藏？

> 登录页面的编写

+ 未实现切换和功能。

> 骨架屏的学习

+ 骨架屏的优势：
  在动态、文章加载时预先渲染出结构布局，数据加载完成后再填充数据显示，这样的好处在于不干扰用户操作，使用户对于加载的内容有一个大致的预期，特别是弱网络环境下极大的优化了用户体验。
+ Question：
  1. 怎样在数据加载时显示骨架屏，在加载完成时显示文本？
  2. 找到的关于瀑布流的资料都是写图片加载的，一想到怎么实现数据传输，就又和上面的问题重复了；
  3. 总之明天学学`node.js`和`mongoDB`之类的吧！(但愿学得完...)

### Day 6

> 瀑布流加载的编写

+ 学习的内容：
  1. `Ajax:Asynchronous JavaScript and XML`（异步的` JavaScript `和` XML`）
    + `AJAX` 不是新的编程语言，而是一种使用现有标准的新方法。最大的优点是在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容。
    + `XMLHttpRequest` 对象: `XMLHttpRequest` 用于在后台与服务器交换数据。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。
      +  `open()` 和 `send()` 方法:
      ```
      open(method,url,async) => { method：请求的类型；GET 或 POST | url：文件在服务器上的位置 | async：true（异步）或 false（同步）}
      send(string) => {将请求发送到服务器。string：仅用于 POST 请求}
      ```
  2. `Node.js`
    + `node.js`跨域设置
    + `node.js`输出`json`
  3. 编写过程：
    + 先是建了一个文件夹，在里面尝试`Node、Ajax`,然后调配`test.html`,让内容能够写入，瀑布流能够实现，最后将结果导入`demo`的`html`。 

### Day 7

> 瀑布流图片的添加

问题与解决：
+ 给`creatElement`出的`img`设置`src`属性，用到`setAttribute()`方法。
  + 语法：
  ```
  element.setAttribute(attributename,attributevalue) (属性名称，属性值)
  ```
+ 一开始引入的`src`在`node.js`文件里`json`的键`img`的值写的是`//upload-images.jianshu.io/upload_images/blabla..`
  通过在简书官网中用使用开发者工具调出图片的`src`再复制粘贴到网页搜索栏上，发现：
  + `//upload-images.jianshu.io/upload_images/blabla..`搜索的是文件协议。
  + 官网里的图片调去搜索的是`http`协议。
于是键`img`的值改为`http://upload-images.jianshu.io/upload_images/blabla..`

> 引入瀑布流加载后样式的调试

+ `querySelector()`方法改为`getElementsByTagName()`方法。

+ 明天还有班级春游...早起写写个人主页吧 —— ——2019.3.24 1:10

啊...个人主页做了个半成品，要去春游没时间了，555

### 写在最后
> 通过这一个星期的实习，我真的学了很多东西。