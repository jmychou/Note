## Json 语法
- 并列数据用逗号（，）分割
- 映射用冒号（:)
- 并列数据集合（数组）用 [] 表示
- 映射的集合（对象）用{} 表示。

1. JSON语法规则
- 数组(Array)用方括号[] 表示。
- 对象(Object)用大括号{}表示。
- 名称/值对 (name/value) 组合成数组和对象 
1. 名称（name）置于双引号中，值（value）可以是字符串、数值、布尔值、null、对象和数组。 不能是undefined，和函数
2. 字符串不能用单引号，数值不能用八进制或者十六进制


- 并列数据用逗号（，）分割,最后一个名称/值对 不要用逗号。

##JS中解析JSON
- JSON.stringify 用于将JavaScript对象转换成JSON字符串
- JSON.parse  用于将JSON字符串转换为JavaScript对象
```
var str = '{"name": "jmy","age": 26,"friends":["Lucy","Lily","Gwen"]}';

var obj = JSON.parse(str);  //返回一个 JavaScript 对象

console.log(obj);    //Object {name: "jmy", age: 26, friends: Array[3]}

var jsonstr = JSON.stringify(obj);  //返回一个 JSON 字符串

console.log(jsonstr);    //"{"name":"Geoff Lui","age":26,"friends":["Lucy","Lily","Gwen"]}"
```

##JavaScript中解析字符串主要有三种方式
1. 使用eval()  //不推荐使用
eval()函数的参数是一个字符串，其作用是直接执行其中的JS代码
```
var str="console.log('hello')";
eval(str);   //hello
```

- eval解析JSON
```
var str='{"name":"jmy","age":20}';
eval("("+str+")");    //输出解析后的对象  Object {name: "jmy", age: 20}
```

- eval()中在字符串两旁加了括号，否则会报错，因为JSON字符串是被{}包围的，直接放到eval中会被当做语句块
执行，因此要加上括号，使其变成表达式。
```
eval("{}");   //输出undefined
eval("({})");   //输出Object{}
```

2. 使用JSON.parse()
- JSON.parse()可以有第二个参数，是一个函数，此函数有两个参数，name和value,分别表示名称和值。当传入一个
JSON字符串后，JSON的每一组名称/值对都有调用此函数。该函数有返回值，返回值将赋值给当前的name
```
var str='{"name":"jmy","age":20}';
function fun(name,value){ 
console.log(name+" "+value);
return value;
}
JSON.parse(str,fun)

##输出
//name  jmy
// age  20
// [object Object]
第一、二行输出分别是 str 的两个名称/值对。第三行输出是 JSON 最外层的对象本身：没有名称，只有值，值是一个对象。
```

- 这样利用第二个参数，可以在解析JSON字符串的同时对数据做一些处理
```
var str = '{"age": 26, "money": 10000}';

var obj = JSON.parse(str,fun);

function fun(name, value){
    if( name == "age" )
        value = 14;
    return value;
}

console.log(obj); // 输出 Object {age: 14, money: 10000}
```


##JSON序列化--------将JavaScript值（一般是对象或者数组）转换为JSON字符串的过程
**JSON.stringify()**
1. 如果要转化的JavaScript数组，存在不符合JSON的语法的值，例如undefined或者function
则自动忽略，不会输出
- 如果值是数组，而数组里有函数，则转换为null
```
var str={
name:"jmy",
age:[function aa(){return abc;}]
};

JSON.stringify(str);  //结果为："{"name":"jmy","age":[null]}"
```

2. 可以有第二个参数，可以是函数或者数组，

- 如果是函数，每一组名称/值对都会调用此函数，与JSON.parse()类似

{% highlight javascript linenos %}
var obj = {
    name: "jmy",
    age: 26
};

var json = JSON.stringify(obj, fun);

function fun(name, value){
    console.log(name + ":" + value);
    return value;
}

// 输出
// :[object Object]
// name:jmy
// age:26
{% endhighlight %}

- 如果是数组，则只有数组中存在的名称才能被转换，且转换后的顺序与数组中值保持一致
```
var obj = { a: 1, b: 2, c: 3, d: 4 };

var json = JSON.stringify(obj, ["c","b","a"]);

console.log(json);

//输出：
//{"c":3,"b":2,"a":1}
```

- JSON.stringify()第三个参数space是用于排版的
```
var obj = { a: 1, b: 2, c: 3, d: 4 };

var json = JSON.stringify(obj, ["c","b","a"], "\t");

console.log(json);

//输出：
//{
//  "c": 3,
//  "b": 2,
//  "a": 1
//}
```

## 正则表达式
$pattern  正则表达式
$subject 匹配的目标数据 数组或字符串

## 正则函数
1.  preg_match（）  和 preg_match_all()  返回值为匹配的次数
-  preg_match($pattern ，$subject，array &$matches )  只匹配一次，匹配结果放入第三个参数数组中
所以此函数饿的第三个数组的key只能是0或1 

##如果patern中含有模式单元， preg_match matches[0] 内容就是原字符串 subject、matches[1] 为第一个合乎规则的字符串、matches[2] 就是第二个合乎规则的字符串


- preg_match_all() 多次匹配，匹配结果放入第三个参数数组中

2. preg_replace($pattern,$replacement,$subject)  和 preg_filter() 执行一个正则表达式搜索和替换 
pattern在subject中找到匹配目标，然后用replacement替换目标。

- preg_replace( ) 会返回匹配结果，不匹配的也会保留
而preg_filter()只返回匹配的结果

3. preg_grep ( string $pattern , array $input）
- 返回匹配模式的数组条目 ，只匹配不替换，只保留匹配的结果

4. preg_split （）
通过一个正则表达式分隔给定字符串. 返回分割结果数组

5. preg_quote （） 转意需要匹配的字符串中的特殊字符
向其中 每个正则表达式语法中的字符前增加一个反斜线

#元字符
1. 定义原子的筛选方式
- | -> 或运算，匹配|符号左右两边的任意一个
- [] -> 匹配出现在[]中的任意一个原子
- [^ ] ->首先这里^必须和[紧挨着，才表示匹配除了[]里面任意原子的字符 （如[^789]除了789之外）
- [ ^ ]->如果不挨着，就只表示匹配^这种字符了，（如[78^9]匹配 7，8，9 ,^，任一一个出现，就匹配出来）

2. 定义原子的集合，将类似的原子进行归类来进行缩写简化

- . 匹配除换行符之外的任意字符 即[^\n]
- \b 匹配单词的开始或结束 例如 \bhi\b 只匹配hi, ahip 不匹配。
（好像hi后可以有空格，而^hi$匹配不到hi后有空格的）
- \d 匹配任意一个十进制数字 即[0-9]
- \D 匹配任意一个 非 十进制数字 即[^0-9]
- \s 匹配一个 不 可见原子 即[\f\n\r\t\v]
- \S 匹配一个可见原子 即[^\f\n\r\t\v]
- \w 匹配任意一个数字、字母、或者下划线 即[0-9a-zA-Z_]
- \W 匹配任意一个 非 数字、字母、或者下划线 即[^0-9a-zA-Z_]

## 正则语法
1. 界定符
表示一个正则表达式的开始和结束,(两个/) 如 /[0-9]/ 或者$(当需要转意很多/时)
2. 原子
- 可见原子 
键盘输入可见，常见的字符
- 不可见原子
键盘输入不可见 如换行 ，tab ,空格
3. 量词
- {n}: 表示其前面的原子恰好出现了n次 例如连续出现三个数字 \d{3}
- {n,}: 表示其前面的原子最少出现了n次
- {n,m}: 表示其前面的原子最少出现n次，最多出现了m次
- *：   表示匹配0次、1次或者多次其之前的原子，即{0,}
- +:    表示匹配1次或者多次其之前的原子，即{1,}
- ?:    匹配0次，或者1次其之前的原子，即{0,1}  如 _?i 就是匹配全文的“_i”和“i”

- .*连在一起就意味着任意数量的不包含换行的字符

4. 边界控制
- ^ 开始 前面不能有任何字符 空格也不行 如^D 只匹配以D开头的，连续的DD也不行，只会匹配第一个D
- $ 结束 后面不能有任何字符 空格也不行   如D$ 只匹配以D结尾的

5. 模式单元
（）  会将括号里面的正则表达式当作一个整体，一个原子来看待 
例如 [Dd]ung~ Dung~|dung~ (D|d)ung~ 三个效果一样。

## 模式单元
- 总模式
每一个pattern可以看作一个总模式
- 子模式
pattern中的每一个模式单元（...）称为子模式

如 ；$pattern = '/^(\w+)\.(\w+)\.com$/'; 整个pattern为总模式，括号内为子模式

## 模式修正(也可以共用)
- 贪婪匹配（默认）：匹配结果存在歧义时取其长，默认匹配模式，添加在匹配模式的最后面，实例/0-9/
- U 懒惰匹配，匹配结果存在歧义时取其短，实例/0-9/U ,
也可以用？表示，如“*？”会取0个，而“+？”会取1个，“{3,5}？”会取3个

- 例如: .*?就意味着匹配任意数量的重复，但是在能使整个匹配成功的前提下使用最少的重复
a.*?b匹配最短的，以a开始，以b结束的字符串。如果把它应用于aabab的话，它会匹配aab（第一到第三个字符）和ab（第四到第五个字符）

- i 忽略英文字母大小写，如:"/abc/i"可以与abc或aBC或ABc等匹配
- x 忽略正则表达式中的空白，(空格和tab制表符)
- g 全局匹配
- m 将字符串视为多行
- s 让元字符"."匹配包括换行在内的所有字符
```
模式为:$mode="/pr.y/";
要匹配字符串为:$str="pr\ny";
两者不可匹配; . 是除了换行以外的字符可匹配;

修改下模式为:$mode="/pr.y/s";
其中修正符s将\n视为普通字符,即不是换行;
最后两者可以匹配;
```

- A 强制从目标字符串开头匹配
```
$mode="/abc/A";
可以与$str="abcsdfi"匹配,
不可以与$str2="sdsdabc"匹配;
因为$str2不是以abc开头;
```

- D 如果使用$限制结尾字符,则不允许结尾有换行;

```
模式为:$mode="/abc$/";
    可以与最后有换行的$str="adshabc\n"匹配;
元子符$会忽略最后的换行\n;
如果模式为:$mode="/abc/D",
则不能与$str="adshabc\n"匹配,
修正符D限制其不可有换行;必需以abc结尾;
```

# 正向预查和反向预查  简单理解
1. 正向预查 （?=pattern)   匹配pattern前面的值,   即某表达式后面是pattern
例如  'Windows (?=7|8)' 可以匹配windows7 windows8 前面的windows 而匹配不到windowsxp 前的windows


2. 正向预查的非 (?!pattern ) 匹配 不是pattern 前面的值    即某表达式后面不是pattern
例如 'Windows (?！7|8)' 可以匹配到windowsxp 前的windows  即windows后面不是7或者8

3. 反向预查 (?<=pattern)  匹配pattern后面的值 即某表达式前面是pattern
例如 windows7(?<=7) 可以匹配 windows7 ,而windows(?<=7)则不行
**注意** 因为(?<=7)本身不占字符，只是查找一个位置。

4. 反向预查的非  (?<！pattern)  匹配XX的前面不是pattern的值
例如 ；$str4='/\b\w+n(?< !g)\w+\b/';  //n的前面不是g的单词 

## 例如歌词的格式为 ；[时间]xxx内容xxx
1. - 时间匹配 ：前面有一个"[" ,然后时一到多个字符 ，然后是"]"
即：（?<=\[).+?(?=\])
- 歌词匹配  前面有个"]",接着不是"["，然后时0个或多个字符串
-即：(?<=\])(?!\[).* 

2. 匹配html标签中的文本
(?<=<(\w+)>).*(?=<\/\1>)  会匹配比如<b>abcd</b> 中的abcd

##常用正则
1. 非空匹配
.+

2. 匹配只保留两位小数的浮点数
\d+\.\d{2}$

3. 匹配手机号
/^1(3|4|5|7|8)\d{9}$/ 或 /^1[34578]\d{9}$/

4. 匹配邮箱
^\w+(\.\w+)*@\w+(\.\w+)+$

5. 匹配简单URL
^(https?://)?(\w+\.)+[a-zA-Z]+$
