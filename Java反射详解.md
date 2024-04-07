# Java反射详解

## 1.1 反射的概述：

专业解释

是在运行状态中

- 对于任意一个类，都能够知道这个类的所有属性和方法；
- 对于任意一个对象，都能够调用它的任意属性和方法；


这种动态获取信息以及动态调用对象方法的功能称为Java语言的反射机制。

通俗来讲：

* 利用**反射**创建的对象**可以无视修饰符**调用类里面的内容

* 可以跟**配置文件结合起来使用**，把要创建的对象信息和方法写在配置文件中

读取到什么类，就创建什么类的对象

读取到什么方法，就调用什么方法

此时当需求变更的时候不需要修改代码，只要修改配置文件即可。

## 1.2 学习反射到底学什么？

反射都是从class字节码文件中获取的内容。

* 如何获取class字节码文件的对象
* 利用反射如何获取构造方法（创建对象）
* 利用反射如何获取成员变量（赋值，获取值）
* 利用反射如何获取成员方法（运行）

## 1.3 获取字节码文件对象的三种方式

**Student类**

```java
public class Student {
    private String name;
    private int age;

    public String gender;

    public String address;

    public Student() {
    }
    public Student(String name) {
        this.name = name;
    }
    protected Student(int age) {
        this.age = age;
    }
    private Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String toString() {
        return "Student{name = " + name + ", age = " + age + "}";
    }
}
```

1. Class这个类里面的静态方法forName（“全类名”）**（最常用）**

2. 通过class属性获取  

3. 通过对象获取字节码文件对象

   字节码文件对象在内存中是唯一的，所以我们每次获取到的都是**同一个对象**

```java
//1.Class这个类里面的静态方法forName
//Class.forName("类的全类名")： 全类名 = 包名 + 类名
Class clazz1 = Class.forName("com.itheima.reflectdemo.Student");
//源代码阶段获取 --- 先把Student加载到内存中，再获取字节码文件的对象
// clazz 就表示Student这个类的字节码文件对象。
//就是当Student.class这个文件加载到内存之后，产生的字节码文件对象
```

这个全类名手敲比较麻烦，带容易出错，我们可以这样获取

![image-20231023164245947](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20231023164245947.png)

```java
//2.通过class属性获取
//类名.class
Class clazz2 = Student.class;

//因为class文件在硬盘中是唯一的，所以，当这个文件加载到内存之后产生的对象也是唯一的
System.out.println(clazz1 == clazz2);//true
```

```java
//3.通过Student对象获取字节码文件对象
Student s = new Student();
Class clazz3 = s.getClass();
System.out.println(clazz1 == clazz2);//true
System.out.println(clazz2 == clazz3);//true
```

## 1.4 字节码文件和字节码文件对象

java文件：我们自己编写的java代码文件。

字节码文件：就是通过java文件编译之后的class文件（是在硬盘上真实存在的，用眼睛能看到的）

字节码文件对象：当class文件加载到内存之后，JVM自动创建出来的对象。

​					这个对象里面至少包含了：构造方法，成员变量，成员方法。

而我们的反射获取的是什么？字节码文件对象，这个对象在内存中是唯一的。

## 1.5 获取构造方法

| 方法名                                                       | 说明                              |
| ------------------------------------------------------------ | --------------------------------- |
| Constructor<?>[] getConstructors()                           | 获得所有的构造（只能public修饰）  |
| Constructor<?>[] getDeclaredConstructors()                   | 获得所有的构造（包含private修饰） |
| Constructor<T> getConstructor(Class<?>... parameterTypes)    | 获取指定构造（只能public修饰）    |
| Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes) | 获取指定构造（包含private修饰）   |

根据上面的四个方法我们不难发现，当Constructor**s**时，我们获取的构造方法可以是多个，当我们再在中间加上**Declared**就可以获取所有的构造方法(注意**private**修饰的方法我们直接用会报错)

接下来我们逐个介绍

首先我们获取class字节码文件对象

```java
//1.获得整体（class字节码文件对象）
        Class clazz = Class.forName("com.itheima.reflectdemo.Student");
```

```java
//2.获取构造方法对象
        //获取所有构造方法（public）
        Constructor[] constructors1 = clazz.getConstructors();
        for (Constructor constructor : constructors1) {
            System.out.println(constructor);
        }
```

<img src="C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20231023170259381.png" alt="image-20231023170259381"  />

从输出结果来看我们获取到了所有public的构造函数

```java
 //获取所有构造（包括私有的）
        Constructor[] constructors2 = clazz.getDeclaredConstructors();
        for (Constructor constructor : constructors2) {
            System.out.println(constructor);
        }
```

![image-20231023170508899](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20231023170508899.png)

这样我们就把左右的构造函数都获取到了

获取到指定的构造函数

```java
 Constructor con1 = clazz.getDeclaredConstructor();
        System.out.println(con1);

        Constructor con2 = clazz.getDeclaredConstructor(String.class);
        System.out.println(con2);

        Constructor con3 = clazz.getDeclaredConstructor(int.class);
        System.out.println(con3);

        Constructor con4 = clazz.getDeclaredConstructor(String.class,int.class);
        System.out.println(con4);
```

![image-20231023171420293](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20231023171420293.png)

通过不同的参数我们可以获取指定的构造方法

那么你可能会问了，反射有什么用呢

![image-20231023171932878](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20231023171932878.png)



![image-20231023172259605](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20231023172259605.png)

其实在idea的底层就是利用的反射来提示输入的

那现在我们拿到了构造方法我们可以用它来创建对象吗？**可以**

但是当我们直接用private修饰的构造方法来创建对象会报错，我们怎么处理呢

![image-20231023172553730](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20231023172553730.png)

有一个暴力的方法，用于临时取消权限校验，这样就不会报错了

```java
 //暴力反射：表示临时取消权限校验
con4.setAccessible(true);
Student stu = (Student) con4.newInstance("张三", 23);
```

## 1.6 获取成员变量

| 方法名                                        | 备注                                                         |
| --------------------------------------------- | :----------------------------------------------------------- |
| public String **getName**()                   | 返回属性名分                                                 |
| public int **getModifiers**()                 | 获取属性的修饰符列表,返回的修饰符是一个数字，每个数字是修饰符的代号【一般配合Modifier类的toString(int x)方法使用】 |
| public Class<?> **getType**()                 | 以Class类型，返回属性类型【一般配合Class类的getSimpleName()方法使用】 |
| public void **set**(Object obj, Object value) | 设置属性值                                                   |
| public Object **get**(Object obj)             | 读取属性值                                                   |

成员变量的获取和获取构造函数相似，部分内容就不赘述

| 方法名                              | 说明                                         |
| ----------------------------------- | -------------------------------------------- |
| Field[] getFields()                 | 返回所有成员变量对象的数组（只能拿public的） |
| Field[] getDeclaredFields()         | 返回所有成员变量对象的数组，存在就能拿到     |
| Field getField(String name)         | 返回单个成员变量对象（只能拿public的）       |
| Field getDeclaredField(String name) | 返回单个成员变量对象，存在就能拿到           |

```java
//获取单个的成员变量
Field name = clazz.getDeclaredField("name");
System.out.println(name);
//获取权限修饰符
int modifiers = name.getModifiers();
System.out.println(modifiers);

//获取成员变量的名字
String n = name.getName();
System.out.println(n);

//获取成员变量的数据类型
Class<?> type = name.getType();
System.out.println(type);
```

接下来介绍获取成员变量的值

当然，我们想获取或者修改值，我们肯定得创建对象

```java
Student s = new Student("zhangsan",23,"男");
```

可以直接用get方法获取值，set方法设置值（注意private的需要setAccessible(true)临时取消权限校验）

![image-20231023190107585](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20231023190107585.png)



## 1.7 获取成员方法

| 方法名                                             | 备注                                                         |
| -------------------------------------------------- | :----------------------------------------------------------- |
| ublic String **getName**()                         | 返回方法名                                                   |
| public int **getModifiers**()                      | 获取方法的修饰符列表,返回的修饰符是一个数字，每个数字是修饰符的代号【一般配合Modifier类的toString(int x)方法使用】 |
| public Class<?> **getReturnType**()                | 以Class类型，返回方法类型【一般配合Class类的getSimpleName()方法使用】 |
| public Class<?>[] **getParameterTypes**()          | 返回方法的修饰符列表（一个方法的参数可能会有多个。）【结果集一般配合Class类的getSimpleName()方法使用】 |
| public Object **invoke**(Object obj, Object… args) | 调用方法                                                     |

对于获取成员方法还是和前面相似，不多赘述

| 方法名                                                       | 说明                                         |
| ------------------------------------------------------------ | -------------------------------------------- |
| Method[] getMethods()                                        | 返回所有成员方法对象的数组（只能拿public的） |
| Method[] getDeclaredMethods()                                | 返回所有成员方法对象的数组，存在就能拿到     |
| Method getMethod(String name, Class<?>... parameterTypes)    | 返回单个成员方法对象（只能拿public的）       |
| Method getDeclaredMethod(String name, Class<?>... parameterTypes) | 返回单个成员方法对象，存在就能拿到           |

这里有一点和上面不一样

Method[] getMethods()这个方法获取所有public的方法，包括父类的public方法

但是Method[] getDeclaredMethods()这个就无法获取到父类的方法

当我们需要获取指定的方法出现重载的时候，我们需要指定名字以及对应的参数类对象

![image-20231023192041900](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20231023192041900.png)

我们获取到这些方法之后我们怎么使用呢？**invoke**

```java
Student s = new Student();
//3.获取一个指定的方法
//参数一：方法名
//参数二：参数列表，如果没有可以不写
Method eatMethod = clazz.getMethod("eat",String.class);
//运行
//参数一：表示方法的调用对象
//参数二：方法在运行时需要的实际参数
//注意点：如果方法有返回值，那么需要接收invoke的结果
//如果方法没有返回值，则不需要接收
String result = (String) eatMethod.invoke(s, "重庆小面");
System.out.println(result);
```

总结一下

反射

- 对于任意一个类，都能够知道这个类的所有属性和方法；
- 对于任意一个对象，都能够调用它的任意属性和方法；

​		我们首先需要获取字节码文件对象，再通过这个对象获取里面的构造方法，成员变量，以及成员方法，当我们获取到这些之后又可以继续获取他们里面的具体信息，包括方法参数个数啊，参数类型，修饰符啊等等，当获取到构造方法和成员方法之后，我们还能利用invoke来调用方法。这样看来，Java反射就是把java类中的各种成分映射成一个个的Java对象给我们使用

![image-20231023194826263](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20231023194826263.png)

