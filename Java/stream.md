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

#### 基本用法
```
InputStream in = new FileInputStream("in.text");
OutputStream out = new FileOutputStream("out.text");
byte[] buf = new byte[1024];
int hasRead = 0; // 实际读取的字节数
while((hasRead = in.read(buf) > 0) {
    // 结束的时候，返回的是-1
    System.out.println(new String(buf, 0, hasRead));
    out.write(buf);
}
stream.close(); // 关闭输入流 
```