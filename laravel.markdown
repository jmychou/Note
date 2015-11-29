#MVC
1. 注册路由
2. 创建控制器
3. 控制器中添加方法，从数据库中取数据，传递到视图
4. 在视图中显示数据

##路由
1. Route::get('/','WelcomeController@index')
- Route()声明一个路由
- get 定义一个get路由，此外还有post,put,delete 
- WelcomeController@index : 前面位控制器，后面是调用控制器中的方法
- 第二个参数也可以是一个匿名函数Route::get('/',function(){rerurn view('welcome')};)
- php artisan route:list 会列出所有的路由
- Route::resources('articles',ArticleController);  会生成相应控制器的所有路由

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
return view('page.about',compact('name','age'));  //compact的字符串就是变量名字
```

##Blade  模版
- 可以创建一个公用的布局模版，供其他视图使用，使用`@yield('xxx')`,可以创建多个,供其他视图填充
其他视图开始使用 `@extends('模版名')` 表示使用该模版，
然后 @section('xxx') *** @stop 中间的内容将被填充到@yield('xxx')的位置。

- @yield($name,$default) 的意思是，如果子模板中没有@section('xxx')这块内容，则会显示默认内容，否则就显示子模板中的内容

- 与@section结合的@stop,@show的区别
1. 父模板中使用`@show` ,然后在子模板中使用`@stop`，  如果在父模板中使用了@stop，则子模板使用这块的时候，子模板中的内容不会被显示，
而如果子模板中使用@show来继承父模板，则子模板中的内容会显示两次

- @append 和@override
1. 如果在子模板中多次使用一个@section()区域，可以使用`@append`,多次使用此区域，然后可以用`@stop`结束使用，
2. 而如果在多次使用一个区域的时候，最后使用`@override`，则此前定义的都会被覆盖，只会显示`@section()...@override`这次的内容
3. 所以在对一个数据进行遍历输出时，可以使用`@append`,而如果遍历某处发现出错了，则可以使用`@override`

- 父模板中也可以定义一些公用的片段，然后子模板通过`@parent`调用（@section也可以预定义可替换的内容(在父视图中使用)）
```
//父模板
<!DOCTYPE html>
<html>
<body>
    @section('sidebar')
    This is the master sidebar.
    @show

    @yield('content')

    @section('footer')
        This is the master footer.        
    @show
</body>
</html>

//子模板

@extends('')
@section('sidebar')
     @parent
     <p>This is appended to the master sidebar.</p>
@stop
```

- 常用的blade语句
1. @if...@else...@endif
2. @unless()  相当于if not
3. @foreach...@endforeach
4. 打印变量 {{ dump($name) }} 
5. 在视图中可以使用`$errors`来判断是否有错误
```
@if($errors->any())
    @foreach($errors->all() as $error)
        <li>{{ $error }}</li>
    @endforeach
@endif
```

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

- 此时会在迁移文件中，创建两个方法，up()和down(),up方法是执行php artisan migrate
时调用的，即创建数据表，而down()方法是执行php artisan migrate:rollback时调用，
即会删除数据表
```
 public function up()
    {
        Schema::create('articles', function (Blueprint $table) {
            $table->increments('id');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        //删除表
        Schema::drop('articles');

        //删除某个字段
        Schema::drop('articles',function(Blueprint $table){ 

              $table->dropColumn('字段名')；
            });
    }
```


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

##Model
- Model文件位于app/ 根目录下
- Mode命名,数据表复数而model单数大写
```
如果说我们有一个articles数据表，我们的model相对应就命名为Article；

如果说我们有一个users的数据表，我们的model对应就命名为User；
```

###tinker
- $article= new App\Acticle; 相当于实例化了一个Article类，然后就可以设置
$article的各个字段，$article->id=xxx;
- $article->save() 向数据库中插入一条数据
- 更新 $article->update(['id'=> 2]);(要现在model中设置$fillable)
- 删除，$article = App\Article::find(2); $article->delete();

1. all()方法会返回Article的所有记录
```
App\Article::all();
```

- create()方法会自动把数据插入数据库，不需要save()方法(需要在$fillable中设置要填充的字段，否则会报MassAssigmentException异常)
```
 $article = App\Article::create(['title' => 'New Title', 'body' => 'New body', 'published_at' => Carbon\Carbon::now()]);
```


2. find() 方法介绍一个参数$id，或者为数组
```
1. 查找id为1的一条记录
$article=App\Article::find(1);

2. 传入数组，可以查询多条记录
$article=App\Article::find([1,2]);
```

3. toArray() 将一个Eloquent对象转换为数组
```
$article= App\Article::find(1)->toArray();
```

4. toJson()  将一个Eloquent对象转换为JSON
```
$article= App\Article::find(1)->toJson();
```

5. where() 方法，条件查询
```
$article=App\App\Article::where('title','=','xxx')->get();
此时返回的是一个Eloquent\Collection结果集，如果只需要第一天记录

//此方法返回一个普通的Article类
$article=App\App\Article::where('title','=','xxx')->first();
```

6. destory() 删除数据，介绍一个$id或者一个数组$id
```
App\Article::destory(3);
```


##Carbon

##Laravel 中 `\` 和 `use Illuminate\Support\Facades\`  作用相同

- 查看程序执行的调用栈
1. debug_print_backtrace // 直接打印调用栈
2. debug_backtrace   //不会直接打印，而是把要打印的内容返回给一个字符串，然后可以再输出

- dd()
```
dd();
等同于
var_dump();
die();
```

##Laravel初始化
1. 初始化$app对象，容器
2. 创建$kernel 对象，负责处理http请求
3. $kernel->handle(); 在handle()内部调用bootstrap()处理请求 
4. $app->bootstrapWith($kernel->$bootstraper) ,注册facades,调用RegisterFacades类中的
 bootstrap()方法；调用AliasLoader中注册的自动加载初始化，它使用了在app.php中配置的 
 `aliases`数组，从而通过这个别名， 可以找到对应的真实的类存在的文件，完成对象的实例化，注册
 Facades自动加载_autoload()的实现方法，以AliasLoader类中的load()方法，返回指定的Facades实例
注册配置文件中的providers
5. 初始化完毕，$app->boot()启动

##Session
Laravel的Session没有使用PHP的Session,而是自己的Session(详细见文档，只列出一些方法)
- 设置Session
1. Session::set('key','value');
2. Session::put('key','value');
3. Session::push('array','value');   //将Session存入一个数组中

- 获取Session
1. Session::get('key','default');   //获取Session的值，如果没有的话，则取默认值'defalut'
2. Session::pull('key','default');  //获取Session值，并删除，如果没有的话，则取默认值'defalut'

- 获取所有Session
Session::all()

- 检测某个Session是否存在
Session::has('key')

- 移除某个Session值
Session::forget('key')

- 清空整个Session
Session::flush()















