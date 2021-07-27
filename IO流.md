# IO流

### File 类型

-  File 类的一个对象，代表一个文件或一个文件目录(俗称：文件夹)
-  File 类声明在 `java.io` 包下
-  File 类中涉及到关于文件或文件目录的创建、删除、重命名、修改时间、文件大小等方法，并未涉及到写入或读取文件内容的操作。如果需要读取或写入文件内容，必须使用 IO 流来完成
-  File 类的对象常会作为参数传递到流的构造器中，指明读取或写入的"终点"

##### 创建File类的实例

- `File(String filePath)`
- `File(String parentPath,String childPath)`
- `File(File parentFile,String childPath)`

```java
		@Test
    public void test1() {
        //构造器1
        File file1 = new File("hello.txt");		//相对于当前module
        File file2 = new File("D:\\workspace_idea1\\JavaSenior\\day08\\he.txt");

        System.out.println(file1);	// hello.txt
        System.out.println(file2);	// D:\workspace_idea1\JavaSenior\day08\he.txt

        //构造器2：
        File file3 = new File("D:\\workspace_idea1", "JavaSenior");
        System.out.println(file3);	// D:\workspace_idea1/JavaSenior

        //构造器3：
        File file4 = new File(file3, "hi.txt");
        System.out.println(file4);	// D:\workspace_idea1/JavaSenior/hi.txt
    }
    
   	//路径分隔符 	windows:\\   	unix:/
```

##### File 类型常用方法

- `public String getAbsolutePath()`：获取绝对路径
- `public String getPath()` ：获取路径
- `public String getName()` ：获取名称
- `public String getParent()`：获取上层文件目录路径。若无，返回null
- `public long length() `：获取文件长度（即：字节数）。不能获取目录的长度
- `public long lastModified()` ：获取最后一次的修改时间，毫秒值。
常用 `System.out.println(new Date(file.lastModified()));`
- `public String[] list()` ：获取指定目录下的所有文件或者文件目录的名称数组
- `public File[] listFiles() `：获取指定目录下的所有文件或者文件目录的 File 数组
- `public boolean renameTo(File dest)`：把文件重命名为指定的文件路径(剪切)
     比如：`file1.renameTo(file2)`：要想保证返回 `true`，需要 file1 在硬盘中**存在**，且 file2 在硬盘中**不存在**
- `public boolean isDirectory()`：判断是否是文件目录
- `public boolean isFile()` ：判断是否是文件
- `public boolean exists()` ：判断是否存在
- `public boolean canRead()` ：判断是否隐藏
- `public boolean createNewFile()`：创建文件。若文件存在，则不创建，返回 false
- `public boolean mkdir()`：创建文件目录。如果此文件目录存在，就不创建。如果此文件目录的上层目录不存在，也不创建
- `public boolean mkdirs()`：创建文件目录。如果此文件目录存在，就不创建。如果上层文件目录不存在，一并创建
- `public boolean delete()`：删除文件或者文件夹。要想删除成功，文件目录下不能有子目录或文件

------

### IO 流

<p align="center">
<img src="https://github.com/TortoiseKnightB/Java_notes/blob/main/images/IO流/003.jpg?raw=true" width=500/>
</p>

- 模型图：文件（File）<==> 管道（IO流）<==>程序（内存）

##### IO 流的分类

- 操作数据单位：字节流、字符流
- 数据的流向：输入流、输出流
- 流的角色：节点流、处理流

<p align="center">
<img src="https://github.com/TortoiseKnightB/Java_notes/blob/main/images/IO流/001.jpg?raw=true" width=500/>
</p>

##### IO 流的体系结构

|抽象基类|节点流（或文件流）|缓冲流（处理流的一种）|
|:----:|:----|:----|
|InputStream|`FileInputStream   (read(byte[] buffer))`|`BufferedInputStream (read(byte[] buffer))`|
|OutputStream|`FileOutputStream  (write(byte[] buffer,0,len)`|`BufferedOutputStream (write(byte[] buffer,0,len) / flush()`|
|Reader|`FileReader (read(char[] cbuf))`|`BufferedReader (read(char[] cbuf) / readLine())`|
|Writer|`FileWriter (write(char[] cbuf,0,len)`|`BufferedWriter (write(char[] cbuf,0,len) / flush()`|

<p align="center">
<img src="https://github.com/TortoiseKnightB/Java_notes/blob/main/images/IO流/002.jpg?raw=true" width=500/>
</p>

### 节点流（文件流）

- 不能使用字符流来处理图片等字节数据
- 使用字节流 FileInputStream 处理文本文件，可能出现乱码(如果只进行文件的复制，不进行读操作，则没有影响)
- 对于**文本文件**(.txt,.java,.c,.cpp)，使用**字符流**处理
- 对于**非文本文件**(.jpg,.mp3,.mp4,.avi,.doc,.ppt,...)，使用**字节流**处理

##### FileReader

将 module 中的 hello.txt 文件内容读入程序中，并输出到控制台

- File 类的实例化
- FileReader 流的实例化
- 读入的操作
- 资源的关闭

```java
		@Test
    public void testFileReader()  {
        FileReader fr = null;
        try {
            //1.File类的实例化
            File file = new File("hello.txt");

            //2.FileReader流的实例化
            fr = new FileReader(file);

            //3.读入的操作
            //read(char[] cbuf):返回每次读入cbuf数组中的字符的个数。如果达到文件末尾，返回-1
          	// read():读取一个字符，返回ASCII码，末尾返回-1
            char[] cbuf = new char[5];
            int len;
            while((len = fr.read(cbuf)) != -1){
                //方式一：
                //错误的写法
//                for(int i = 0;i < cbuf.length;i++){
//                    System.out.print(cbuf[i]);
//                }
                //正确的写法
//                for(int i = 0;i < len;i++){
//                    System.out.print(cbuf[i]);
//                }
              
                //方式二：
                //错误的写法,对应着方式一的错误的写法
//                String str = new String(cbuf);
//                System.out.print(str);
                //正确的写法
                String str = new String(cbuf,0,len);
                System.out.print(str);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(fr != null){
                //4.资源的关闭
                try {
                    fr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

##### FileWriter

从内存中写出数据到硬盘的文件里

- 提供 File 类的对象，指明写出到的文件
- 提供 FileWriter 的对象，用于数据的写出
- 写出的操作
- 流资源的关闭

```java
		@Test
    public void testFileWriter() {
        FileWriter fw = null;
        try {
            //1.提供File类的对象，指明写出到的文件
            File file = new File("hello1.txt");

            //2.提供FileWriter的对象，用于数据的写出(false 覆盖，true 追加)
            fw = new FileWriter(file,false);

            //3.写出的操作
            fw.write("I have a dream!\n");
            fw.write("you need to have a dream!");
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.流资源的关闭
            if(fw != null){
                try {
                    fw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

**注意**

- 输出操作，对应的 File 可以不存在的，并不会报异常
- File 对应的硬盘中的文件如果不存在，在输出的过程中，会自动创建此文件
- File 对应的硬盘中的文件如果存在：
  - 如果流使用的构造器是：`FileWriter(file,false) / FileWriter(file)`: 对原有文件的覆盖
  - 如果流使用的构造器是：`FileWriter(file,true)`: 不会对原有文件覆盖，而是在原有文件基础上追加内容

```java
		// 输入+输出	
		@Test
    public void testFileReaderFileWriter() {
        FileReader fr = null;
        FileWriter fw = null;
        try {
            //1.创建File类的对象，指明读入和写出的文件
            File srcFile = new File("hello.txt");
            File destFile = new File("hello2.txt");

            //2.创建输入流和输出流的对象
            fr = new FileReader(srcFile);
            fw = new FileWriter(destFile);

            //3.数据的读入和写出操作
            char[] cbuf = new char[5];
            int len;//记录每次读入到cbuf数组中的字符的个数
            while((len = fr.read(cbuf)) != -1){
                //每次写出len个字符
                fw.write(cbuf,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.关闭流资源
            try {
                if(fw != null)
                    fw.close();
            } catch (IOException e) {
                e.printStackTrace();
            }

            try {
                if(fr != null)
                    fr.close();
            } catch (IOException e) {
                e.printStackTrace();
            }

        }
    }
```

##### FileInputStream & FileOutputStream

字节流拷贝图片

```java
 		@Test
    public void testFileInputOutputStream() {
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            File srcFile = new File("爱情与友情.jpg");
            File destFile = new File("爱情与友情2.jpg");

            fis = new FileInputStream(srcFile);
            fos = new FileOutputStream(destFile);

            //复制的过程
            byte[] buffer = new byte[5];
            int len;
            while ((len = fis.read(buffer)) != -1) {
                fos.write(buffer, 0, len);
            }

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fos != null) {
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (fis != null) {
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

------

### 缓冲流（处理流一）

- 内部提供了一个缓冲区，提高流的读取、写入的速度

##### BufferedInputStream & BufferedOutputStream

- 造文件
- 造流(节点流、缓冲流)
- 复制(读取、写入)
- 资源关闭(关闭外层流，内层流自动省略)

```java
		/*
    实现非文本文件的复制
     */
    @Test
    public void BufferedStreamTest() throws FileNotFoundException {
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;

        try {
            //1.造文件
            File srcFile = new File("爱情与友情.jpg");
            File destFile = new File("爱情与友情3.jpg");
            //2.造流
            //2.1 造节点流
            FileInputStream fis = new FileInputStream((srcFile));
            FileOutputStream fos = new FileOutputStream(destFile);
            //2.2 造缓冲流
            bis = new BufferedInputStream(fis);
            bos = new BufferedOutputStream(fos);

            //3.复制的细节：读取、写入
            byte[] buffer = new byte[10];
            int len;
            while((len = bis.read(buffer)) != -1){
                bos.write(buffer,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.资源关闭
            //要求：先关闭外层的流，再关闭内层的流
            if(bos != null){
                try {
                    bos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
            if(bis != null){
                try {
                    bis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
            //说明：关闭外层流的同时，内层流也会自动的进行关闭。关于内层流的关闭，我们可以省略.
//        fos.close();
//        fis.close();
        }
    }
```

##### BufferedReader & BufferedWriter

```java
		// 使用BufferedReader和BufferedWriter实现文本文件的复制
		@Test
    public void testBufferedReaderBufferedWriter() {
        BufferedReader br = null;
        BufferedWriter bw = null;
        try {
            //创建文件和相应的流
            br = new BufferedReader(new FileReader(new File("dbcp.txt")));
            bw = new BufferedWriter(new FileWriter(new File("dbcp1.txt")));

            //读写操作
            //方式一：使用char[]数组
//            char[] cbuf = new char[1024];
//            int len;
//            while((len = br.read(cbuf)) != -1){
//                bw.write(cbuf,0,len);
//    //            bw.flush();			// 清空缓存，缓存写入
//            }

            //方式二：使用String
            String data;
            while ((data = br.readLine()) != null) {
                //方法一：
//                bw.write(data + "\n");//data中不包含换行符
                //方法二：
                bw.write(data);//data中不包含换行符
                bw.newLine();//提供换行的操作
            }
          
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //关闭资源
            if (bw != null) {
                try {
                    bw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (br != null) {
                try {
                    br.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

------

### 转换流（处理流二）

- 提供字节流与字符流之间的转换
- `InputStreamReader`：将一个字节的输入流转换为字符的输入流
- `OutputStreamWriter`：将一个字符的输出流转换为字节的输出流

<p>
<img src="https://github.com/TortoiseKnightB/Java_notes/blob/main/images/IO流/004.jpg?raw=true" width=500/>
</p>

```java
 		/*
    综合使用InputStreamReader和OutputStreamWriter
     */
    @Test
    public void test2() {
        InputStreamReader isr = null;
        OutputStreamWriter osw = null;
        try {
            //1.造文件、造流
            File file1 = new File("dbcp.txt");
            File file2 = new File("dbcp_gbk.txt");

            FileInputStream fis = new FileInputStream(file1);
            FileOutputStream fos = new FileOutputStream(file2);

            isr = new InputStreamReader(fis, "utf-8");
            osw = new OutputStreamWriter(fos, "gbk");

            //2.读写过程
            char[] cbuf = new char[20];
            int len;
            while ((len = isr.read(cbuf)) != -1) {
                osw.write(cbuf, 0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //3.关闭资源
            if (isr != null) {
                try {
                    isr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (osw != null) {
                try {
                    osw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

##### 字符集

- ASCII：美国标准信息交换码。用一个字节的7位可以表示
- ISO8859-1：拉丁码表，欧洲码表。用一个字节的8位表示
- GB2312：中国的中文编码表。最多两个字节编码所有字符
- GBK：中国的中文编码表升级，融合了更多的中文文字符号。最多两个字节编码
- Unicode：国际标准码，融合了目前人类使用的所有字符。为每个字符分配唯一的字符码。所有的文字都用两个字节来表示
- UTF-8：变长的编码方式，可用1-4个字节来表示一个字符

<p>
<img src="https://github.com/TortoiseKnightB/Java_notes/blob/main/images/IO流/005.jpg?raw=true" width=500/>
</p>

------

### 其他流

##### 标准输入、输出流

- System.in:标准的输入流，默认从键盘输入
- System.out:标准的输出流，默认从控制台输出



- 方法一：使用Scanner实现，调用next()返回一个字符串
- 方法二：使用System.in实现。System.in  --->  转换流 ---> BufferedReader的readLine()

##### 打印流

- PrintStream 和PrintWriter 提供了一系列重载的print() 和 println()

```java
  	@Test
    public void test2() {
        PrintStream ps = null;
        try {
            FileOutputStream fos = new FileOutputStream(new File("text.txt"));
            // 创建打印输出流,设置为自动刷新模式(写入换行符或字节 '\n' 时都会刷新输出缓冲区)
            ps = new PrintStream(fos, true);
            if (ps != null) {// 把标准输出流(控制台输出)改成文件
                System.setOut(ps);
            }
            for (int i = 0; i <= 255; i++) { // 输出ASCII字符
                System.out.print((char) i);
                if (i % 50 == 0) { // 每50个数据一行
                    System.out.println(); // 换行
                }
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } finally {
            if (ps != null) {
                ps.close();
            }
        }
    }
```

##### 数据流

- DataInputStream 和 DataOutputStream 用于读取或写出基本数据类型的变量或字符串

将内存中的字符串、基本数据类型的变量写出到文件中

```java
		@Test
    public void test3() throws IOException {
        //1.
        DataOutputStream dos = new DataOutputStream(new FileOutputStream("data.txt"));
        //2.
        dos.writeUTF("刘建辰");
        dos.flush();//刷新操作，将内存中的数据写入文件
        dos.writeInt(23);
        dos.flush();
        dos.writeBoolean(true);
        dos.flush();
        //3.
        dos.close();
    }
```

将文件中存储的基本数据类型变量和字符串读取到内存中，保存在变量中

注意点：读取不同类型的数据的顺序要与当初写入文件时，保存的数据的顺序一致

```java
		@Test
    public void test4() throws IOException {
        //1.
        DataInputStream dis = new DataInputStream(new FileInputStream("data.txt"));
        //2.
        String name = dis.readUTF();
        int age = dis.readInt();
        boolean isMale = dis.readBoolean();

        System.out.println("name = " + name);
        System.out.println("age = " + age);
        System.out.println("isMale = " + isMale);

        //3.
        dis.close();
    }
```

------

### 对象流（序列化/反序列化）

- ObjectInputStream 和 ObjectOutputStream
- 作用：用于存储和读取**基本数据类型数据**或**对象**的处理流。它的强大之处就是可以把 Java 中的对象写入到数据源中，也能把对象从数据源中还原回来
- 要想一个java对象是可序列化的，需要满足相应的要求：
  - 需要实现接口：Serializable
  - 当前类提供一个全局常量：serialVersionUID
  - 除了当前类需要实现 Serializable 接口之外，还必须保证其内部所有属性也必须是可序列化的（默认情况下，基本数据类型可序列化）
- 补充：ObjectOutputStream 和 ObjectInputStream 不能序列化 static 和 transient 修饰的成员变量

```java
// 可序列化的对象类
public class Person implements Serializable {

    public static final long serialVersionUID = 475463534532L;

    private String name;
    private int age;
    private int id;
    private Account acct;

    public Person(String name, int age, int id) {
        this.name = name;
        this.age = age;
        this.id = id;
    }

    public Person(String name, int age, int id, Account acct) {
        this.name = name;
        this.age = age;
        this.id = id;
        this.acct = acct;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", id=" + id +
                ", acct=" + acct +
                '}';
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
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

    public Person(String name, int age) {

        this.name = name;
        this.age = age;
    }

    public Person() {

    }
}


class Account implements Serializable {
    public static final long serialVersionUID = 4754534532L;
    private double balance;

    @Override
    public String toString() {
        return "Account{" +
                "balance=" + balance +
                '}';
    }

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {
        this.balance = balance;
    }

    public Account(double balance) {
        this.balance = balance;
    }
}
```

```java
		// 对象流的操作
    /*
    序列化过程：将内存中的java对象保存到磁盘中或通过网络传输出去
    使用ObjectOutputStream实现
     */
    @Test
    public void testObjectOutputStream() {
        ObjectOutputStream oos = null;

        try {
            //1.
            oos = new ObjectOutputStream(new FileOutputStream("object.dat"));
            //2.
            oos.writeObject(new String("我爱北京天安门"));
            oos.flush();//刷新操作

            oos.writeObject(new Person("王铭", 23));
            oos.flush();

            oos.writeObject(new Person("张学良", 23, 1001, new Account(5000)));
            oos.flush();

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (oos != null) {
                //3.
                try {
                    oos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
        }

    }

    /*
    反序列化：将磁盘文件中的对象还原为内存中的一个java对象
    使用ObjectInputStream来实现
     */
    @Test
    public void testObjectInputStream() {
        ObjectInputStream ois = null;
        try {
            ois = new ObjectInputStream(new FileInputStream("object.dat"));

            Object obj = ois.readObject();
            String str = (String) obj;

            Person p = (Person) ois.readObject();
            Person p1 = (Person) ois.readObject();

            System.out.println(str);
            System.out.println(p);
            System.out.println(p1);

        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            if (ois != null) {
                try {
                    ois.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
        }
    }
```

------

### 随机存取文件流

- RandomAccessFile
- RandomAccessFile 直接继承于 java.lang.Object 类，实现了 DataInput和DataOutput 接口
- RandomAccessFile 既可以作为一个输入流，又可以作为一个输出流
- 如果 RandomAccessFile 作为输出流时，写出到的文件如果不存在，则在执行过程中自动创建。
- 如果写出到的文件存在，则会对原有文件内容进行覆盖。（默认情况下，从头覆盖）
- 可以通过相关的操作，实现 RandomAccessFile **“插入”**数据的效果

```java
		// 基本读写
    @Test
    public void test1() {
        RandomAccessFile raf1 = null;
        RandomAccessFile raf2 = null;
        try {
            //1.
            raf1 = new RandomAccessFile(new File("爱情与友情.jpg"),"r");
            raf2 = new RandomAccessFile(new File("爱情与友情1.jpg"),"rw");
            //2.
            byte[] buffer = new byte[1024];
            int len;
            while((len = raf1.read(buffer)) != -1){
                raf2.write(buffer,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //3.
            if(raf1 != null){
                try {
                    raf1.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
            if(raf2 != null){
                try {
                    raf2.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

		// 指定位置读写
    @Test
    public void test2() throws IOException {

        RandomAccessFile raf1 = new RandomAccessFile("hello.txt","rw");

        raf1.seek(3);//将指针调到角标为3的位置
        raf1.write("xyz".getBytes());//

        raf1.close();

    }


    /*
    使用RandomAccessFile实现数据的插入效果
     */
    @Test
    public void test3() throws IOException {

        RandomAccessFile raf1 = new RandomAccessFile("hello.txt","rw");

        raf1.seek(3);//将指针调到角标为3的位置
        //保存指针3后面的所有数据到StringBuilder中
        StringBuilder builder = new StringBuilder((int) new File("hello.txt").length());
        byte[] buffer = new byte[20];
        int len;
        while((len = raf1.read(buffer)) != -1){
            builder.append(new String(buffer,0,len)) ;
        }
        //调回指针，写入“xyz”
        raf1.seek(3);
        raf1.write("xyz".getBytes());

        //将StringBuilder中的数据写入到文件中
        raf1.write(builder.toString().getBytes());

        raf1.close();

        //思考：将StringBuilder替换为ByteArrayOutputStream
    }
```

------

