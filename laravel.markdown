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
{% highlight php linenos %}
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

##Blade  模版
- 可以创建一个公用的布局模版，供其他视图使用，使用`@yield('xxx')`,可以创建多个,供其他视图填充
其他视图开始使用 `@extends('模版名')` 表示使用该模版，
然后 @section('xxx') *** @stop 中间的内容将被填充到@yield('xxx')的位置。


## 维护模式
当你要更新或维护网站时，「关闭」整个网站是很简单的。如果应用程序处于维护模式，HttpException 会抛出 503 的状态码。
- 启用维护模式，只需要执行 Artisan 命令 down：
>   php artisan down
- 关闭维护模式，请使用 Artisan 命令 up：
> php artisan up
- 维护模式响应的默认模板放在 resources/views/errors/503.blade.php。

##数据库迁移
- Laravel 中Migration把表结构存储位一个PHP类，通过调用其中的方法来创建、更改数据库

1. 创建迁移文件，此时会在/databse/migrations/ 创建包含时间戳的PHP文件，即迁移文件，
–-create 是创建一个新表，而 –-table 为修改指定表

- php  artisan make:migration create_user_table   --create==users   //创建user表，    create_user_table 为文件名  
- php artisan make:migration  add_xx_to_users_table  --table=users   //修改user表，给users表添加一个字段


2. 创建迁移文件完成后，用 `php artisan migrate` 执行所有迁移文件，此时数据库中就会出现要创建的表
- 执行迁移文件时，一些tips
```
1. 如果执行出现[class not found ]错误，可以先执行 composer dump-autoload 命令后再执行一次

2. 在线上环境中强制执行迁移
php artisan migrate --force 

3. 回滚迁移，此时数据库中表会被删除
*回滚上一次迁移
php artisan migrate:rollback 

* 回滚所有迁移
php artisan migrate:reset

*回滚所有迁移并且再执行一次(没懂)

php artisan migrate:refresh
 
php artisan migrate:refresh --seed
```

3. <del>test,hahha</del>