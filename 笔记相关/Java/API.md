## API

#### 说明

​		本文档记录常用的API（包括实现）以及 使用场景即分析

#### **bitCount**(对于整数，short，int ，long，bigInteger都可以使用)

```java
@IntrinsicCandidate
public static int bitCount(int i) {
    // HD, Figure 5-2
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

使用：e：Integer类中的静态方法，只需要利用Integer/.bitCount( int/integer ) 返回 二进制中 **1 的个数**

场景：当需要计算二进制1的个数时

时间复杂度O(1)    空间复杂度S(1)