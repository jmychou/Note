1. JavaScript中如果对一变量进行重复定义，则第一次定义之后的变量定义语句等价于赋值语句,如果只定义，未赋值，则该变量值为undefined
```
var c = "hello";
console.log(c);//hello
var c = true;
console.log(c);//true
```

## 函数定义有两种方式，以变量方式var  foo = function () {}定义的函数实际上也是一个全局变量，因此，顶层函数的定义也被视为一个全局变量，并绑定到window对象：
 
比如alert()也是window的一个变量。
```
function foo() {
    alert('foo');
}

foo(); // 直接调用foo()
window.foo(); // 通过window.foo()调用
```

2. JS 的作用域
浏览器就是一个实例化的window对象，是一个全局对象，一个大的function
- 如果定义的变量为全局变量，同时在定义的过层中没有使用var,则该变量会作为全局对象的属性，可以通过访问this（全局对象）的相应属性而获得，也可以通过delete关键字将其从全局对象中删除掉。
```
var e='g1';
f='g2';
this.g='g3';
console.log(this.e)  //undefined
console.log(this.f)  //g2
console.log(this.g)  //g3


delete f;
delete g;
console.log(this.f)  //undefined
console.log(this.g)  //undefined

```

3. JS解析器在执行函数的时候，**会先寻找函数下的所有局部变量**，然后在执行后续语句
```
var name='first';
function test(){
    var name='second';
    console.log(name);
}

test();          //second
console.log(name)  //first     ## 可以正常理解
————————————————————————————————————————————
var name='first';
function test(){
    console.log(name);
     var name='second';   //将定义这一句放下面
}

test();          //undefined
console.log(name)  //first    

##可以看到在执行test()函数时，第一步是先收集name这个局部变量，为什么没有输出second ,是因为初始化操作必须是逐句执行，所以test()函数在执行console.log(name)时，此时解析器只知道有一个未赋值的变量name,所以就打印undefined。

_______________________________________________
相当于
function foo() {
    var y; // 提升变量y的申明
    var x = 'Hello, ' + y;
    alert(x);
    y = 'Bob';
}

```

4. 作用域链
上面的例子说明，在function中定义的变量只具有function范围内的作用域，而且只有一层嵌套，即window是个大的function，里面是一个test的function，同样可以嵌套多层。

- 例1
```
var name='first';
function test(){
    console.log(name);
}

test();   //first 

## 当执行test()函数时，发现test()中，没有name这个局部变量，而test()有在window这个大函数中，所以就回溯到window中寻找name,找到了，所以就打印first了。
——————————————————————————————
var name='first';
function test(){
    name='second';
    console.log(name);
}

test();   //second
console.log(name)  //second  

## 道理与上同，此时修改的是window下的变量name
————————————————————————————————————————————————
```

- 例2
```
 var name="first";
  function test1(){
   var name="second";
    function test2(){
       name="third";        //没有写var
       console.log(name);
    }
    test2();
    console.log(name);
 }
 test1();        // third third
 console.log(name); //first

##  执行到test2时，发现test2()没有这个局部变量，就回溯到test1()中
，找到了，就把test1()的name修改为"third"。
————————————————————————————————————————————————
```

- 例3
```
如果在test2()中，没有用var定义了一个新变量
var name="first";
  function test1(){
   var name="second";
    function test2(){
       nam="third";        //没有写var，定义了一个新变量
       console.log(name);
    }
    test2();
    console.log(name);
 }
 test1();  //second second
 console.log(name);  //first
 console.log(nam);   //third
 
 ##同理，在回溯时，发现test1()中没有，在回溯到window下，也没有，
 这是解析器就给window下隐式定义了一个`nam`变量，此时nam就是全局变量了，属于window了。

```

- 减小全局变量和局部变量冲突，把所有的变量和函数全部绑定在一个全局变量中
```
// 唯一的全局变量MYAPP:
var MYAPP = {};

// 其他变量:
MYAPP.name = 'myapp';
MYAPP.version = 1.0;

// 其他函数:
MYAPP.foo = function () {
    return 'foo';
};
```

- 定义常量 const