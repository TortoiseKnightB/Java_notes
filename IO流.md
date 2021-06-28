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
