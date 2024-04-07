# JavaScript模版字符串（ES6）

**优点**：允许在字符串中嵌入表达式和变量

**用法**：`` 反引号来嵌套

> 允许
>
> 多行字符串 （无需转义但是有限制见下，最终会到一行来）
>
> ```
> `string text line 1
>  string text line 2`
> ```
>
> 带嵌入表达式的字符串
>
> ```
> `string text ${expression} string text`
> ```
>
> 带标签的模板的特殊结构
>
> ```
> tagFunction`string text ${expression} string text`
> ```

### 需要理解：

- **string text**：将成为模板字面量的一部分的字符串文本。几乎允许所有字符，包括换行符和其他空白字符。但是，除非使用了标签函数，否则无效的转义序列将导致语法错误。
- **expression**：要插入当前位置的表达式，其值被转换为字符串或传递给 tagFunction。
- **tagFunction**：如果指定，将使用模板字符串数组和替换表达式调用它，返回值将成为模板字面量的值

### 值得注意：

若要转义模板字面量中的反引号（**`**），需在反引号之前加一个反斜杠（**\**）。

```
`\`` === "`"; // true
```

### 举例说明

> 插值可以用变量名
>
> const name = 'CanLee';
> const age = 30;
> const message = `My name is ${name} and I'm ${age} years old.`;

> 插值可以进行简单计算
>
> let price = 10;
> let VAT = 0.25;
>
> let total = `Total: ${(price * (1 + VAT)).toFixed(2)}`;

可以  String模版-->HTML模版

```js
let tags = ["RUNOOB", "GOOGLE", "TAOBAO"];

let html = `<h2>${header}</h2><ul>`;
for (const x of tags) {
 html += `<li>${x}</li>`;
}
html += `</ul>`;
```

### 感受

​	字符串模版挺好用的，特别是插值字符串，简化了字符串拼接，类似于C语言输出一样





















