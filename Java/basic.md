# Basic Class
> java基础类库的学习

## 系统相关
> 如何获取平台相关的属性，调用平台相关的命令（如shell），Java提供System和Runtime类来实现程序和平台指尖的交互

###1. System 类

#### - 说明：
提供了标准输入输出、错误输出，访问系统环境变量，加载文件和动态链接库的方法

#### - 访问系统属性、环境变量
```java
public class Test{
    public static function main(String[] args){
        // 获取系统所有的环境变量
        Map<String, String> env = System.getenv();
        // 获取系统指定变量的值
        String jpath = System.getenv("JAVA_HOME");
        // 获取系统所有属性
        Propertes props = System.getProperties();
        // 保存所有属性值到指定的file中
        props.store(new FileOutputStream("props.txt"), "System properteis);
        // 获得特定的系统属性
        String osName = System.getProperties("os.name");
    }
}
```