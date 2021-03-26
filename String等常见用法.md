# String等常用方法

### String 类的常用方法

&ensp;&ensp;&ensp;&ensp;注意：由于 String 的字符序列**不可变性**，需要用新的 String 类对象来接收返回值，如`String s2 = s1.toLowerCase`

- `int length()`：返回字符串的长度

- `char charAt(int index)`：返回索引处字符

- `boolean isEmpty()`：判断是否为空

- `String toLowerCase()`：转小写

- `String toUpperCase`：转大写

- `String trim()`：忽略首部和尾部空白

- `boolean equals(Object obj)`：比较内容是否相同

- `boolean equalsIgnoreCase(String anotherString)`：忽略大小写比较内容

- `String contact(String str)`：连接指定字符串到末尾，等价于`+`

- `int compareTo(String anotherString )`：比较大小，大于返回正数 s1 - s2

- `String substring(int beginIndex)`：截取子字符串，从指定位置开始到最后

- `String substring(int beginIndex, int endIndex)`：截取指定位置字符串，左闭右开区间

------

- `boolean endsWith(String suffix)`：判断是否以指定后缀结束

- `boolean startsWith(String prefix)`：判断是否以指定前缀开始

- `boolean startsWith(String prefix, int offset)`：从字符串指定位置开始，判断是否以指定前缀开始

<p aline="center">
	<img src="https://raw.githubusercontent.com/TortoiseKnightB/Java_notes/main/images/String等常见用法/01.jpg" width="600" />
</p>

------

<p aline="center">
	<img src="https://raw.githubusercontent.com/TortoiseKnightB/Java_notes/main/images/String等常见用法/02.jpg" width="600" />
</p>
-------

### String类与其他类型的转换

- 与**基本数据类型**的转换

&ensp;&ensp;&ensp;&ensp;`int i = Integer.parseInt(str);`

&ensp;&ensp;&ensp;&ensp;`String str = String.valueOf(i);`

- 与**字符数组**的转换

```java
        char[] chars = {'a', 'b', 'c', 'd', 'e'};
        String s1 = new String(chars);
        System.out.println(s1);     // abcde
        String s2 = new String(chars, 2, 2);
        System.out.println(s2);     // cd
```
```java
        String s3 = "ABCDEFG";
        char[] chars1 = s3.toCharArray();
        System.out.println(chars1);     // ABCDEFG
        s3.getChars(3,5,chars1,1);			// 将字符串 s3 的第 3、4 位拷贝到数组 chars1 的第 1、2 位上
        System.out.println(chars1);     // ADEDEFG
```

- 与**字节数组**的转换

```java
 				String s3 = "ABC";
        byte[] bytes = s3.getBytes();
//	  	bytes = s3.getBytes(StandardCharsets.UTF_8); 	// 使用指定的字符集
        System.out.println(bytes[0]);   // 65
        System.out.println(bytes[1]);   // 66
        System.out.println(bytes[2]);   // 67
        System.out.println(Arrays.toString(bytes));     // [65, 66, 67]
```


```java
				byte[] bytes = {97, 98, 99, 100, 101};
        String s = new String(bytes);
        System.out.println(s);     // abcde
        s = new String(bytes,1,3);  // 输出字节数组 bytes 从下标 1 开始的 3 个字节
        System.out.println(s);      // bcd
```

------

### StringBuffer 类的常用方法

<p aline="center">
	<img src="https://raw.githubusercontent.com/TortoiseKnightB/Java_notes/main/images/String等常见用法/03.png" width="600" />
</p>

- `public int indexOf(String str)`：返回 str 第一次出现的位置
- `public String substring(int start)`：截取从指定位置开始往后的子字符串
- `public String substring(int start, int end)`：截取 `[start, end)` 区间的字符串
- `public int length()`：返回长度
- `public char charAt(int n)`：返回指定位置的字符
- `public void setCharAt(int n, char ch)`：替换指定位置的字符

### StringBuilder 类的常用方法

- （同 StringBuffer 类）

