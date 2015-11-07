# Properties
> 基本类，用于处理文件存储键值对，其中健和值用等号分隔
>
> 将资源装载到Properties对象中，每项内容都当做String

```
Properties prop = new Properties();
FileInputStream fis = new FileInputStream("test.properties");
prop.load(fis);
prop.list(System.out);
```
- 上面显示了装载配置文件的用法，首先是获取文件输入流，然后load到Properties中，对象即初始化完毕

- list函数，将Properties中的键值对打印到指定的输出流
	`prop.list(System.out)`
	
- `getProperty(key, defaultValue);` 获取key对应的value值

- `stringPropertyNames()`  获取所有的keys， string值

