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

##jQuery中的 $ 其实就是一个函数，它下面的函数，比如$.tirm()其实就是在$上加了一个trim属性，属性值是函数
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


