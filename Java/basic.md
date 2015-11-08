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
        String osName = System.getProperty("os.name");
    }
}
```
通过上面的实例，可以看出主要用的是`System.getenv(); System.getProperty();`

#### - 其他

1. 资源回收 ： 

  `System.gc(); System.runFinalization();`

2. 获取时间戳:
    
  微秒： `System.currentTimeMillis(); `

  纳秒： `System.nanoTime();`

3. hashCode

  hashCode是根据对象的地址进行计算得到的值，如果一个类的hashCode被重写后，该类实例的hashCode方法就不能唯一标识该对象；
  但是通过`identityHashCode()`方法返回的hashCode值， 依然是根据改对象的地址计算的，如果两个对象的identityHashCode值相同，则表示两个对象绝对是同一个
  
  `System.identityHashCode(obj1);`
  
###2. Runtime 类
#### - 说明：
表示java程序的运行时环境，每个java程序都有一个与之对应的Runtime实例，程序不能创建，只能通过getRuntime()方法获取与之关联的Runtime对象

