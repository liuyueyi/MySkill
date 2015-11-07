# String

> 本节主要记录PHP string的相关用法

> 场景： 判断一个字符串中是否包含另一个子串，判断字符串是否以另一个字符串开头和结束

1. 正则匹配

    ```php
    $parten = '\_100x100.(jpg|jpeg|png|gif)$\';
    $imgs = 'asdf_100x100.jpg_120x100.jpg';
    preg_match_all($pattern, $imgs, $out);
    var_dump($out);
    if(!empty($out[0][0])){
        echo 'end with';
    } else {
        echo 'not end with';
    }
    ```

    主要是利用函数 `preg_match_all`， 进行正则匹配，然后判断结果中[0][0]元素是否存在

2. strpos 位置

    ```php
    $imgs = 'asdf_100x100.jpg_120x100.jpg';
    $index = strrpos($imgs, "_100x100.jpg"); // 最后出现的位置
    if(strlen($imgs) - $index == 12){
        echo 'ends with';
    } else {
        echo 'not ends with';
    }
    ``` 
    这主要是利用函数 `strrpos($text, $subText)`, 查找最后出现的位置

3. 获得字符串的子串

    `substr($text ,$index, $length); `

4. 字符串分割为数组

    `$ary = explode(',', 'helo,world,nihem,hao');`

5. 数组转字符串

    `$text = implode(',', array('hello', 'world', 123, 456));`

