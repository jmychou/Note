#注册路由


#创建控制器

- 使用make:controller
    `php artisan make:controller xxxxx`
这种方法会创建很多默认的方法，如index()、create()、store()、show($id)、edit($id)、update($id)、destory($id)
若不需要这些，可以使用 
     `php artisan make:controller xxxxx --plain`


##配置文件
- 根目录下的 `.env`是配置文件
- 可以通过Application的实例的方法 environment()方法来获取当前配置  $app->environment(),也可以传递方法
```
if($app->environment('local')){
  //this environment is local
}

if($app->environment('local','staging')){
  //this environment is local or staging
}

```

- 也可以通过APP facade 获取配置 App::environment()
- 辅助方法app()   app()->environment()

---

- 根据id查找文章
```
$article=Article::findOrFail($id);

等价于

$article=Article::find($id);
if(is_null($article)){
    abort(404);
}
```

- 在模板中添加超链接
```
1. 
<a href="/articles/{{ $arti->id }}"></a>

2. 
<a href="{{ action('ArticlesController@show',[$arti->id]) }}"></a>

3. 
<a href="{{ url('/articles',$arti->id) }}"></a>
```

- 使用illuminate/html来创建Form表单

1. 引入HTML库依赖
`composer require illuminate/html`

2. 修改config/app.php 文件，把库注册到laravel中
`在providers 字段中添加  'Illuminate\Html\HtmlServiceProvider'`

3. 接着在 aliases 字段为刚才注册的 Provider 设置别名, 以后调用的时候就可以使用别名
```
'Form' => 'Illuminate\Html\FormFacade',
'Html' => 'Illuminate\Html\HtmlFacade'
```

- 在视图文件中创建Form表单(此时会生成一个隐藏的_token input标签)
```
{{!! From::open( ['url' => '/articles'] ) !!}} //打开一个表单，设置提交的地址

{{!! Form::close() !!}}

```

- 控制器中获取提交过来的文章
```
//控制器中引入Request类
use Request  

$input= Request::all();  //获取输入的全部数据

//获取单个字段的值
$title = Request::get('title');
$body = Request::get('body');

```

- 设置文章的显示顺序
```
//方法一
$articles= Article::latest('published_at')->get();


//方法二
$articles = Article::orderBy('published_at','desc')->get();
```

- 在Model中设置字段的属性
方法命名规则：set + 字段名 +Attribute,如果字段中带有下划线，需要转换成驼峰命名规则
```
//为published_at 字段设置属性的方法

public function setPublishedAtAttribute($date){

    //设置成未来日期的 当前时间

     $this->attributes['published_at'] = Carbon\Carbon::createFromFormat('Y-m-d',$date);

   //未来日期的 0 点

    $this->attributes['published_at']=Carbon::parse($date);
}

//然后就可以通过条件选择不显示比当前日期大的文章了

$articles=Article::lastet('published_at')->where('published_at','<=', Carbon::now())->get();

//为了防止每次查询都要写上面这一段，可以在Model中定义queryScope方法

public function scopePublished($query){
    $query->where('published_at','<=', Carbon::now());
}

然后在Controller中调用

$articles=Article::lastet('published_at')->published()->get();
```

- Carbon 格式化日期
```
// 返回当前日期加8天
$article->created_at->addDays(8);

//格式化日期
$article->created_at->addDays(8)->format('Y-m-d');

//返回距离当前的时间 如："3 day ago"， "1 week ago"
$articles->created_at->diffForHumans();

```

- Form表单验证
1. 创建Request子类
```
1. 通过命令行创建FromRequest类
    php artisan make:request CreateArticleRequest
此时会在app\Http\Request 目录下生产一个 CreateArticleRequest.php 文件，里面包含了 authorize()和rules()两个方法，
authorize()方法验证用户是否有权限执行FormRequest类或者方法，如果不需要验证，可以修改为 return true;
rules()方法编写字段的验证规则，如：(可以根据不同的条件来判断编写不同的验证规则，提高代码的复用性)

public function rules()
{
    return [
        'title' => 'required|min:3',  //必填项,长度为3
        'body' => 'required',         //必填项
        'published_at' => 'required|date',  //必填项，日期类型
        // 也可以使用数组
        //'published_at' => ['required', 'date'],
    ];
}

2.控制器中引入创建的FormRequest类，并修改相应的方法

// 添加刚刚创建的FromRequest类
use App\Http\Requests\CreateArticleRequest;

//并且修改store()方法，使用CreateArticleRequest对象
    public function store(CreateArticleRequest $request){
        Article::create($request->all());
        return redirect('articles');
    }
```

2. 使用Request类的Validate 方法
```
1. 引入Request类
use Illuminate\Http\Request

2.修改相应方法

public function store(Request $request){
    $this->validate($request,['title' => 'required|min:3', 'body' =>'required', 'published_at' => 'required|date']);
    Article::create($request->all());
    return redirect('articles');
}
```

- 修改文章时，使用Laravel的模型绑定，让相应的要修改文章的内容显示在页面里
```
在视图中不使用From:open()创建表单，而是用{!! Form::model($article, ['method' => 'PATCH', 'action' => ['ArticlesController@update', $article->id]]) !!}关联相应的文章。
```

- Eloquent的一对多关系
一个用户可以发表多篇文章
```
在app\User.php模型中添加 
public function articles(){
    return $this->hasMany('App\Article');
}
这样就可以通过$user->articles 来获取用户的文章

此时修改控制器中的方法
 public function store(ArticleRequest $request){
        $article = new Article($request->all());
        \Auth::user()->articles()->save($article);   //会自动关联用户ID
        return redirect('articles');
    }


在app\Article.php 中添加
public function user(){
    return $this->belongsTo('App\User');
}
这样就可以通过$article->user  获取文章所属用户
```

##中间件
- 如果每个需要验证的地方都要写`if(Auth::guest()){redirect('articles')}` 很麻烦，所以使用中间件可以解决
1. 中间件位于app/Http/Middleware 目录下，`php artisan  make:middleware Demo` 创建中间件，然后在`app/Http/Kerner.php`中，注册生产的中间件

2. 在ArticleController的construct中使用中间件
```
  public  function  __construct(){
        //无论访问哪个页面，都要中间件验证
        $this->middleware('auth');

        //除了主页之外，其他页面需要验证
       $this->middleware('auth',['except' => ['index','show']]);

        //只有发布页需要验证
       $this->middleware('auth',['only' => 'create']);


    }

```

3. 在定义路由时，也可是使用中间件
```
Route::get('about',['middleware' =>'auth' ,function(){
        return 'Only register can show';
    }])
```

##路由模型绑定
- 在控制器中需要频繁的写`Article::findOrFali($id);`,通过路由模型绑定，可以自动找到ID相对应的记录
- app/Providers 中定义了Laravel如何创建和启动组件，其中RouteServiceProvider.php 就是定义程序路由如何启动的。
- 通过修改路由模型，控制器中的方法就可以改为
```
//修改之前

 public function show($id){
        $article = Article::findOrFail($id);
 
        return view('articles.show', compact('article'));
    }

//修改之后

public function show(Article $article){
        return view('articles.show', compact('article'));
    }
```

- 使用Flash Message创建文章发布后的显示的信息
1. 使用Session::flash()暂存数据的方法
```
1. 在控制器的store()方法中，添加
\Session::flash('flash_message','文章发布成功');  //普通信息自动关闭
\Session::flash('flash_message_import',true);   //重要信息点关闭按钮
上面的代码也可以写成
return redirect('articles')->with([
        flash_message =>'发布成功',
        flash_message_import =》true
    ]);

2. 在模板中,通过判断不同的Session值，显示不同的信息
Session::has('xxx');  //判断Session是否存在
Session::get('xxx');  //取出Session的值，或者 Session('xxx')也可以读取
```

2. 使用库依赖
```
1. 引入库依赖
compoer require laracasts/flash

1.1. 修该config/app.php文件，在providers添加
'Laracasts\Flash\FlashServiceProvider'

1.2. 在aliases中添加别名
'Flash'  => 'Laracasts\Flash\Flash'

2. 修改store()方法
flash('发布成功!');  //或者使用sucess(),warning(),error()等

3. 修改模板
- 引入模板
@include('flash::message');

- $('#flash-overlay-modal').modal() 等同于 $('div.alert').not(.alert-important).delay(3000).slideup(300);

- 修改store
flash->overlay('发布成功'，'Good Job'); //此时消息会等用户点击关闭按钮
```

- 给文章添加标签
1. 在文章模型中添加多对多关系
```
在/app/Article.php中添加 
public function tags(){
    return $this->belongsToMany('App\Tag')->withTimestamps();
}
```

2. 创建标签的Eloquet Model 

3. 然后在标签模型中添加多对多关系
```
在/app/Tags.php中添加 
public function articles(){
        return $this->belongsToMany('App\Article');
    }
```

4. 关联标签和文章
```
App\Tag::all()->toArray();   //获取所有Tag，以数组形式显示

App\Tag::lists('name'); //列出所有Tag的名称
[
       "PHP",
       "Java",
       "C##",
       "Python",
       "Go"
]
而App\Tag::lists('name','name')结果为
[
       "PHP"    => "PHP",
       "Java"   => "Java",
       "C##"    => "C##",
       "Python" => "Python",
       "Go"     => "Go"
]

- 使用attach()方法来关联标签和文章
$article= Auth::user()->articles()->create($request->all());
$article->tags()->attach($request->input('tags'));

- 获取文章的标签
$article->tags()->toArray();
//获取标签名
$article->tags()->lists('name');

- 获取标签对应的文章
$tag->articles->toArray();

```

5. 在模板中显示标签
```
<div class="form-group">
    {!! Form::label('tags', 'Tags:') !!}
    {!! Form::select('tags[]', $tags, null, ['class' => 'form-control', 'multiple']) !!}  //使用tags[]才会显示所有的标签
</div>
```

6. 在编辑文章页面，显示文章的标签，需要使用表单模型绑定
```
在文章模型Article.php中 获得属性的方法
public function getTagListAttribute(){
    return $this->tags()->lists('id');   //$this就指向当前显示的文章，类似 $article->tags()->lists('name');
}

```

7. 确保文章和标签的 文章-标签对 的唯一性
 1. 第一中方法 是设置数据表article_tag 表中，article_id 和tag_id  对的唯一性

 2. 使用sync()方法， 方法可以确保其传递的参数数组是唯一与该博客关联的标签的ID，而其它的都会被删除。
 ```
 public function update(Article $article, ArticleRequest $request){
    $article->update($request->all());
    $article->tags()->sync($request->input('tag_list'));
    return redirect('articles');
}

//类似的 attach()是附加，detach()是删除，sync()是同步
 ```




















