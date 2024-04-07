# Vue中的数据代理

### You need  to know

> **Object.defineProperty**
>
> 望文生义：defineProperty：定义属性
>
> 用法：**Object.defineProperty**(对象，属性名，配置项)
>
> 通过这个方法定义的属性**默认**
>
> - **不参与遍历**（不可枚举，可以通过配置项中配置enumerable：true设置为可以枚举）
> - **不可以修改**（配置writable:true 设置为可以修改）
> - **不可以被删除**（配置configurable：true 设置为可以删除 delete 对象.属性）
>
> 这样定义出来的属性是普通的属性，并不是响应式的，这时我们可以在配置项中配置
>
> get：function（）//获取属性值    这个配置让每次获取属性值的时候都会走**get**方法
>
> set：function（）//设置属性值    这个配置让每次修改属性值的时候都会走**set**方法

​		数据代理是Vue.js中一种机制，它允许你将组件实例的属性或方法代理到其内部的数据对象上。这样做的主要目的是为了让组件实例更方便地访问和操作其内部数据，同时也使得数据的变化能够触发视图的更新，做到响应式布局。

​		Vue.js使用了`Object.defineProperty`来实现数据代理。这个方法允许你在对象上定义一个新属性或修改现有属性的特性，比如配置其`get`和`set`方法。

![image-20240308161332548](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20240308161332548.png)

1. Vue中的数据代理：通过vm对象来代理data对象中属性的操作（读/写）
2. Vue中数据代理的好处：更加方便的操作data中的数据
3. 基本原理：

​			通过object.defineProperty()把data对象中所有属性添加到vm上。
​			为每一个添加到vm上的属性，都指定一个getter/setter。
​			在getter/setter内部去操作（读/写）data中对应的属性。