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

##JS面向对象
- JavaScript实际上在使用new时,执行了下列三个过程:

* 创建空对象;
* 将类的prototype中的属性和方法复制到实例中;
* 将第一步创建的空对象做为类的参数调用类的构造函数.

---
- 构造函数中直接定义的属性等同于直接定义了对象的属性，可以用obj.hasOwnProperty('属性名')检测到
而通过函数原型(prototype)定义的属性是挂载在原型上的，不属于对象，所以obj.hasOwnProperty('属性名')返回false

---

**JS通过闭包来完成面向对象的私有属性和封装。**
```
function Girl(name,bf){ 
    var secrect =bf;
    this.name=name;
 
    //通过showlove做接口，来读取私有属性secrect
    this.showlove=function(){ 
        retrurn secrect;
    }

    //改变私有属性

    this.movelove=function(){ 
        secrect="xiaowang";
    }
}

var girl =new Girl('hanmeimei','jim');

console.log(girl.showlove())    //输出私有属性的值， jim

girl.movelove() ;  //改变私有属性


console.log(girl.showlove())    //输出改变后的私有属性的值， xiaowang
```

- 构造函数的原型指向 其他构造函数创建的某个具体的对象 来创建自己对象，实现继承
```
//此构造函数用来创建老虎对象
function Tiger(){
    this.huck=function(){
        console.log("I'm tiger");
    }
}

var tiger=new Tiger();
##此时老虎并没有猫的属性，所以可以通过原型继承实现老虎有猫的属性
即：对Tiger()函数指定，用某个具体的"cat对象" 作为老虎的原型，并创建"老虎对象"

function Cat(){ 
    this.climb=function(){ 
            console.log("我会爬树")；
        }
}

//开始继承

Tiger.prototype=new Cat();
var huhu=new Tiger();


huhu.climb(); //实现了继承， 输出 我会爬树

huhu.__proto__ 指向其原型对象----Tiger.prototype(即Cat对象)
```

**原型**
```
console.log(Cat.prototype);  //空对象，显示为Cat{};
console.log(Cat.prototype.constructror);  //因为原型对象的constructror指向Cat,所以显示为Cat()函数。


console.log(Cat.prototype.__proto__);  //Object{}
console.log(Cat.prototype.__proto__.__proto__); //null
```

##对象冒充和复制继承   也可实现继承
- 对象冒充
```
function Cat(leg,tail){ 
    this.leg=leg;
    this.tail=tail;

    this.climb=function() { 
        console.log("我会爬树");
    }
}

function Tiger(leg,tail,color) { 
    this.parent=Cat;   //把父类构造函数引入到一个属性上
    this.parent.apply(this,argumetns);
    
    delete this.parent;  //此时改属性已没用了

    this.color=color;
        
}

//上述实际是将Cat函数放进了Tiger函数中执行，即：

function Tiger(leg,tail,color) { 
    this.parent=Cat;   
    
     this.leg=leg;
     this.tail=tail;

    this.climb=function() { 
        console.log("我会爬树");
    }
    
    delete this.parent;  

    this.color=color;
        
}

var tiger=new Tiger(4,1,'yellow'); 
tiger.climb();   //此时climb() 可以执行，并且tiger本身拥有climb() 方法，(通过原型继承的话，并不拥有)

##分析过程
在用Tiger() 制造对象前，用Tiger语句影响一个空对象，而在此过程中，先有Cat函数实施影响，
所以最终得到的对象，是由Cat和Tiger两者共同作用的对象。
```

- 复制继承 
```
//复制继承，就是把父对象的所有属性，直接复制到自己对象上
function Cat(leg,tail){ 
    this.leg=leg;
    this.tail=tail;

    this.climb=function() { 
        console.log("我会爬树");
    }
}

function Tiger(color) { 
    this.color=color;

    this.extend=function() { 
        for(var key in parent) { 
            this[key]=parent[key];
        }
    }
}

var tiger=new Tiger('yellow') ;
tiger.extend(new Cat(4,1));
tiger.climb();    //此时tiger拥有了Cat的所有属性。


```

##JS多态
```
function a() { 
    this.buck=function() { 
        consol.log("aa") ;
    }
 }

 function b() { 
    this.buck=function() { 
        consol.log("bb") ;
    }
 }

 function test(tt) { 
    tt.buck();
 }

 var aa=new a();
 var bb=new b() ;

 test(aa);   //aa
 test(bb);   //bb

 ##Js 中因为不存在类型检测，所以不是严格意义上的多态
 ```

##JS静态方法
```
function test(color) { 
    this.color=color;
}


test.yy=function() { 
    console.log("static method") ;   //此时就为test的静态方法
}

//
1. yy() 方法是属于"函数"本身的，因为函数也是对象，并且和它自己创建的对象没有关系
2. yy() 要调用，不需要new对象，直接用函数来调用

//
1. Math.random()  就是静态方法
2. $.ajax()   也是静态方法
3. 编写jQuery插件
   3.1 通过闭包，把方法写到jQuery的原型上，(稍复杂)
   3.2 直接增加 $ 对象的静态方法 ，例如
     
      $.sing=function() { 
            alert("haha");
      }

      $.sing() ;//会直接执行
   

```

##混合模式实现继承
- 创建类时，用构造函数定义属性，使用原型方式定义方法
- 继承时，使用对象冒充方式来继承构造函数的属性，用原型链继承来继承方法
```
function ClassA(color){ 
    this.color=color;      //定义属性
} 
ClassA.prototype.getColor=function() { 
        return this.color;       //定义方法
} 

function ClassB(color,area){ 
    ClassA.call(this,color) ;   //对象冒充 来继承属性
    this.area=area;            
}

ClassB.prototype=new ClassA() ;   //原型继承来继承方法
ClassB.prototype.getArea=function() { 
        return this.area;
}

var b=new ClassB('red',100);

b.getColor();
```