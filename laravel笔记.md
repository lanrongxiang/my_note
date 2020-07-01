###### homestead

查看 vagrant 版本
`vagrant -v`

销毁 hometead
`vagrant destroy` 或者 在虚拟机里面直接删除

重装 hometead
vagrant up;vagrant reload --provision 重启并更新Homestead.yaml文件;

重新编译配置文件
`vagrant reload --provision`

###### 安装 laravel

```
Laravel安装:用中国Composer镜像下载composer config -g repo.packagist composer https://packagist.phpcomposer.com;下载Laravel:sudo composer create-project laravel/laravel (项目名称:Taskmanager) 无参数默认下载最新版本;下载提示中文包:composer require caouscs/laverl-lang:~4.0 下载好进入项目目录下的vendor目录caouecs/laravel-lang/src/zh-CN 放到resources/lang;查看Laravel版本:artisan -v ;composer多线程下载: composer global require hirak/prestissimo;清除composer缓存:sudo composer clearcache;:6.0x需要下载:composer require laravel/ui ;查看:php artisan 最后栏目是否多了ui; php artisan ui:auth 开启web服务; 

```



升级 laravel 到指定版本 （更改数字部分）执行 composer update
`"laravel/framework": "5.8.*",  "laravel/framework": "6.0.*",`

查看 laravel 版本
`php artisan -v`

时区
`'timezone' => 'UTC',`
更改
`'timezone' => 'Asia/Shanghai',`

###### 注册登陆功能

artisan route:lsit 访问路由列表;执行artisan后routes目录发生改变web.php是管理所有路由的文件,api.php定义api的文件;auth目录是管理登录注册;app/http/auth/loginC登录相关逻辑代码;bootstrap引入:php artisan ui bootstrap --auth 之后编译 npm install && npm run dev;切换前端框架: artisan preset (框架名字);

生成 auth 脚手架
`php artisan make:auth`

查看路由列表
`php artisan route:list`

创建模型 -h 参数查看帮助
` php artisan make:model Tag -h`

![image-20191203172648527](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20191203172648527.png)

###### 数据库设计与操作

mvc结构:model文件在app目录下,在homestead命令行创建: artisan make:model 文件名(首字母大写) 可以有参数-h是帮助命令, -m 是同时在database目录下创建;Controller文件在Http目录的Auth下;view文件在resources目录下的views里面;



查看迁移状态
`php artisan migrate:status`

数据迁移  
`php artisan migrate`

数据回滚
`php artisan migrate:rollback`

重置
`php artisan migrate:refresh --seed`

###### 开发过程中更改数据结构的两种方式

更新数据结构
`php artisan make:migration add_users_id --table=projects`

```php
添加字段
  $table->integer('user_id'); 
回滚
 $table->dropColumn('user_id');
```

生成数据表 
` php artisan make:migration create_projects_table `

 database/migrations(数据表的迁移或者蓝图);根据蓝图文件创建相应的数据表,可以进行相关的数据表操作(清除或者更新等);在命令行输出artisan migrate:status 查看数据表的迁移状态;artisan migrate执行蓝图文件;make:make:migration生成一个迁移文件(数据表要有意义);migration rollback 撤销之前的蓝图文件操作;

###### 模板视图

>  laravel 里视图模板的扩展与区块声明

```php
模板区块
 <main class="py-4">
      @yield('content')
 </main>

继承
@extends('layouts.app')
@section('content')
   内容
@endsection

局部视图  复用
_ create.blade.php

引入
 @include('projects._createModal')

解析符
{{ asset('js/app.js') }}
{{ mix('js/app.js') }}
```

###### laravel 里的前端框架的选择与切换

```
php artisan preset -h
```

移走前端框架
`php artisan preset none`
切换前端框架 php
`php artisan preset bootstrap   php artisan preset vue `

> 在 artisan preset none 以后再执行 artisan preset bootstrap，就会导致 bootstrap 这个前端框架的 js 文件没有加载上，同时 jQuery 也没有加载上，从而导致模态框只是有 css 样式，但是点击并不会弹出模态框
> （问题原因）

由于这个 preset 命令的 bug，导致 resources/js/bootstrap.js 中下面这块代码没有被加上

```
 try { window.Popper = require('popper.js').default; window.$ = window.jQuery = require('jquery'); require('bootstrap'); } catch (e) {} 可以看到这块的作用就是尝试加载bootstrap和jQuery相应的js文件
```

（问题解决）

如果遇到了上述问题，就在 resources/js/bootstrap.js 中加入上面的这块代码，然后再编译即可

下载新版错误调试工具
`composer require facade/ignition`

设置淘宝镜像
`npm config set registry "https://registry.npm.taobao.org"`

yarn 安装  windows
`yarn install --no-bin-links`

`cross-env: not found 解决`

`yarn add cross-env`

`删除 cross-env  在 package.json`

新版框架需要执行
`yarn add vue-template-compiler --save-dev`

编译和安装
`npm run dev 开发时的编译`
`npm run production 生产时的编译`

###### 使用 laravel collective form 新建项目表单

安装 
`composer update`

laravel的组件仓库网址: `https://packalyst.com/` 

composer国内镜像的几种:

`composer config -g repos.packgist composer https://php.cnpkg.org`

`composer config -g repos.packgist composer https://packagist.laravel-china.org`

`composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/`

`composer报内存不足错误: https://learnku.com/laravel/t/180/how-to-solve-composer-update-memory-is-not-enough`  

把 memory_limit = 1024M 改为-1;

composer常用:

`1)composer self-update 更新到最新版本`

`2)composer diagnose     执行诊断命令`

`3)composer clear            清除composer缓存`

`4)composer update --lock  若项目之前已通过其他源安装，则需要更新 composer.lock 文件` 

模板视图

![image-20191204162428259](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20191204162428259.png)

![image-20191204163019382](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20191204163019382.png)##权限验证
 `$this->middleware('auth');`



###### ##定义一对关系

定义 user 和 project 之间的关系 用户有几个项目

```php
user.php
   public function projects(){
      return $this->hasMany(Project::class); ##hasMany:一对一关系
   }
projects.php
    public function user(){
        // 调用关系 $projec->user
        return $this->belongsTo(User::class); ##belongsTo:一对多关系
    }
```

通过关系添加数据

```php
    public function store(Request $request)
    {
       $request->user()->projects()->create([
           'name'=>$request->name,
           'thumbnail'=>$request->thumbnail
       ]);
    }
```

允许添加数据的字段

```php
 protected $fillable=[
         'name','thumbnail'
 ];
```

表单验证

` php artisan make:request ProjectsRequest`


```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class CreateProjectRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;//判断当前的用户是否有权限来执行当前表单提交;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            'name' => [
                'required', //必填的,
                // 'unique:projects' 不允许同时存在两个,这个projects表里面name这一栏.
                Rule::unique('projects')->where(function($query){
                    //porjects这张表,条件是一个匿名函数需要$query参数,返回这个参数看这张表的user_id是否和登录用户的id一样
                    return $query->where('user_id',request()->user()->id);
                })
            ], 
            'thumbnail' => 'image|dimensions:min_width=260,min_height=100' //是一个图片格式的(不是必填的),定义尺寸:最小的宽度为260px,最小的高度为100px
        ];
    }
    public function messages(){ //laravel提供的一个提示错误信息方法
        return [
            'name.required'=>'项目名称是必填的~',
            'name.unique'=>'项目名称必须是唯一的,不能有重名',
            'thumbnail.dimensions'=>'图片的最小尺寸是260*100px'
        ];
    }
}
?>
    
在视图目录下新建一个提示错误信息的文件errors/_errors.blade.php:
{{-- 全局变量$error可以获取错误信息,any()是否存在错误信息 --}}
          @if($errors->any())
            <ul class="alert alert-danger">
              {{-- 错误信息不止一个所以需要遍历,获取所有的错误信息as单个error --}}
              @foreach ($errors->all() as $error)
                  <li>{{ $error }}</li>
              @endforeach
            </ul>
          @endif
在需要引入的地方引过去:@include('errors._errors')

```

判断一个集合是否为空

```php
 @if(count($projects)>0)
 @if(!$projects->isEmpty())
 @each

  <div class="card-deck">
        @each('projects._card',$projects,'project')
    </div>
```

_card.blade.php

```php
<div class="col-3 my-3">
    <a href="projects{{ $project->id }}" class="card">
        <img class="card-img-top" src="{{ asset('storage/thumbs/original/'.$project->thumbnail) }}" alt="">
        <div class="card-body">
            <h5 class="card-title text-center"> {{ $project->name }}</h5>
        </div>
    </a>
</div>
```

###### #删除功能

```php
{!! Form::open(['route'=>['projects.destroy',$project->id],'method'=>'DELETE']) !!}
 <button type="submit" class='btn btn-default'>
      <i class="fa fa-btn fa-times"></i>
 </button>
{!! Form::close() !!}
```

##SoC原则

关注点分离:（Separation of concerns，SOC）是对只与“特定概念、目标”（关注点）相关联的[软件](https://zh.wikipedia.org/wiki/软件)组成部分进行“标识、[封装](https://zh.wikipedia.org/wiki/封装)和操纵”的能力，即标识、封装和操纵[关注点](https://zh.wikipedia.org/w/index.php?title=关注点&action=edit&redlink=1)的能力。是处理复杂性的一个原则。由于关注点混杂在一起会导致复杂性大大增加，所以能够把不同的关注点分离开来，分别处理就是处理复杂性的一个原则，一种方法。一个laravel可以分为dara Layer:数据层面；Business Later:逻辑业务层面；Web Layer:Http的页面(控制器 );API layer:API接口层面； Mobile Layer:手机端层面;

###### 图片上传

```
php artisan storage:link  //  在linux 环境下生成软连接 ll public
```

安装图片管理组件
`composer require intervention/image`
文档  api
[http://image.intervention.io/getting_start...](http://image.intervention.io/getting_started/installation)

```php
    //存取图片
    public function thumb($request)
    {
        if ($request->hasFile('thumbnail')) { //是否有上传文件
            $thumb = $request->thumbnail;
            $name = $thumb->hashName();
            $thumb->storeAs('public/thumbs/original', $name);
            $path= storage_path('app/public/thumbs/cropped/'.$name);
            Image::make($thumb)->resize(200, 90)->save($path);//需要手动创建文件夹
            return $name;
        }
    }
```

引用
`src="{{ asset('storage/thumbs/original/'.$project->thumbnail) }}"`

```
auth:routes()等同于
// 用户身份验证相关的路由
Route::get('login', 'Auth\LoginController@showLoginForm')->name('login');
Route::post('login', 'Auth\LoginController@login');
Route::post('logout', 'Auth\LoginController@logout')->name('logout');

// 用户注册相关路由
Route::get('register', 'Auth\RegisterController@showRegistrationForm')->name('register');
Route::post('register', 'Auth\RegisterController@register');

// 密码重置相关路由
Route::get('password/reset', 'Auth\ForgotPasswordController@showLinkRequestForm')->name('password.request');
Route::post('password/email', 'Auth\ForgotPasswordController@sendResetLinkEmail')->name('password.email');
Route::get('password/reset/{token}', 'Auth\ResetPasswordController@showResetForm')->name('password.reset');
Route::post('password/reset', 'Auth\ResetPasswordController@reset')->name('password.update');

// Email 认证相关路由
Route::get('email/verify', 'Auth\VerificationController@show')->name('verification.notice');
Route::get('email/verify/{id}/{hash}', 'Auth\VerificationController@verify')->name('verification.verify');
Route::post('email/resend', 'Auth\VerificationController@resend')->name('verification.resend');
```

