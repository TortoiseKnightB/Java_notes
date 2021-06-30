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
<img src=""/>
</p>