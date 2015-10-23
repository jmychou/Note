- var 只是声明变量

```
function ff(){
    d=10;
}
d没有加var 仅仅是一个赋值操作
ff(）执行的时候，遇到d,会先在ff()中寻在d的定义，没找到，就会一直向上找，直到找到window，而window中
也没有d,所以就执行window.d=10
```

```
function f1(){
    var d;
    function f2(){
        d=10;
        e=12;
    }
    f2();
}

f1();

console.log(e);  //12
console.log(d);  //d is not defined


##f1()执行时，遇到f2()执行，碰到d时，发现f2中未定义，就找到f1，在f1中找到了d,所以在f1中d=10；而在
window中就没有d  ,e是在f1，f2中都未找到，于是就时window.e=12；所以没有var定义，仅仅是一个赋值操作，
假如当前没找到定义，就一直向上级找，直到window，把它理解成全局变量有点片面。

——————————————————————————————
window.xxx 引用全局变量，寻找不到，作为某个属性不存在，返回undefined
直接以xxx引用某变量，寻找不到，则会报 xxx is not defined 错误。
```

```
var s1="global";
function ff(){
    console.log(s1);  //c1
    console.log(s2);  //c2

     s2="local";      //c3
}

ff();   // 输出 global   s2 is not defined 


##分析
ff()执行到c1时，在ff内寻找s1，没有找到，又在window中，寻找到，所以打印global
执行到c2时，在ff内寻找s2，没有找到, 又在window中，也没找到，所以打印 s2 is not defined

执行到c3(实际执行不到)， 在ff内寻找s2，没有找到, 又在window中，也没找到，所以在赋值全局属性window.s2

__________________________________
类似的一个

var s1="global";
function ff(){
    console.log(s1);  //c1
    console.log(s2);  //c2

   var   s2="local";    //加个var   //c3
}

ff();   // 输出 global   undefeated

##分析
js是自上而下执行，但在执行之前有一个'词法分析', 所以上例等价于 var s2;  console.log(s2);  s2="local";

1步，先分析ff函数，分析出ff内部有s2变量，但未赋值，赋值时运行时确定的，此时函数未执行，所以s2为undefined

2 步，自上而下执行，执行到c2时，此时s2还未赋值，而执行到c3时，s2才被赋值，此时s2="global".


——————————类似的
var str="global";
function ff(){
 console.log(str);
 var str="local";    //此不若去掉 var ,则输出为 global  local
console.log(str);
}

ff();   //undefined  local 

```

----
##词法分析
1. 先分析参数(函数参数和传递的实参)
2. 再分析变量声明(var xxx)
3. 最后分析函数声明

**具体步骤**
1. 函数运行前，生气Active Object(活动对象)，检测AO
2. 
  - 2.1 函数声明的参数(形参)，形成AO的属性，相当于在函数内部定义一个变量(var a)值全是undefined
  - 2.2 接受实参，形成AO相应的属性值，如果有同名属性，则被覆盖
3. 分析变量声明 ，如var age
    如果AO上还没有age属性，则添加age属性，值是undefined
    如果AO上已经有age属性，则不做任何操作
4. 分析函数声明，如function Foo(){};
   如果AO已经有FOO属性，则被覆盖，否则把函数赋给AO.FOO 属性 
---

```
 function fn(a){

   alert(a);    //undefined,JS会把传参当作var一样对待,相当于在fn内部定义了一个变量--> var a;

   a = 2;

   alert(a);    //2,这里修改的不是全局的哦,改的是fn的局部变量哦

 }

fn()

alert(a);    //报错
```

1. 例1
```
function t(age){ 
    console.log(age);
}

t(5);      //5
t();       //undefined

##词法分析
1. 先产生 Active Object (简称AO)； AO={}
2. 得到age属性，即AO={age：undefined}
3. 运行过程
t(5) -->AO.age=5;所以输出5
t() --->AO.age没有赋值，还是undefined
 
```

2. 例2
```
function t2(age){ 
    var age=99;
    console.log(age);
//
    var age=99;   //若上句放在此，则输出5
}

t2(5);    //99

##分析
- 分析过程
1. 形成空的 AO={}；
2
  2.1 分析形参，AO={age :undefined}
  2.2 接受形参，AO={age:5}
3. 分析 var age ,发现AO已有age属性，不做任何影响

- 执行过程
AO.age=99;
所以输出99
```

3. 例三
```
function t3(great){ 
    var great="hello";     //此处如改为 var great ,则输出结果为 两个function
    console.log(great);

    function great(){ 
    }
    console.log(great);
}

t3(null);   //hello   hello

##分析
- 分析过程
1.产生AO空对象 AO={}；
2. 
    2.1 分析形参，AO={great:undefined}
    2.2 接受形参，AO={great:null}
3. 分析great变量声明(var great) ;AO已经有great属性，因此不做任何影响
4. 分析great函数声明，AO.great=function (){} ;原有的great属性被覆盖成函数

- 执行过程
great="hello";
console.log(great);
console.log(great);

执行过程，great被赋值为hello;于是输出hello,hello
```

4. 例4 
```
function a(b){ 
    console.log(b);

    function b(){ 
        console.log(b);
    }

    b();
}

a(1);   //输出两个function ,   function b(){  console.log(b); }   function b(){  console.log(b); }  

##分析

- 分析过程
1. AO={}；
2. 
    2.1 分析形参，AO={b:undefined}
    2.2 接受形参，AO={b:1}

3. 分析 var声明，此函数没有var
4. 分析函数声明，此时AO{b: function(){} } ,原有b属性被覆盖

--执行过程
 console.log(b);   //输出function
  b();             //执行b函数，执行到console.log(b)时，发现b函数中没有b,于是沿作用域在a函数中
  找到b,所以输出函数。

```

5. 上例改为
```
function a(b){ 
    console.log(b);

    b=function(){ 
        console.log(b);
    }
    b();
}

a(1);            //输出结果为 1  function(){ console.log(b); }


##分析

-- 分析过程
1. 产生AO空对象，AO={}；
2. 
    2.1 分析形参，AO={b:undefined};
    2.2 接受形参，AO={b:1}
3. 分析Var 声明，没有
4. 分析函数声明，没有 ； 函数表达式在函数执行阶段，才起作用

——— 执行过程
console.log(b);     //1
b=function(){ 
        console.log(b);
    }
b();   //在b()函数内，没有找到b,所以到a函数中去找，找到了，所以输出 function(){ 
        console.log(b);
    }

```

6. 匿名函数立即执行 -- 不污染全局变量
```
( function(window,undefined){ 
  })(window)

- ( function(window,undefined){ })    // 内层表达式，返回值是函数，在小括号里，当成表达式来执行
- ( function(window,undefined){ })()   //立即调用
```

- 作用链域就是函数(嵌套)中的AO链，词法分析时从外部函数分析支内部函数，执行时从内部查找到外部函数。

**当一个函数运行的时候，函数内部引用的变量有   AO，arguments ,this**
1. AO   ---本函数AO上没有属性，则继续去外层函数的AO上去找，直到全局对象  --作用域链
2. arguments  ---每个函数都有自己的arguments,但不会向外层函数寻找arguments属性，即不会形成链。


---
##arguments
- 是一个对象，长得很像数组的对象
- 函数运行时的所以实参列表，可以多于函数的形参
- arguments.length可以获得函数执行时实参的个数(多态时可以用到)
```
(function(a,b,c){
console.log(a);            //hello
arguments[0]="china";               //形参与相应的arguments单元相互对应，互相影响。
console.log(a);            //china
console.log(arguments[3]);  //dd
})("hello","bb","cc","dd");  
```

- arguments.callee  --代表当前运行的函数
```
#递归求和
1. 函数调用
function ff(n){
    if(n<=1)
        return n;
    return n+ff(n-1);
}

ff(100);

2. 匿名函数调用

(function(n){
    if(n<=1)
        return n;
    return n+arguments.callee(n-1);
})(100)
```


##this
**JS中函数的4中调用方式

1. 作为普通函数来调用
```
function t(){
    this.xx=100;
}

t();
console.log(window.x);  //100
```

- 作为普通函数来调用时，this的指向为window(准确的说为null,但被解释成window)

2. 作为对象的方法来调用   this指向方法的调用者，即该对象
```
var obj={
    xx:999,
    yy:888,
    t:function(){
        console.log(this.xx);
    }
}

obj.t();   //999
_____________

var dog={
    xx:"wangwang"
}
dog.t=obj.t;

dog.t();   //wangwang 
```

- 作为对象方法调用时，this指向调用那一刻的调用者，即谁调用的就指向谁，
不管被调用函数，声明时属于方法，还是函数。
```
show=functon(){
    console.log('show '+ this.xx);
}

dog.t=show;
dog.t();   //show wangwang
```

3. 函数作为构造函数调用
- JS中创建对象使用构造函数完成的

```
function Dog(name,age){ 
    this.name=name;
    this.age=age;

    this.bark=function(){ 
            console.log(this.name);
    }
}

var dog=new Dog('huzi',2);

dog.bark();
```

- new Dog()发生了以下几个步骤
a. 系统创建空对象 {}，(空对象的constructor属性指向Dog()函数)
b. 把函数的this指向该对象
c. 执行该函数
d. 返回该对象

```
function Pig(){ 
    this.age=99;
    return 'hello';
}

var pig=new Pig();
console.log(pig);    

//返回pig对象，因为函数作为构造函数运行时，return的值是忽略的，还是返回对象，所以不用写return
```

4. 函数通过call()、apply()调用
- fn.call(对象obj,参数1，参数2...参数N)；
运行如下：
1. fn()函数中的this指向 对象obj
2. 运行fn(参数1，参数2...参数N)


**每个函数都有自己独有的arguments和this,且不进行链式查找。**

##闭包
```
function t1(){ 
    var age=20;

    function t2(){ 
        console.log(age);
    }

    return t2;
}

var tmp=t1();
var age=99;

tmp();   //20
```

- t1()在执行过程中，产生了t2(),从作用域讲，t2能访问到age=20;于是"age=20"并没有消失，
而是和返回的t1()函数形成了一个"环境包"，这个包属于t2()，所以叫闭包
- age=20 这个变量，被t2捕获，所以即使t1执行完毕，通过t2依然能访问该变量 ，这种情况返回的函数，
并非孤立的函数，甚至包括周围的变量环境，形成的封闭的环境包，共同返回，即闭包。

```
function foo (a) {
  return function (b) {
    return a+++b;
  };
}

var a = foo(10);
var b = foo(20);

var arrs = [a(10), b(20), a(10)];
console.log(arrs);  // [20,40,21]    //每次foo中返回的函数，会和形参的最新的值(组成最新的环境返回)
```

**闭包调用，每次调用都会返回一个新函数，即使传入相同的参数**
```
function lazy_sum(arr){ 
    
    var sum=function() { 
        return arr.reduce(function(x,y){ 
                return x+y;
            });
    }

    return sum;
}

var f1=lazy_sum([1,2,3,4,5]);
var f2=lazy_sum([1,2,3,4,5]);

console.log(f1===f2);  //false

```

**闭包中的返回函数，不要引用任何循环变量，或者后续会发生变化的量**
```
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push(function () {
            return i * i;
        });
    }
    return arr;
}

var results = count();
var f1 = results[0];
var f2 = results[1];
var f3 = results[2]; 

上述返回结果都是16 ，所以需要改进

//用函数参数绑定需要循环变化的变量的当前值
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push((function (n) {
            return function () {
                return n * n;
            }
        })(i));
    }
    return arr;
}

```

---

**函数的作用域，取决于声明时，而不取决于调用时。**

- 闭包计数器
```
function counter(){ 
    var cnt=0;

    var cnter=function(){ 
        return ++cnt;
    }

    return cnter;
}
var inc=counter();

console.log(inc());     //1
console.log(inc());     //2
console.log(inc());     //3 

##简化2
var cnt=(function(){ 
        var cnt=0;

    return function(){ 
        return ++cnt;
    }
    })();

console.log(cnt());     //1
console.log(cnt());     //2
console.log(cnt());     //3  

##为了避免全局污染或冲突
1. 统一放在一个全局对象上，如jQuery --->$
2. 每人用自己的命名空间，(把自己的变量放入一个对象中)

//jQuery的计数器插件

$={};

$.counter=(function (){ 
           var cnt=0;

    return function(){ 
        return ++cnt;
    }
 })();
 

console.log($.counter());
console.log($.counter());
console.log($.counter());


```

**JS中有对象，没有类 (但有构造函数)，对象的属性可以任意添加和删除.**