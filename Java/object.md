# object study
> 对象的学习笔记

### 1. function

方法，类似c&c++的函数，但是只能放在类中，单独提来是因为： <font color="red">方法的参数都是值传递</font>

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

上面输出的结果实际是`