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