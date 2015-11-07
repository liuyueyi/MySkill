# object study
> 对象的学习笔记

## 一. 方法 function

> 方法，类似c&c++的函数，但是只能放在类中

### 1. 传参方式都是值传递
单独提来是因为： <font color="red">方法的参数都是值传递</font>

关于上面这一点，肯定会有疑问，不都是引用传参嘛？！！

话不多说，看实例:

```java
import java.util.*;

public class Test{
    public void printA(StringBuffer text) {
        System.out.println(text.toString());
        text.append( " hello world");
        text = null;
    }   

    public static void main(String[] args){
        Test tt = new Test();
        StringBuffer buffer = new StringBuffer("nihao");
        tt.printA(buffer);
        System.out.println(buffer);
    }  
```
上面的代码很简单，printA传入一个StringBuffer类型的参数，首先是打印出值，并追加一个字符串，随后将形参设置为null

上面输出的结果实际是
```
nihao
nihao hello world
```
**解释：** 从上面的输出其实可以很清晰的看出，传过去的其实是值，即传过去的是buffer的一个拷贝，一个和buffer指向同一堆内数据的引用text，在printA函数中，修改堆内数据，那么指向该堆数据的buffer输出的值当然会改变；而引用text本身发生改变，是不会影响到buffer的，即text执行空的时候，buffer并不会指向空

### 2. 形参个数可变
java也支持可变参数

**使用： ** 最后一个参数类型后加..., 表名该形参可以接受多个参数值，且多个参数值是作为数组的形式传入进去的

实例：
```java
public class Varags {
    public satic void test(int a, String ... books) {
        for(String book : books) {
            System.out.println(book);
        }
    }
    
    public static void main(String[] args) {
        test(5, "english book", "cook book");
    }
}
```