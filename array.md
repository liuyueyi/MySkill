# Array
> php的数组无比强大，其中可以防止任意的对象

1. 申明：  $items = [];   $items = array();   建议使用前者

2. 判断是否在数组内

	`  $ans = in_array('hello', ['hello', 'world'], true);`
	
	第三个参数表示使用强类型比较，效率会提升很大
	
3. 数组合并

	`$newAry = array_merge((array)$ary1,  (array)$ary2);`
	
	**如果键名有重复，该键的键值为最后一个键名对应的值（后面的覆盖前面的）。如果数组是数字索引的，则键名会以连续方式重新索引。** 注意参数里面，显示申明一下参数为array类型
	
	```
	$array1 = array("color" => "red", 2, 4);
	$array2 = array("a", "b", "color" => "green", "shape" => "trapezoid", 4);
	$result = array_merge($array1, $array2);
	print_r($result);
	/* //输出：
	Array
	(
    [color] => green
    [0] => 2
    [1] => 4
    [2] => a
    [3] => b
    [shape] => trapezoid
    [4] => 4
	)
	*/
	```
	
4. 数组处理 `array_map`

	轮循处理数组中的元素，返回的东东组成一个新的数组
	
	```
	$ary = [
		['id' => 1230],
		['id' => 2345],
		['id' => 3343]
	];
	// 将二维数组中的内容提取上一层
	$res = array_map(function($id){
		return $id['id'];
	}, $ary)
	print_r($ans);
	/* // 输出
	Array
	(
    [0] => 1230
    [1] => 2340
    [2] => 3343
	)
	*/
	```
	
5. 数组分割为新的数组块 `array_chunk`

	函数把一个数组分割为新的数组块，其中每个数组的单元数目由 size 参数决定。最后一个数组的单元数目可能会少几个
	
	```
	array_chunk(array,size,preserve_key);
	- array: 必需，规定要使用的数组
	- size: 必需，规定每个新数组中包含多少个元素
	- preserve_key: bool true 则保留原始数组中的键名； false  默认。每个结果数组使用从零开始的新数组索引
	```
