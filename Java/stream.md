# Intput&Output Stream
> Java的输入输出流

## 1. File
> 文件类

### 1. 常用方法
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

### 2. 文件过滤器
list方法中可以接受一个FilenameFilter函数接口，进行过滤
```

```