#数据类型
- 根据ECMAScript 5.1的规范，javascript中共有六种数据类型
Undefined,Null,Boolean,Number,String,Object  前5种属于基本类型，最后一种属于对象类型

- Undefined 类型只有一个值，为undefined，适用于所有数据类型
没有初始化的变量，函数中缺失的参数，函数没有显示return值时都为undefined
- Null 类型只有一个值，为null,意味着"空对象(no object)" ,只适用于对象类型
- Boolean 类型有两个值，true 与 false
- Number 类型还包括三个特殊的值 NaN,Infinity,-Infinity

**基本类型的string与对象类型的String的区别**
- 使用字面量方式创建的字符串，为基本类型的string
-  使用String()创建的字符串，为基本类型的string
-  使用new String()方式创建的字符串，为object
```
str1 = "javascript"
str2 = String("javascript")
str3 = new String("javascript")
> typeof str1
"string"
> typeof str2
"string"
> typeof str3
"object"
```

- 当字符串对象与基本类型的string比较的时候，是按值比较，而与字符串对象比较的话，按引用比较
```
var s = new String('hello world');
var s1 = new String('hello world');
var s2 = String('hello world');

console.log( s == s2 );    //true
console.log( s == s1 );    //false
```

- javascript会在合适的时候自动把基本类型的string转为对象类型的string，也就是说我们可以对基本类型string使用String.prototype中的方法。这两者也可以进行显式转化。
```
// 基本类型----->对象类型
str1 = "javascript"
str1 = new String(str1)
> typeof str1
"object"
// 对象类型----->基本类型
str1 = new String("javascript")
str1 = str1.valueOf()
> typeof str1
"string"
```

- 这两者用在eval函数中时，结果有所区别：
```
var s1="2+2";
eval(s1);    //4

var s2=new String("2+2");
eval(s2)   //[String : "2+2"]    返回的还是对象
```

## && 与 ||
- && 如果&&左侧的表达式为真值，则返回右侧表达式的值，否则返回左侧表达式的值
```
var i="" && 1;     //""
i=1 && 2;    //2
i=1 && "" ;   //""

i=0 && "";    //0
```

- ||  如果||左侧表达式为真值，则返回左侧表达式的值，否则返回右侧表达式的值
```
var i="" || 1;   //1 
i=1 || 2；;     //1 
i=1 || "";       //1 

i=0 ||"" ;      //""

```

1. Number js不区分整数和浮点数，统一用Number表示，特殊的Number类型
- NaN  //NaN表示Not a Number ，当无法计算结果时用NaN表示
- Infinity   //Infinity 表示无限大，当数值超过了JavaScript的Number所能表示的最大值时，就表示为Infinity

2. 字符串
字符串可以用'' 或者 "" 表示。

3. 比较运算符
```
javaScript在设计时，有两种比较运算符：

第一种是==比较，它会自动转换数据类型再比较，很多时候，会得到非常诡异的结果；

第二种是===比较，它不会自动转换数据类型，如果数据类型不一致，返回false，如果一致，再比较。

由于JavaScript这个设计缺陷，【不要使用==比较，始终坚持使用===比较】。
例如
false==0  //true 
false====0 //false

`!!` 一般用来将后面的表达式转换为布尔型的数据（boolean）
1. var abc="true";
typeof(!! abc);   //boolean

2. var tt={flag:true};
  var test=!!tt.flag;

  等价于test=tt.flag || false 
  即如果tt设置了flag的值，如果flag的值不是(null、undefined、0、"") ,则test的值就为true
  而如果没有flag这个属性，或是以上各值，test就为false。
```

## NaN这个特殊的Number与其他值都不相等，包括它自己
- NaN===NaN   //false 

## 唯一能判断NaN的方法是通过isNaN() 函数
- isNaN(NaN); // true

4. null和undefined
- null 表示一个空值 undefined表示未定义
大多数情况用null,undefined仅仅在判断函数参数是否传递时使用

- null和undefined 的比较
1. typeof
2. 全等 ===
```
undefined==null;     //true

typeof undefined ==typeof null ;   //false  (typeof(undefined); 输出undefined typeof(null);输出objet)


undefined === null;    //false
```

3. 转换为数字
```
Number(null);     //0

5+null;    //5

Number(undefined);  //NaN

5+undefined;    //NaN
```

4. JSON 转换
```
>JSON.parse(null);
null

>JSON.parse(undefined);
错误

>JSON.stringify(null);
"null"

>JSON.stringify(undefined);

undefined

```




5. 数组
1. JavaScript的数组可以包括任意数据类型。例如：
[1, 2, 3.14, 'Hello', null, true];
2. 另一种创建数组的方法为 Array(),省略new 也可以。
new Array(1,2,3)   //创建数组[1,2,3]

## 建议直接用[]

6. 对象
JavaScript的对象是一组由键-值组成的无序集合，例如
```
var person={
    name: 'jmy',
    age:20,
    tags:['js','web'],
    zipcode:null 
}
```

- 获取对象属性，用person.name

7. strict模式
如果一个变量没有用`var`声明，则该变量自动被申明为全局变量，所以为了避免这一缺陷，应该使用strict模式。

- 在strict模式下，强制使用`var`声明变量，如果未使用，则会报错（ReferenceError错误）
```
'use strict';
// 如果浏览器支持strict模式，
// 下面的代码将报ReferenceError错误:

abc='hello ,word';
alert(abc);

```

# 字符串
新的标准可以这样写多行文本；（如果不支持，将报SyntaxError错误；用\n表示）
```
alert(`多行
字符串
测试`);

//打印 
多行
字符串
测试
```

##字符串常见操作
1. 
var s = 'Hello, world!';
s.length; // 13

2. 要获取字符串某个指定位置的字符，可以使用下标操作
```
var s = 'Hello, world!';

s[0]; // 'H'
s[6]; // ' '
s[7]; // 'w'
s[12]; // '!'
s[13]; // undefined 超出范围的索引不会报错，但一律返回undefined
```

3. 需要特别注意的是，字符串是不可变的，如果对字符串的某个索引赋值，不会有任何错误，但是，也没有任何效果：
```
var s = 'Test';
s[0] = 'X';
alert(s); // s仍然为'Test'
```

4. 
- toUpperCase()把一个字符串全部变为大写： 
- toLowerCase()把一个字符串全部变为小写：
- indexOf()会搜索指定字符串出现的位置：
```
var s = 'hello, world';
s.indexOf('world'); // 返回7
s.indexOf('World'); // 没有找到指定的子串，返回-1
```

- substr (start[,length])方法返回一个从指定位置开始的指定`长度`的子字符串。
如果 length 为 0 或负数，将返回一个空字符串。如果没有指定该参数，则子字符串将延续到字符串的最后。



- substring()返回指定索引区间的子串：
substring 方法使用 start 和 end 两者中的较小值作为子字符串的起始点。例如，substring(0, 3) 和substring(3, 0) 将返回相同的子字符串。如果 start 或 end 为 NaN 或者负数，那么将其替换为0。

```
var s = 'hello, world'
s.substring(0, 5); // 从索引0开始到5（不包括5），返回'hello'
s.substring(7); // 从索引7开始到结束，返回'world'
```

#数组
- 两种方式定义数组
1. var s = [1, "ctrip"];
2. var a = new Array(1, "ctrip");

JavaScript的数组可以包含任意数据类型。
1. 取得数组的长度，用arr.length
**直接给Array的length赋一个值，会导致Array大小的变化**
```
var arr = [1, 2, 3];
arr.length; // 3
arr.length = 6;
arr; // arr变为[1, 2, 3, undefined, undefined, undefined]
arr.length = 2;
arr; // arr变为[1, 2]
```

2. Array可以通过索引赋值修改相应的元素，所以就改变了这个数组 
**如果通过索引赋值时，索引超过了范围，也会导致Array大小的变化**
Array可以通过arr[1]或者arr["1"]都可以取值，而随便给Array一个字符串索引，可以读出来，但是数组里没有，相当于额外的一个属性
```
var test=new Array(1,2,3);
test[1] //2
test["1"] //2

test["aa"]="test"; 
test;   //[1,2,3]
test["aa"]   //test
test.aa   //test

var arr = [1, 2, 3];
arr[5] = 'x';
arr; // arr变为[1, 2, 3, undefined, undefined, 'x']
```

- 可以用delete操作符删除数组的元素,注意这个删除仅仅是将数组在该位置的元素设为undefined,数组的长度并没有改变.

##数组常用方法
1. indexOf()  搜索元素的位置
```
var arr = [10, 20, '30', 'xyz'];
arr.indexOf(10); // 元素10的索引为0
arr.indexOf(20); // 元素10的索引为1
arr.indexOf(30); // 元素30没有找到，返回-1
arr.indexOf('30'); // 元素'30'的索引为2
```

2. slice()      类似String的substring();截取数组部分元素，然后返回一个新的Array
```
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
arr.slice(0, 3); // 从索引0开始，到索引3结束，但不包括索引3: ['A', 'B', 'C']
arr.slice(3); // 从索引3开始到结束: ['D', 'E', 'F', 'G']
```

- 如果不给slice()传递任何参数，它就会从头到尾截取所有元素。利用这一点，我们可以很容易地复制一个Array：
```
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
var aCopy = arr.slice();
aCopy; // ['A', 'B', 'C', 'D', 'E', 'F', 'G']
aCopy === arr; // false
```

- 如果slice(start,end)，end超过数组长度，会截取到数组最后一个元素，end为-1的话，为最后一个元素
```
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
arr.slice(5,9)   //["F", "G"]
arr.slice(-2,-1)  //['F']

arr.slice(1)    //["B", "C", "D", "E", "F", "G"]
arr.slice(-1)   //["G"]
```

3. push() 和 pop()
push()想Array的尾部添加若干元素，pop()则把Array的最后一个元素删除掉
```
var arr = [1, 2];
arr.push('A', 'B'); // 返回Array新的长度: 4
arr; // [1, 2, 'A', 'B']
arr.pop(); // pop()返回'B'
arr; // [1, 2, 'A']
arr.pop(); arr.pop(); arr.pop(); // 连续pop 3次
arr; // []
arr.pop(); // 空数组继续pop不会报错，而是返回undefined
arr; // []
```

4. unshift() 和shift()
unshift()向Array的头部添加若干元素，shift()把Array的第一个元素删掉
```
var arr = [1, 2];
arr.unshift('A', 'B'); // 返回Array新的长度: 4
arr; // ['A', 'B', 1, 2]
arr.shift(); // 'A'
arr; // ['B', 1, 2]
arr.shift(); arr.shift(); arr.shift(); // 连续shift 3次
arr; // []
arr.shift(); // 空数组继续shift不会报错，而是返回undefined
arr; // []
```

5. sort()  sort()可以对当前Array进行排序，它会直接修改当前Array的元素位置,
如果是字符串，会根据ASCII码进行排序
此方法会对数组的每一项进行toString(),所以最后得到的数组每一项字符串，所以会有以下结果
```
var arr=[1,2,3,15];
arr.sort()  # 结果为1,15,2,3
```

- 但sort()可以接受一个函数，所以可以写一个比较函数，可以避免此情况

```
function compare(v1,v2){ 
    if(v1<v2){ 
        return -1;
    }else if(v1>v2){ 
        return 1;
    }else return 0;
}
var arr=[1,2,3,15];
arr.sort(compare);  #结果为1,2,3,15
```

- 默认情况下，对字符串排序，是按照ASCII的大小比较的，现在，我们提出排序应该忽略大小写，按照字母序排序。
```
var arr = ['Google', 'apple', 'Microsoft'];
arr.sort(function (s1,s2){ 
    x1=s1.toUpperCase();
    x2=s2.toUpperCase();
    if(v1<v2){ 
        return -1;
    }else if(v1>v2){ 
        return 1;
    }else return 0;
    })   //['apple', 'Google', 'Microsoft']
```

- sort()方法会直接对Array进行修改，它返回的结果仍是当前Array：
```
var a1 = ['B', 'A', 'C'];
var a2 = a1.sort();
a1; // ['A', 'B', 'C']
a2; // ['A', 'B', 'C']
a1 === a2; // true, a1和a2是同一对象
```

6. reverse() 反转数组

7. splice() 是修改Array的“万能方法”，它可以从指定的索引开始删除若干元素，然后再从该位置添加若干元素：
```
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
// 从索引2开始删除3个元素,然后再添加两个元素:
arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']

// 只删除,不添加:
arr.splice(2, 2); // ['Google', 'Facebook']
arr; // ['Microsoft', 'Apple', 'Oracle']

// 只添加,不删除:
arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
```

8. concact() 把当前的Array和另一个Array连接起来，并没有修改原数组，而是返回一个新的Array：
```
var arr = ['A', 'B', 'C'];
var added = arr.concat([1, 2, 3]);
added; // ['A', 'B', 'C', 1, 2, 3]
arr; // ['A', 'B', 'C']

实际上，concat()方法可以接收任意个元素和Array，并且自动把Array拆开，然后全部添加到新的Array里：
var arr = ['A', 'B', 'C'];
arr.concat(1, 2, [3, 4]); // ['A', 'B', 'C', 1, 2, 3, 4]
```

9. join 把当前Array的每个元素都用指定的字符串连接起来，然后返回连接后的字符串：
如果不指定分隔符，默认为逗号（，）
```
var arr = ['A', 'B', 'C', 1, 2, 3];
arr.join('-'); // 'A-B-C-1-2-3'
```

10. 数组转换为字符串

所有对象都有toLocalString()、toString这2个方法返回以逗号分隔的字符串，它们的原理分别是循环数组，将数组的每一项分别使用toLocalString()、toString()方法后，拼接成字符串,输出一个新数组。
如果数组中的一项值是null，或者undefined，使用join()，toLocalString()、toString()方法返回的是以空字符串表示该项。


##对象
- 创建对象的两种方式
1. var obj={};
2. var obj=new Object();

- 对象的constructor属性
每一个javascript对象都有一个constructor属性.这个属性对应了对象初始化时的构造函数(函数也是对象).
```
var date = new Date();
alert(date.constructor);  //function Date(){};
alert(date.constructor == "Date");  //false
alert(date.constructor == Date);  //true
```

- JavaScript用一个{...}表示一个对象，键值对以xxx: xxx形式申明，用`,`隔开。注意，最后一个键值对不需要在末尾加`,`
如果属性名包含特殊字符，就必须用''括起来：
```
var xiaohong = {
    name: '小红',
    'middle-school': 'No.1 Middle School'
};

//xiaohong的属性名middle-school不是一个有效的变量，就需要用''括起来。访问这个属性也无法使用.操作符，必须用['xxx']来访问：
普通属性也可以用['xx']访问,并且['xxx']里还可以写一些表达式。

xiaohong['middle-school']; // 'No.1 Middle School'
xiaohong['name']; // '小红'
xiaohong.name; // '小红'
```

- 属性可以是数组或者函数
```
 var delivery = {
          no: 1,
          sendtime: "2014-11-25",
  
          //三个可用配送地址
         address: ["北京", "上海", "南京"],
  
          way: "PJS",
  
         //根据逻辑是否支持配送
         cansend: function () {
             //logic code....
             return true;
         }
     };
```

- 访问不存在的属性不报错，而是返回undefined：

- 由于JavaScript的对象是动态类型，你可以自由地给一个对象添加或删除属性,删除属性用delete
```
var xiaoming = {
    name: '小明'
};
xiaoming.age; // undefined
xiaoming.age = 18; // 新增一个age属性
xiaoming.age; // 18

delete xiaoming.age; // 删除age属性
xiaoming.age; // undefined
delete xiaoming['name']; // 删除name属性
xiaoming.name; // undefined
delete xiaoming.school; // 删除一个不存在的school属性也不会报错
```

- 要检测一个对象是否拥有某个属性，可以用`in`操作，不过此属性不一定是该对象的，也可能是继承的
所以要判断一个属性是否是xiaoming自身拥有的，而不是继承得到的，可以用`hasOwnProperty()`方法：
```
var xiaoming = {
    name: '小明'
};

'name' in xiaoming; // true
'grade' in xiaoming; // false
'toString' in xiaoming; // true


xiaoming.hasOwnProperty('name'); // true
xiaoming.hasOwnProperty('toString'); // false
```

## JavaScript把null、undefined、0、NaN和空字符串''视为false，其他值一概视为true.


##JavaScript 中的foreach  for.....in
- 对象
```
var o = {
    name: 'Jack',
    age: 20,
    city: 'Beijing'
};
for (var key in o) {
    alert(key); // 'name', 'age', 'city'
    alert(o[key]); //Jack ,20 ,Beijing
}
```

- 数组 由于Array也是对象，而它的每个元素的索引被视为对象的属性，因此，for ... in循环可以直接循环出Array的索引：
请注意，for ... in对Array的循环得到的是String而不是Number。
```
var a = ['A', 'B', 'C'];
for (var i in a) {
    alert(i); // '0', '1', '2'
    alert(a[i]); // 'A', 'B', 'C'
}
```

## Map和Set




#函数  如果没有return语句，函数执行完毕后也会返回结果，只是结果为undefined。
**函数定义的三种方式**
1. 函数声明方式
- 这种方式，会被解析器执行函数声明提升，所以`即使在函数定义前调用该函数也不会报错`
```
alert(sum(3,4));   //7
function sum(x,y){ 
    return x+y;
}
```

2. 函数表达式方式
- 除了不能定义前调用外，和函数声明一样
```
alert(sum(3,4));   //此种方式会报错
var sum=function (x,y){ 
    return x+y;
}
```

3. 函数对象方法
- 以对象的方式定义函数，前面参数为函数的参数，最后为函数体，但是会造成解析器两次解析
一次是普通的ECMAScript代码，一次是解析传入构造函数里的字符串，所以会影响性能，不推荐
```
var sum=new Function('x','y','return x+y');
```


- 由于JavaScript的函数也是一个对象，下述定义的abs()函数实际上是一个函数对象，而函数名abs可以视为指向该函数的变量。
所以下列两种方式等价
1. 
```
function abs(x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```

2. 在这种方式下，function (x) { ... }是一个匿名函数，它没有函数名。但是，这个匿名函数赋值给了变量abs，所以，通过变量abs就可以调用该函数；需要在末尾加`;`号
```
var abs = function (x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
};

```

- JS允许传入多个参数，而不影响调用，所以传入的参数比定义的多也没问题
```
abs(10, 'blablabla'); // 返回10
abs(-9, 'haha', 'hehe', null); // 返回9
```

- 传入的参数少也没问题
abs(); // 返回NaN
此时abs(x)函数的参数x将收到undefined，计算结果为NaN。
要避免收到undefined，可以对参数进行检查：
```
function abs(x){
    if(typeof x !='number'){
    throw 'not a Number ';
    }
     if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```

## 函数内部还有一个arguments关键字
arguments只在函数内部起作用，指向函数调用时传入的全部参数，类似Array而不是Array
1. 可以输出传入的参数比定义的参数多的  其他参数
```
function foo(x) {
    alert(x); // 10
    for (var i=0; i<arguments.length; i++) {
        alert(arguments[i]); // 10, 20, 30
    }
}
foo(10, 20, 30);

##foo() 只定义了一个参数，而arguments可以取得所有传入的参数。
```

2. 即使函数不定义参数，也可以取得参数的值
```
function abs() {
    if (arguments.length === 0) {
        return 0;
    }
    var x = arguments[0];
    return x >= 0 ? x : -x;
}

abs(); // 0
abs(10); // 10
abs(-9); // 9
```

- 所以arguements最常用于判断传入参数的个数 arguments.length

- 新标准的rest参数

## 注意return 
- JavaScript引擎有一个在行末自动添加分号的机制，这可能让你栽到return语句的一个大坑：
```
function foo() {
    return { name: 'foo' };
}

foo(); // { name: 'foo' }
```

如果把return 拆为两行
```
function foo() {
    return
        { name: 'foo' };
}

foo(); // undefined
```
由于JavaScript引擎在行末自动添加分号的机制，上面代码等同于
```
    function foo() {
    return; // 自动添加了分号，相当于return undefined;
        { name: 'foo' }; // 这行语句已经没法执行到了
}
```

所以正确的多行写法为
```
function foo() {
    return { // 这里不会自动加分号，因为{表示语句尚未结束
        name: 'foo'
    };
}
```

## 方法 ---在一个对象的属性中绑定方法
```
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var y = new Date().getFullYear();
        return y - this.birth;
    }
};

xiaoming.age; // function xiaoming.age()
xiaoming.age(); // 今年调用是25,明年调用就变成26了
```

- JS 的函数内部如果调用了this,而这个this的指向，视情况而定
1. 如果以对象的方式调用，比如`xiaoming.age()`,则该函数的this指向被调用的对象，也就是`xiapming`,
2. 如果单独调用函数，比如`getAge()`,此时this指向全局对象，即 `window`。

```
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25, 正常结果
getAge(); // NaN

——————————————————————————————————————
并且如果这么写，
var tt=xiaoming.age;
tt();   //得到也是NaN
```

- 要保证this指向正确，必须用`obj.xxx()`的形式调用。

- 在函数内部重构函数，也会出错
```
'use strict';

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        function getAgeFromBirth() {
            var y = new Date().getFullYear();
            return y - this.birth;
        }
        return getAgeFromBirth();
    }
};

xiaoming.age(); // Uncaught TypeError: Cannot read property 'birth' of undefined
```

- 因为this指针，只在age()方法的函数内指向`xiaoming`,而在函数内部 嵌套定义的函数，this又指向undefined，
(在非strict模式下，它重新指向全局对象window)
所以需要先用一个其他的变量捕获this
```
'use strict';

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var that = this; // 在方法内部一开始就捕获this
        function getAgeFromBirth() {
            var y = new Date().getFullYear();
            return y - that.birth; // 用that而不是this
        }
        return getAgeFromBirth();
    }
};

xiaoming.age(); // 25
```

##apply()
- 调用一个对象的一个方法，以另一个对象来替换当前对象
- apply() 和call() 将一个对象的初始上下文转化为thisObj所指定的对象.
如果没有提供thisObj,则Global对象被用做thisObj.
- 要指定函数的this指向哪个对象，可以用函数的apply()方法，它接受两个参数，第一个参数就是需要绑定的
`this`变量，第二个参数是Array,表示函数的本身的参数。上面的例子可以改为
```
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空
```

##call()
- call()和apply()类似，区别是
1. apply()第二参数必须是Array，即把参数打包成Array再传入
2. call()把参数按顺序传入
```
Math.max(3,4,5);     //5
Math.max.apply(null,[3,4,5]);   //5,如果传入的不是Array就会出错
Math.max.call(null,3,4,5)   //5; 如果传入的是数组，返回NaN

##普通函数调用，通常把this绑定为null
```

- 利用apply(),还可以动态改变函数行为
现在假定我们想统计一下代码一共调用了多少次parseInt()，可以把所有的调用都找出来，然后手动加上
count += 1，不过这样做太傻了。最佳方案是用我们自己的函数替换掉系统的默认函数parseInt()：

```
var count = 0;
var oldParseInt = parseInt; // 保存原函数

//修改系统自带的parseInt
window.parseInt = function () {
    count += 1;
    return oldParseInt.apply(null, arguments); // 调用原函数
};

// 测试:
parseInt('10');
parseInt('20');
parseInt('30');
count; // 3
```

##高阶函数
一个函数的参数为另一个函数  即高阶函数
```
function add(x, y, f) {
    return f(x) + f(y);
}
add(-5,6,Math.abs)  //11

计算过程如下：
x = -5;
y = 6;
f = Math.abs;
f(x) + f(y) ==> Math.abs(-5) + Math.abs(6) ==> 11;
return 11;
```

##Array中的Map方法
- 调用Array的Map()方法，传入自定义函数，就可以得到一个新的Array作为结果，
作用是处理Array，并返回一个`Array`
```
function pow(x) {
    return x * x;
}

var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81]
```

- 还可以把Array中的数字转换为字符串
```
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
arr.map(String); // ['1', '2', '3', '4', '5', '6', '7', '8', '9']
```


##Array中的reduce方法
- Array中的reduce方法把一个函数作用于Array的元素上，传入reduce的函数必须接受两个参数，
然后，reduce()把结果继续和Array中的下一个元素做累积计算，效果为：
> [x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4)
作用是处理Array中的元素，然后返回一个结果
例如对一个Array求和
```
var arr = [1, 3, 5, 7, 9];
arr.reduce(function (x, y) {
    return x + y;
}); // 25


##把[1,3,5,7,9]转换成整数 13579
var arr = [1, 3, 5, 7, 9];
arr.reduce(function (x, y) {
    return x*10 + y;
}); // 13579
```

- 把字符串转为int   先把字符串13579，转换为Array(1,3,5,7,9,),然后在转换为数字13579
```
'use strict';

function string2int(s) {
    return s.split('')
      .map(function(x){ return x.charCodeAt(0) - '0'.charCodeAt(0);})
      .reduce(function(x, y){ return x*10+y; });
}

// 测试:
if (string2int('0') === 0 && string2int('12345') === 12345 && string2int('12300') === 12300) {
    if (string2int.toString().indexOf('parseInt') !== -1) {
        alert('请勿使用parseInt()!');
    } else if (string2int.toString().indexOf('Number') !== -1) {
        alert('请勿使用Number()!');
    } else {
        alert('测试通过!');
    }
}
else {
    alert('测试失败!');
}
```

##Array中的filter方法
- filter也接受一个函数，把传入的函数依次作用于Array中每个元素，然后根据返回值是true还是false,
决定保留还是舍弃元素，功能是从Array中筛选出一些元素，并返回。

1. 在一个Array中，删掉偶数，只保留奇数，可以这么写：
```
var arr = [1, 2, 4, 5, 6, 9, 10, 15];
arr.filter(function (x){ 
    return x%2!=0;
    })  //[1,5,9,15]

```

2. 把一个Array中的空字符串删掉，可以这么写：
```
var arr = ['A', '', 'B', null, undefined, 'C', '  '];
arr.filter(function (s) {
    return s && s.trim(); // 注意：IE9以下的版本没有trim()方法
}); // ['A', 'B', 'C']
```

--- 

