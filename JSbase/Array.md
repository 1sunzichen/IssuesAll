join()
Array.join() 将数组转成成字符串并连接
  ```
    var a=[1,2,3];
    a.join(); //=> "1,2,3"
    a.join(" ")//=>"1 2 3"
    a.join("")//=>"123"
    var b=new Array(10)//=> 长度为10的空数组
    b.join("-") //=> "---------" 9个连字号组成的字符串
  ```
reverse()
Array.reverse()方法将 数组中的元素颠倒顺序 返回逆序的数组
  ```
  var a=[1,2,3];
  a.reverse().join()//=>"3,2,1" 并且现在的a 是 [3,2,1]
  ```
sort()
Array.sort()数组排序 默认按照字母表的方式排序 
  ```
  var a=new Array("banana","cherry","apple");
  a.sort();
  var s=a.join(",")//s=="apple,banana,cherry"
  ```
concat()
Array.concat()方法创建并返回一个数
```
  var a=[1,2,3];
  a.concat(4,5);//[1,2,3,4,5]
  a.concat([4,5]);//[1,2,3,4,5]
  a.concat([4,5],[6,7])//[1,2,3,4,5,6,7]
  a.concat(4,[5,[6,7]]) //[1,2,3,4,5,[6,7]] //不会递归扁平化数组的数组
  
```
slice()
Array.slice() 方法返回指定数组的一个片段或子数组。
包含第一个参数 不包含第二个参数的所有数组元素
负数 -1 指定了最后一个元素
正数 1  表示索引1
只能从左向右截取
```
  
  var a=[1,2,3,4,5]
  
  a.slice(0,3)//返回[1,2,3]

  a.slice(3)//返回[4,5]
  
```
