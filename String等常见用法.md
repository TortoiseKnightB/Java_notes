# String等常用方法

### String类的常用方法

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

------



