# Intput&Output Stream
> Java的输入输出流

## 1. File
> 文件类

### 常用方法
- `getName() ` : 文件名，若是根据路径创建的对象，则返回最后一级别的路径
- `getPath()` : 返回路径
- `getAbsolutePath()`: 绝对路径
- `getAbsoluteFile()` : 绝对路径+文件名
- `getParent()`： 父目录名
- `renameTo(File newName)`: 重命名
- // 检测相关
- `exists()` 是否存在
- `canWrite(); canRead();`
- `isFile(); isDirectory();`
- `isAbsolute()` 是否为绝对路径
- `lastModified()` 最后修改时间
- `length()` 文件内容大小
- //文件操作
- `createNewFile()` 创建
- `delete()`
- `createTempFiles(String prefix, String suffix, File direcotry = null); ` 临时文件
- `deleteOnExit();` 注册一个删除钩子，程序over时删除
- // 目录操作
- `mkdir()`
- `String[] list()` file对象的所有子文件名和路径名
- `File[] listFiles()` file对象的所有子文件和路径
- `listRootes()` 列出系统所有根路径，静态方法

### 文件过滤器
list方法中可以接受一个FilenameFilter函数接口，进行过滤
```java
String[] files = file.list(new FilenameFilter() {
    @Override
    public boolean accept(File dir, String name) {
        boolean ans =  name.endsWith(".java") || new File(name).isDirectory();
        return ans;
    }
});
// lambda 进阶
String[] files = file.list((dir, name) ->  name.endsWith("java") || new File(name).isDIrectory));
```

### 2. Stream, Reader, Writer
> 输入输出流均为接口形式，需要根据实际情况实例化
>
> InputStream, outputSteam 和 Reader，Wirte的区别在于前者输入输出都是字节（Byte）后者为字符（Char）

#### 基本用法： read，write

```java
try{
    InputStream in = new FileInputStream("in.text");
    OutputStream out = new FileOutputStream("out.text");
    byte[] buf = new byte[1024];
    int hasRead = 0; // 实际读取的字节数
    while((hasRead = in.read(buf) > 0) {
        // 结束的时候，返回的是-1
        System.out.println(new String(buf, 0, hasRead));
        out.write(buf, 0, hasRead);
    }
    stream.close(); // 关闭输入流 
} catch (Exception e) {
    ...
}
```

#### 处理流
> 利用处理流来包装上面的节点流，程序通过处理流来执行输入输出功能，让节点流与底层IO设备，文件交互

下面的代码也很明显的可以看出优越性，不用关心输出流的关闭
```java
try{
    FileOutputStream out = new FileOutputStream("out.text");
    PrintStream ps = new PrintStream(out);
    ps.println("输出普通");
} catch (Exception e) {

}
```

#### 转换流
> 将字节流转换为字符流
>
> BufferedReader, BufferedWriter


***

## 3. NIO.2
> 更加全面的文件IO和文件系统访问支持

> 基于异步Channel的IO

### Files和Paths

Path处理路径，Files通过处理Path来处理文件

```java
import java.io.file.*;

Path path = Paths.get("."); // 以当前路径创建对象
path.getRoot(); // 获取根路径
Path absolutePath = path.toAbsolutePath();
absolutePath.getNameCount(); // 获取路径数量，如 /usr/yihui/Test.java 就有三层，返回3

// 用后面的参数拼接出路径
Path pp = Paths.get("C:", "publish", "codes");

// Files 用于对文件的操作
// 复制
Files.copy(Paths.get("File1.java"), new FileOutputStream("NewFile.java"));
// 判断是否隐藏
Files.isHiden(Paths.get("File.java"));
List<String> content = Files.readAllLines(Paths.get("Test.java"), Charset.forName("gbk"));

Files.size(Paths.get(File.java)); // 文件大小

Files.write(path, "写入的文字", Charset.forName("gbk"));
```



## 文件遍历

> FileVisitor 

`walkFileTree(Path, FileVisotr<? super Path>)` 遍历path路径下的所有文件&目录

FileVisitor 是一个文件访问器，遍历文件和目录，会触发其中相应的方法

- `FileVisitResult postVisitDirectory(T dir, IOException exc)` 访问子目录之后触发
- `FileVisiResult preVisitDirectory(T dir, BasicFileAttributes attrs)` 访问子目录之前触发
- `FileVisitResult vistFile(T file, BasicFileAttributes attr)` 访问文件时触发
- `FileVisitResult visitFileFailed(T file, IOException exc)` 访问失败时触发

所以在使用的时候，一般是：

```java
Files.walkFileTree(Paths.get("Test.java"), new SimpleFileVisitor<Path>() {
    @Override
    public FileVisitResult visitFiles(Path file, BasicFileAttributes attrs) throws IOException {
        ....
        return FileVisitResult.CONTINUE;
    }
    ...
});
```
即，在遍历的时候，重写FileVistor类, 主要是上面提到的四个方法

返回的FileVisitResult是一个枚举类

- CONTINUE  继续访问
- SKIP_SIBLINGS 继续访问，不访问该文件or目录的兄弟文件或目录
- SKIP_SUBTREE 继续访问，但不访问该文件or该目录树
- TERMINATE 终止