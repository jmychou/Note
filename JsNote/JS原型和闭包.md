##typeof()  typeof是操作符，不是方法
```
  function show(x) {

            console.log(typeof(undefined));  //undefined  
            console.log(typeof(x));    // undefined
            console.log(typeof(10));   // number
            console.log(typeof('abc')); // string
            console.log(typeof(true));  // boolean

            console.log(typeof(function () { }));  //function

            console.log(typeof([1, 'a', true]));  //object
            console.log(typeof ({ a: 10, b: 20 }));  //object
            console.log(typeof (null));  //object
            console.log(typeof (new Number(10)));  //object
        }
        show();
```

1. 其中上面的四种,(undefined,number,string,boolean)属于值类型，不是对象
2. 其余的(函数，数组，对象，null,new Number(10))都是对象，属于引用类型

##判断一个变量是不是对象，值类型的类型判断用typeof，引用类型的类型判断用instanceof
```
var fn=function(){};
console.log(fn instanceof Object)  //true
```

##JS中 数组和function也是对象， fuction和object既有对象的特征也有数组的特征，对象是属性的集合

##jQuery中的 $ 其实就是一个函数，它下面的函数，比如$. irm()其实就是在$上加了一个trim属性，属性值是函数
- 类似于下面
```
typeof($);    //function

 var fn = function () {
            alert(100);
        };
        fn.a = 10;
        fn.b = function () {
            alert(123);
        };
```

##对象都是通过函数创建的
```
var obj={a:20,b:20};
var arr=[5,'x',true];

//以上代码的实质是
var obj = new Object();
        obj.a = 10;
        obj.b = 20;

var arr = new Array();
        arr[0] = 5;
        arr[1] = 'x';
        arr[2] = true;

并且 console.log(typeof (Object));  // function
     console.log(typeof (Array));  // function
```


##执行上下文  (声明提前)
1. 第一种情况

```
1. console.log(a);    //错误，a is not defined

2. console.log(a);     //undefined
   var a;

3. console.log(a);    //undefined
    var a=10;

————————————————
2例和3例都是声明提前，浏览器会先查找变量的声明(不是赋值) 所以3例等同于
var a;
console.log(a);
a=10;

```

2. 第二种情况

```
console.log(this);    //Window {top: Window, location: Location, document: document, window: Window,}


## 此种情况直接对this赋值了
```

3. 第三中情况---函数定义

```
1. 函数声明

console.log(f1);    //此时会打印函数f1   function f1(){}
function f1(){};

2. 函数表达式
console.log(f2);    //undefined   
var f2=function () {};

————————————————
函数声明类似于第二种情况，而函数表达式类似于第二种情况，就如对待`var a=10;` 只会声明提前，而不会赋值。
```

**以上三种情况，就是执行上下文。**
1. 变量、函数表达式  -----声明提前，默认值为undefined
2. this  ----- 赋值
3. 函数声明 -----赋值

#this
**每次调用函数，就会产生一个上下文环境，而this的取值是是执行上下文环境的一部分，this取什么值，是在函数调用的时候确定的，而不是函数定义的时候确定的，所以this指向调用该函数的对象**

- this的几种情况

1. 构造函数
this就代表new出来的对象

```
当定义的函数是为了要new一个对象时，(构造函数的函数名，第一个字母大写，例如Object，Array,Function等)

function Foo(){ 
    this.name='jmy';
    this.age=20;

    console.log(this);   //Foo{ name :'jmy', age:20}
}

var f1=new Foo();  //此时Foo()就作为构造函数
console.log(f1.name);    //jmy
console.log(f1.age);     //20
__________________________________
如果直接调用Foo()函数，而不是new Foo() ，this就指向window ，因为此时Foo()函数是作为windowd的一个属性

function Foo(){ 
    this.name='jmy';
    this.age=20;

    console.log(this); //Window {top: Window, location: Location, document: document, window: Window,}
}
Foo();
__________________________________
在函数作为构造函数的prototype中，this也是指向当前对象

function Foo(){ 
    this.name='jmy';
    this.age=20;
}

Fn.prototype.getName=function(){ 
        console.log(this.name);
}

var f1=new Foo();
f1.getName();      //jmy


如上代码，在Foo.prototype.getName函数中，this指向的是f1对象。因此可以通过this.name获取f1.name的值。
因为当Foo.prototype定义getName时,f1调用getName就会找到Foo.prototype.getName，所以this指向调用它的this
```

2. 函数作为对象的一个属性
- 如果函数作为对象的一个属性时，并且被对象调用，函数中的this就指向该对象
```
var obj={
x:10,
fn:function(){
console.log(this);    //Object { x:10,fn:function()}
console.log(this.x);  //10
}
};

obj.fn();

fn作为obj的一个属性，并且被obj调用，所以this指向obj

```

- 如果函数作为对象的一个属性，但是没有被对象本身调用
```
var obj={
x:10,
fn:function(){
console.log(this);    //Window {top: Window, location: Location}
console.log(this.x);  //undefined
}
};

var ff=obj.fn;
ff();

上例中，fn函数被赋值到了另外一个变量中，并没有作为obj的一个属性被直接调用，所以this指向window
```

3. 函数用call()或者apply()调用  [具体参考call()笔记]

```
var obj={ 
    x:10
}

var fn=function () { 
    console.log(this);    //Object { x:10}
    console.log(this.x);  //10
}

fn.call(obj);
```

4. 全局&普通函数调用
- 全局
```
在全局环境下，this永远是window
console.log(this ==== window);   //true 

__________________________________
直接调用自定义函数,其中的this也都是window。
 var  x=10

var fn=function () { 
    console.log(this);    //Window {top: Window, location: Location}
    console.log(this.x);  //10
}

fn();
```

- 函数作为对象属性时，嵌套函数
```
var obj={
x:10,
fn:function(){
    function f(){
        console.log(this);    //Window {top: Window, location: Location}
        console.log(this.x);  //undefined
        }
    f();
 }
};
obj.fn();

函数f虽然是在obj.fn内部定义的，但是它仍然是一个普通的函数，this仍然指向window。
```

---

