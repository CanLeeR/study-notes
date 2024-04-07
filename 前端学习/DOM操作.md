> 博主 CanLee 带您学 IT.
> ✍ 欢迎参观 [个人Blog网站](http://www.canlee.top)，更多相关Blog等你来探索
>
> 涉及到：前端，Java，Git 等领域
>
> 未来main分支：更新我的**前端/全栈开发学习历程**
>
> 未来feat分支：更新带您快速回顾之**数据库**，**数据结构**，**计算机网络**，**操作系统**等计算机知识
>
> 🍩惟余辈才疏学浅，临摹之作或有不妥之处，还请读者海涵指正。☕🍭
> 🪁 吾期望此文有资助于尔，即使粗浅难及深广，亦备添少许微薄之助。苟未尽善尽美，敬请批评指正，以资改进。！💻⌨

PS：本文大量参考MDN官方文档  戳这里 >> [Blog链接](http://www.canlee.top/#/article/16)

# DOM操作

​		DOM操作就是对DOM**节点**的增删改查

> 常用的DOM节点操作
>
> 查询：querySelector，querySelectorAll，getElementById
>
> 创建&插入：createElement，appendChild
>
> 删除：remove，removeChild
>
> 替换：replaceChild

### **查询**

**选择器**（selectors）：包含一个或多个要匹配的选择器的 DOM 字符串[`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)。该字符串必须是有效的 CSS 选择器字符串；如果不是，则引发`SYNTAX_ERR`异常。

**querySelector**：文档对象模型[`Document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document)引用的 **`querySelector()`** 方法返回文档中与指定选择器或选择器组匹配的第一个 [`Element`](https://developer.mozilla.org/zh-CN/docs/Web/API/Element)对象。如果找不到匹配项，则返回`null`。

**备注：** 匹配是使用深度优先先序遍历，从文档标记中的第一个元素开始，并按子节点的顺序依次遍历。

```js
element = document.querySelector(selectors);
```

---

**querySelectorAll**：返回与指定的选择器组匹配的文档中的元素列表 (使用深度优先的先序遍历文档的节点)。返回的对象是 [`NodeList`](https://developer.mozilla.org/zh-CN/docs/Web/API/NodeList) 。也就是

**备注**：一旦返回匹配元素的[`NodeList`](https://developer.mozilla.org/zh-CN/docs/Web/API/NodeList)，就可以像任何数组一样检查它。如果数组为空（即，其`length`属性为 0），则找不到匹配项。

否则，你只需使用标准数组方法来访问列表的内容。你可以使用任何常见的循环语句，也就是可以forEach操作，index获取等

**意外结果**：

```html
<div class="outer">
  <div class="select">
    <div class="inner">
    </div>
  </div>
</div>
```

```js
var select = document.querySelector(".select");
var inner = select.querySelectorAll(".outer .inner");
inner.length; // 1, not 0!
```

​		在这个例子中，当在`<div>`上下文中选择带有`"select"`类的`".outer .inner"`时，仍然会找到类`".inner"`的元素，即使`.outer`不是基类的后代 执行搜索的元素（`".select"`）。默认情况下，`querySelectorAll()`仅验证选择器中的最后一个元素是否在搜索范围内。

这时可以借助[`:scope`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:scope) 伪类符合预期的行为，只匹配基本元素后代的选择器：

```js
var select = document.querySelector(".select");
var inner = select.querySelectorAll(":scope .outer .inner");
inner.length; // 0
```

可以理解为:scope 限定了作用域，所以返回为0

---

**getElementById**：

**返回值**：返回一个表示与指定 ID 相匹配的 DOM 元素的 [`Element`](https://developer.mozilla.org/zh-CN/docs/Web/API/Element) 对象。若在当前文档中没有找到匹配的元素，则返回 `null`。

**备注**：由于是使用Id查询， 文档中的 ID 必须是唯一的。如果一个文档中真的有两个及以上的元素具有相同的 ID，那么该方法只会返回查找到的第一个元素。

 **值得注意的点**：

不同于其他元素查找方法（如 [`Document.querySelector()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/querySelector) 以及 [`Document.querySelectorAll()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/querySelectorAll)），`getElementById()` 只有在作为**全局 `document`** 的方法时才能起作用，而在 **DOM 中的其他元素上*无法*生效**。这是因为 **ID 值在整个网页中必须保持唯**一。因此没有必要为这个方法创建所谓的“局部”版本。

```js
const parentDOM = document.getElementById("parent-id");
const test1 = parentDOM.getElementById("test1");
// 抛出错误
// Uncaught TypeError: parentDOM.getElementById is not a function
```

`getElementById()` 方法不会搜索不在文档中的元素。当创建一个元素，并且分配 ID 后，你必须使用 [`Node.insertBefore()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/insertBefore) 或其他类似的方法把元素插入到文档树中，之后才能使用 `getElementById()` 访问到：

```js
const element = document.createElement("div"); 
element.id = "testqq";//只创建不插入
const el = document.getElementById("testqq"); // el 会是 null！
```

对于非 HTML 文档，DOM 的实现必须说明哪个属性是 ID 类型。只有文档的 DTD 定义了这个属性名是“id”时，“id”才会被认为是 ID 类型。在 [XHTML](https://developer.mozilla.org/zh-CN/docs/Glossary/XHTML)、XUL 或者其他文档中，“id”通常被定义为 ID 类型的属性。不知道哪个属性是 ID 类型的实现中，这预期会返回 `null`。

### 创建&插入

在 [HTML](https://developer.mozilla.org/zh-CN/docs/Web/HTML) 文档中，**`Document.createElement()`** 方法用于创建一个由标签名称 *tagName* 指定的 HTML 元素。如果用户代理无法识别 *tagName*，则会生成一个未知 HTML 元素 [`HTMLUnknownElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLUnknownElement)。

**语法**

```js
var element = document.createElement(tagName[, options]);
```

**参数说明**

`tagName`指定要创建元素类型的字符串(如 "div")，创建元素时的 [`nodeName`](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/nodeName) 使用 `tagName` 的值为初始化，该方法不允许使用限定名称 (如："html:a")，在 HTML 文档上调用 `createElement()` 方法创建元素之前会将`tagName` 转化成小写，在 Firefox、Opera 和 Chrome 内核中，`createElement(null)` 等同于 `createElement("null")`

**返回值**：新创建的Element

```js
function addElement() {
  // 创建一个新的 div 元素
  let newDiv = document.createElement("div");
  // 给它一些内容
  let newContent = document.createTextNode("Hi there and greetings!");
  // 添加文本节点 到这个新的 div 元素
  newDiv.appendChild(newContent);

  // 将这个新的元素和它的文本添加到 DOM 中
  let currentDiv = document.getElementById("div1");
  document.body.insertBefore(newDiv, currentDiv);
}
```

PS：`createTextNode`: 创建文本节点返回，参数**data**传入的是一字符串

**appendChild**

**`Node.appendChild()`** 方法将一个节点附加到指定父节点的子节点列表的末尾处。如果将被插入的节点**已经存在**于当前文档的文档树中，那么 `appendChild()` 只会将它从**原先的位置移动到新的位置**（不需要事先移除要移动的节点）。

这意味着，**一个节点不可能同时出现在文档的不同位置**。所以，如果某个节点已经拥有父节点，在被传递给此方法后，它首先会被移除，再被插入到新的位置。若要保留已在文档中的节点，可以先使用 [`Node.cloneNode()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/cloneNode) 方法来为它创建一个副本，再将副本附加到目标父节点下。请注意，用 `cloneNode` 制作的副本不会自动保持同步。

如果给定的子节点是 [`DocumentFragment`](https://developer.mozilla.org/zh-CN/docs/Web/API/DocumentFragment)，那么 [`DocumentFragment`](https://developer.mozilla.org/zh-CN/docs/Web/API/DocumentFragment) 的全部内容将转移到指定父节点的子节点列表中

语法 ：

```js
element.appendChild(aChild)
```

参数：要追加给父节点（通常为一个元素）的节点

返回值：返回追加后的**子节点**（`aChild`），除非 `aChild` 是一个文档片段（[`DocumentFragment`](https://developer.mozilla.org/zh-CN/docs/Web/API/DocumentFragment)），这种情况下将返回空文档片段（[`DocumentFragment`](https://developer.mozilla.org/zh-CN/docs/Web/API/DocumentFragment)）。

附注：如果你需要保留这个子节点在原先位置的显示，则你需要先用[`Node.cloneNode`](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/cloneNode)方法复制出一个节点的副本，然后在插入到新位置。

这个方法只能将某个子节点插入到同一个文档的其他位置，如果你想跨文档插入，你需要先调用[`document.importNode`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/importNode)方法。

备注：由于 `appendChild()` 返回的是被附加的子元素，所以链式调用可能无法按照你的预期去执行：

```js
let aBlock = document
  .createElement("block")
  .appendChild(document.createElement("b"));
//只会将 aBlock 设置为 <b></b> ，这可能不是你所想要的。
```

示例：

```js
// 创建一个新的段落元素 <p>，然后添加到 <body> 的最尾部
var p = document.createElement("p");
document.body.appendChild(p);
```

**Node.insertBefore**

**`Node.insertBefore()`** 方法在参考节点之前插入一个拥有指定父节点的子节点。**如果给定的子节点是对文档中现有节点的引用，`insertBefore()` 会将其从当前位置移动到新位置**（在将节点附加到其他节点之前，不需要从其父节点删除该节点）。

这意味着一个节点不能同时位于文档的两个点中。因此，如果被插入节点已经有父节点，则首先删除该节点，然后将其插入到新位置。若要保留已在文档中的被插入节点，在将该节点追加到新父节点之前，可以使用 [`Node.cloneNode()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/cloneNode) 复制节点。注意，使用 `cloneNode()` 创建的节点副本不会自动与原始节点保持同步。

如果引用节点为 `null`，则将指定的节点添加到指定父节点的子节点列表的末尾。

如果给定的子节点是 [`DocumentFragment`](https://developer.mozilla.org/zh-CN/docs/Web/API/DocumentFragment)，那么 `DocumentFragment` 的全部内容将被移动到指定父节点的子节点列表中。

**语法**

```js
var insertedNode = parentNode.insertBefore(newNode, referenceNode);
```

- `insertedNode` 被插入节点 (newNode)
- `parentNode` 新插入节点的父节点
- `newNode` 用于插入的节点
- `referenceNode` `newNode` 将要插在这个节点之前
备注：** `referenceNode` 引用节点**不是**可选参数——你必须显式传入一个 `Node` 或者 `null`。如果不提供节点或者传入无效值，在不同的浏览器中会有[不同](https://bugzilla.mozilla.org/show_bug.cgi?id=119489)的[表现](https://code.google.com/p/chromium/issues/detail?id=419780)。

可以搭配firstChild来使用

​		元素没有首节点时，`firstChild` 返回 `null`。该元素仍然会被插入到父元素中，位于最后一个节点后面。又由于父元素没有第一个子节点，也没有最后一个子节点。最终，新元素成为唯一的子元素。

### 删除

**Element.remove()**//元素，而非节点，按理来说不算DOM操作

语法：

```js
node.remove();
```

**`Element.remove()`** 方法，把对象从它所属的 DOM 树中删除。‘自杀’

**Node.removeChild**

语法：

```js
let oldChild = node.removeChild(child);
//OR
element.removeChild(child);
```

- `child` 是要移除的那个子节点。
- `node` 是`child`的父节点。
- oldChild 保存对删除的子节点的引用。`oldChild` === `child`.

被移除的这个子节点**仍然存在于内存中**，只是**没有添加到当前文档的 DOM 树中**，因此，你还可以把这个节点重新添加回文档中，当然，实现要用另外一个变量比如`上例中的 oldChild`来保存这个节点的引用。如果使用上述语法中的第二种方法，即没有使用 oldChild 来保存对这个节点的引用，则认为被移除的节点已经是无用的，在短时间内将会被[内存管理](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Memory_management)回收。

如果上例中的`child 节点`不是`node`节点的子节点，则该方法会**抛出异常**。

### 替换

**Node.replaceChild**

语法：

```js
replaceChild(newChild, oldChild)
```

参数：

`newChild`用来替换 `oldChild` 的新节点。如果该节点已经存在于 DOM 树中，则它首先会被从原始位置删除。 

`oldChild`被替换掉的原始节点。

返回值：返回值为被替换的[节点](https://developer.mozilla.org/zh-CN/docs/Web/API/Node)，与 `oldChild` 为同一个节点。

```js
// <div>
//   <span id="childSpan">foo bar</span>
// </div>

// 创建一个空的 span 元素节点
// 没有 id，没有任何属性和内容
const sp1 = document.createElement("span");

// 添加一个 id 属性，值为 'newSpan'
sp1.setAttribute("id", "newSpan");

// 创建一个文本节点
const sp1_content = document.createTextNode("新的 span 元素的内容。");

// 将文本节点插入到 span 元素中
sp1.appendChild(sp1_content);

// 获得被替换节点和其父节点的引用。
const sp2 = document.getElementById("childSpan");
const parentDiv = sp2.parentNode;

// 用新的 span 元素 sp1 来替换掉 sp2
parentDiv.replaceChild(sp1, sp2);

// 结果：
// <div>
//   <span id="newSpan">新的 span 元素的内容。</span>
// </div>
```

