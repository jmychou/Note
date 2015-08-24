##路由
1. Route::get('/','WelcomeController@index')
- Route()声明一个路由
- get 定义一个get路由，此外还有post,put,delete 
- WelcomeController@index : 前面位控制器，后面是调用控制器中的方法

##控制器
可以使用php artisan 命令创建 ，帮助可以用 php artisan 或者php artisan help
例如 php artisan help make:controller 
1. php artisan make:controller TestController 会在Controller目录下创建一个TestController.php
里面 默认包含了 index(),create(),store()等方法，如果不需要这些，可以建立一个空方法的控制器
即：php artisan make:controller TestController --plain


## 视图
如果需要访问视图文件夹指定目录（例如views下pages/about.php)，则可以用下列方法调用
return view('pages/about') 或者简单方法 return view('pages.about')

**向视图中传送数据**
1. 
```
$name='abc';
return view('page.about')->with('name',$name);
```
然后视图中可以用<?= $name ?>;因为使用了laraval的blad模版，所以简单写法为：{{ $name }}

## blade模版会转义html语言，如果不需要转义，可以使用{!!  $var !!}

2. 传送数组
```
$data['name']='jmy';
$data['age']=22;
return view('page.about',$data);
```

3. 也可以使用php的compact（）方法，将变量组织成数组
```
$name='jmy';
$age=22;
return view('page.about',compact('first','second'));
```
