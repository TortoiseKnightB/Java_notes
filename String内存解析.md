# String笔记

### String 内存原理

&ensp;&ensp;&ensp;&ensp;查看 String 源码

<p align="center">
        <img src="https://raw.githubusercontent.com/TortoiseKnightB/Java_notes/main/images/String/001.png" width="500"/>
</p>

- 发现 String 内部定义了`final char[] value` 用于存储字符串数据

  ------


&ensp;&ensp;&ensp;&ensp;参考尚硅谷关于  String 的讲解（一）
<p align="center">
        <img src="https://raw.githubusercontent.com/TortoiseKnightB/Java_notes/main/images/String/002.jpg" width="500"/>
</p>

- `String s1 = "javaEE";` 语句执行时，直接在方法区中的**字符串常量池**开辟空间存储 "javaEE"

- `String s2 = new String("javaEE");` 语句执行时，先在堆中创建一个新的 String 对象，新 String 对象的 final 

  char[] 类型常量 **value** 指向字符串常量池中的 "javaEE"

- 字符串常量区中相同的字符数组只会存在一个

  ------

&ensp;&ensp;&ensp;&ensp;此外 String 字符序列具有**不可变性**，如下

<p align="center">
        <img src="https://raw.githubusercontent.com/TortoiseKnightB/Java_notes/main/images/String/003.jpg" width="500"/>
</p>

- 即若执行 `s1 = "hello";` 语句，不会改变字符串常量池中原 "abc" 的值，而是重新在字符串常量池开辟新空间存储 "hello"

  ------

&ensp;&ensp;&ensp;&ensp;参考尚硅谷关于  String 的讲解（二）

<p align="center">
        <img src="https://raw.githubusercontent.com/TortoiseKnightB/Java_notes/main/images/String/004.jpg" width="500"/>
</p>

- `s1 = "hello";` 与 `s3 = "hello" + "word";` 都直接在字符串常量池中赋值

- `s4 = s1 + "world";` 等只要赋值语句中涉及变量，都会在堆中创建新对象

- `(s1 + s2).intern()` 将堆空间的对象转化为字符串常量池中的对象

  ------

  ##### String 形参详解（这一段比较模糊，待以后加深理解再做完善）

&ensp;&ensp;&ensp;&ensp;看下面一段代码，理解其内存原理

```java
public class Solution {
    String str = new String("good");

    public void change(String str2){
        System.out.println(this.str==str2);	// true
        System.out.println(str2);	//good
        str2 = "test";
        System.out.println(this.str==str2);	// false
        System.out.println(str2);	// test
    }

    public static void main(String[] args) {
        Solution s = new Solution();
        System.out.println(s.str);	// good
        s.change(s.str);
        System.out.println(s.str);	// good
    }
}
```

- 首先在栈中生成一个 Solution 类的对象 s，s 指向堆中一块新开辟的空间，空间中存放着 String 类的对象 s.str

- s.str 指向堆空间的 new String 对象，该对象的 value 指向方法区中字符串常量池中的 “good”

- 调用 change 方法，栈中新生成 String 类对象 str2，将 s.str 指向的地址赋值给 str2。注意 str2 不等于 s.str，只是它们两个指向的内存空间地址相同而已，即指向相同的字符串常量池。

- 正常情况下，修改引用类型变量的值会使内存变量的值改变。**但是由于 String 的不可变性，修改 str2 的值会重新开辟新的内存空间存放 “test”，s.str 的值会不改变**

<p align="center">
        <img src="https://raw.githubusercontent.com/TortoiseKnightB/Java_notes/main/images/String/005.png" width="500"/>
</p>

- 修改 str2 前，可以看见 str 和 str2 指向相同的地址 char[4]@494

<p align="center">
        <img src="https://raw.githubusercontent.com/TortoiseKnightB/Java_notes/main/images/String/006.png" width="500"/>
</p>

- 修改 str2 后，str2 指向新的地址 @496。而 str 并不受影响

  ------

