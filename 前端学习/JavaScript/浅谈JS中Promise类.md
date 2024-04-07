# JS中Promise类

## Why And  What？

#### what is Promise

> Promise 是一个 ECMAScript 6 提供的类，下面的解释将会让你明白为什么叫做Promise，
>
> 在 JavaScript 中，Promise 表示一个异步操作的承诺，即在未来某个时刻会产生一个结果。这种结果可能是异步操作成功完成时产生的值，也可能是异步操作失败时产生的错误。因此，Promise 可以被理解为对于异步操作结果的一种承诺，即在未来的某个时间点，会提供操作的结果。然后提供then，catch ，finally 回调函数来完闭 "承诺"

#### Why Promise

> 目的是更加优雅地书写复杂的异步任务。

ps：值得注意的是，由于 Promise 是 ES6 新增加的，所以一些旧的浏览器并不支持，苹果的 Safari 10 和 Windows 的 Edge 14 版本以上浏览器才开始支持 ES6 特性。但现在基本上都支持了。

## How？ 

#### how to  构造 Promise

```js
new Promise(function (resolve, reject) {
    // 要做的事情...
});
```

​			通过新建一个 Promise 对象好像并没有看出它怎样 "更加优雅地书写复杂的异步任务"。我们之前遇到的异步任务都是一次异步，如果需要多次调用异步函数呢？例如，如果我想分三次输出字符串，第一次间隔 1 秒，第二次间隔 4 秒，第三次间隔 3 秒：

```js
setTimeout(function () {
    console.log("First");
    setTimeout(function () {
        console.log("Second");
        setTimeout(function () {
            console.log("Third");
        }, 3000);
    }, 4000);
}, 1000);
```

​	这段程序实现了这个功能，但是它是用 "函数瀑布" 来实现的。可想而知，在一个复杂的程序当中，用 "函数瀑布" 实现的程序无论是维护还是异常处理都是一件特别繁琐的事情，而且会让缩进格式变得非常冗赘。

#### how to simplify it

现在我们用 Promise 来实现同样的功能：

```js
new Promise(function (resolve, reject) {
    setTimeout(function () {
        console.log("First");
        resolve();
    }, 1000);
}).then(function () {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            console.log("Second");
            resolve();
        }, 4000);
    });
}).then(function () {
    setTimeout(function () {
        console.log("Third");
    }, 3000);
});
```

使用Promise后有没有一下变得很优雅！

我们不难发现，代码从原来的嵌套变成了链式（顺序）的了

下面我们来细讲上述代码的关键 -- **Promise**()  构造函数

## Promise 的构造函数

#### You need  to know

> 回调函数（Callback Function）是指在编程中作为参数传递给其他函数的函数。这样的函数可以在其他函数执行完特定任务后被调用，以执行特定的行为或逻辑。回调函数通常用于异步编程中，例如事件处理、异步I/O操作等，以便在某些操作完成后执行相应的逻辑。

- Promise 构造函数是 JavaScript 中用于创建 Promise 对象的内置构造函数。

- Promise 构造函数接受一个**函数作为参数**，该**函数是同步**的并且会被**立即执行**，所以我们称之为**起始函数**。起始函数包含两个参数 **resolve** 和 **reject**，分别表示 **Promise** **成功**和**失败**的状态。

  > ​	起始函数执行成功时，它应该调用 resolve 函数并传递成功的结果。
  >
  > ​	当起始函数执行失败时，它应该调用 reject 函数并传递失败的原因。

Promise 构造函数返回一个 Promise 对象，该对象具有以下几个方法：

- **then**：用于处理 Promise 成功状态的**回调函数**。
- **catch**：用于处理 Promise 失败状态的**回调函数**。
- **finally**：无论 Promise 是成功还是失败，**都会执行的回调函数**。

下面是一个使用 Promise 构造函数创建 Promise 对象的例子：

当 Promise 被构造时，起始函数会被同步执行：

```js
const promise = new Promise((resolve, reject) => {
  // 异步操作
  setTimeout(() => {
    if (Math.random() < 0.5) {
      resolve('success');
    } else {
      reject('error');
    }
  }, 1000);
});

promise.then(result => {
  console.log(result);
}).catch(error => {
  console.log(error);
});
```

在上面的例子中，我们使用 Promise 构造函数创建了一个 Promise 对象，并使用 setTimeout 模拟了一个异步操作。如果异步操作成功，则调用 resolve 函数并传递成功的结果；如果异步操作失败，则调用 reject 函数并传递失败的原因。然后我们使用 then 方法处理 Promise 成功状态的回调函数，使用 catch 方法处理 Promise 失败状态的回调函数。

这段程序会直接输出 **error** 或 **success**。

resolve 和 reject 都是函数，其中调用 resolve 代表一切正常，reject 是出现异常时所调用的：

#### resolve回调函数举例

```js
new Promise(function (resolve, reject) {
    var a = 0;
    var b = 1;
    if (b == 0) reject("Divide zero");
    else resolve(a / b);  //成功状态
}).then(function (value) { //成功后执行
    console.log("a / b = " + value);
}).catch(function (err) {
    console.log(err);
}).finally(function () {
    console.log("End"); 
});
```

输出结果：

```
 a / b = 0
 End
```

Promise 类有 .then() .catch() 和 .finally() 三个方法，这三个方法的参数都是一个函数，.then() 可以将参数中的函数添加到当前 Promise 的正常执行序列，.catch() 则是设定 Promise 的异常处理序列，.finally() 是在 Promise 执行的最后一定会执行的序列。 .then() 传入的函数会按顺序依次执行，有任何异常都会直接跳到 catch 序列：

```js
new Promise(function (resolve, reject) {
    console.log(1111);
    resolve(2222);
}).then(function (value) {
    console.log(value);
    return 3333;
}).then(function (value) {
    console.log(value);
    throw "An error";
}).catch(function (err) {
    console.log(err);
});
```

执行结果：

```
1111
2222
3333
An error
```

resolve() 中可以放置一个参数用于向下一个 then 传递一个值，then 中的函数也可以返回一个值传递给下一个 then。但是，如果 then 中返回的是一个 Promise 对象，那么下一个 then 将相当于对这个返回的 Promise 进行操作，这一点从刚才的计时器的例子中可以看出来。

reject() 参数中一般会传递一个异常给之后的 catch 函数用于处理异常。

但是请注意以下两点：

- resolve 和 reject 的作用域只有起始函数，不包括 then 以及其他序列；
- resolve 和 reject 并不能够使起始函数停止运行，别忘了 return。

### Promise 函数

上述的 "计时器" 程序看上去比函数瀑布还要长，所以我们可以将它的核心部分写成一个 Promise 函数：

```js
function print(delay, message) {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            console.log(message);
            resolve();
        }, delay);
    });
}
```

然后我们就可以放心大胆的实现程序功能了：

```js
print(1000, "First").then(function () {
    return print(4000, "Second");
}).then(function () {
    print(3000, "Third");
});
```

这种**返回值为一个 Promise 对象的函数称作 Promise 函数**，它**常常用于开发基于异步操作的库**。

### 回答常见的问题（FAQ）

**Q: then、catch 和 finally 序列能否顺序颠倒？**

A: 可以，效果完全一样。但不建议这样做，最好按 then-catch-finally 的顺序编写程序。

**Q: 除了 then 块以外，其它两种块能否多次使用？**

A: 可以，finally 与 then 一样会按顺序执行，但是 catch 块只会执行第一个，除非 catch 块里有异常。所以最好只安排一个 catch 和 finally 块。

**Q: then 块如何中断？**

A: then 块默认会向下顺序执行，return 是不能中断的，可以通过 throw 来跳转至 catch 实现中断。

**Q: 什么时候适合用 Promise 而不是传统回调函数？**

A: 当需要多次顺序执行异步操作的时候，例如，如果想通过异步方法先后检测用户名和密码，需要先异步检测用户名，然后再异步检测密码的情况下就很适合 Promise。

**Q: Promise 是一种将异步转换为同步的方法吗？**

A: 完全不是。Promise 只不过是一种更良好的编程风格。

**Q: 什么时候我们需要再写一个 then 而不是在当前的 then 接着编程？**

A: 当你又需要调用一个异步任务的时候。

我们可以将这段代码变得更好看：

```js
async function asyncFunc() {
    await print(1000, "First");
    await print(4000, "Second");
    await print(3000, "Third");
}
asyncFunc();
```

这岂不是将异步操作变得像同步操作一样容易了吗！

这次的回答是肯定的，异步函数 async function 中可以使用 await 指令，**await 指令后必须跟着一个 Promise**，异步函数会在这个 Promise 运行中暂停，直到其运行结束再继续运行。

异步函数实际上原理与 Promise 原生 API 的机制是一模一样的，只不过更便于程序员阅读。



处理异常的机制将用 try-catch 块实现：

```js
async function asyncFunc() {
    try {
        await new Promise(function (resolve, reject) {
            throw "Some error"; // 或者 reject("Some error")
        });
    } catch (err) {
        console.log(err);
        // 会输出 Some error
    }
}
asyncFunc();
```

如果 Promise 有一个正常的返回值，await 语句也会返回它：

```js
async function asyncFunc() {
    let value = await new Promise(
        function (resolve, reject) {
            resolve("Return value");
        }
    );
    console.log(value);
}
asyncFunc();
```

结果：

```js
Return value
```

值得注意的是：上面的例子只是便于理解，在真的需求中我们可以合理的修改reslove和reject函数的使用场景来满足需求