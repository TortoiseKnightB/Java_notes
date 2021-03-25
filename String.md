# String笔记

##### String 内存原理

&ensp;&ensp;&ensp;&ensp;查看 String 源码

<p align="center">
        <img src="https://raw.githubusercontent.com/TortoiseKnightB/Java_notes/main/images/String/1.png" width="500"/>
</p>

- 发现 String 内部定义了final char[] value 用于存储字符串数据


&ensp;&ensp;&ensp;&ensp;参考尚硅谷关于  String 的讲解（一）
<p align="center">
        <img src="https://raw.githubusercontent.com/TortoiseKnightB/Java_notes/main/images/String/2.jpg" width="500"/>
</p>
- String s1 = "javaEE"; 语句执行时，直接在方法区中的字符串常量池开辟空间存储 "javaEE"

- String s2 = new String("javaEE"); 语句执行时，先在堆中创建一个新的 String 对象，新 String 对象的 final 

  char[] 类型常量 value 指向字符串常量池中的 "javaEE"

- 字符串常量区中相同的字符数组只会存在一个

- 此外 String 字符序列具有不可变性，即若 执行 s1 = "new javaEE"; 语句，不会改变字符串常量池中原 "javaEE" 的值，而是重新在字符串常量池开辟新空间存储 "new javaEE"

&ensp;&ensp;&ensp;&ensp;参考尚硅谷关于  String 的讲解（二）

<p align="center">
        <img src="https://raw.githubusercontent.com/TortoiseKnightB/Java_notes/main/images/String/3.jpg" width="500"/>
</p>




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

- s.str 指向堆空间的 new String 对象，该对象的 value 指向方法区中字符串常量池中的 “good”

- 调用 change 方法，将 s.str 指向的地址赋值给 str2。注意 str2 不等于 s.str，只不过它们两个指向的内存空间地址相同而已，即指向相同的字符串常量池。

- 正常情况下，修改 str2 会修改内存变量的值，导致 s.str1 指向的值也改变。但是由于 String 的不可变性，修改 str2 的值会重新开辟新的内存空间存放 “test”

  ![截屏2021-03-24 下午11.37.37](/Users/liuyang/Desktop/截屏2021-03-24 下午11.37.37.png)

- 修改 str2 前，可以看见 str 和 str2 指向相同的地址 @494

  ![截屏2021-03-24 下午11.37.57](/Users/liuyang/Desktop/截屏2021-03-24 下午11.37.57.png)

- 修改 str2 后，str2 指向新的地址 @496。而 str 并不受影响