### laravel 开发项目步骤



#### 新拉laravel:

​		`config/app.php`

- 修改 时区: `'timezone' => 'Asia/Shanghai',`

- 修改语言 : 首先安装中文包

  ```php
  composer require overtrue/laravel-lang
  在config/app.php替换:Illuminate\Translation\TranslationServiceProvider::class,为Overtrue\LaravelLang\TranslationServiceProvider::class,
  ```

  之后修改`'locale' => 'zh-CN',`

- 安装后台管理扩展包: `laravel-admin`

- 清理缓存

  ```php
  php artisan route:clear
  
  php artisan config:clear
  
  php artisan cache:clear
  ```

- 执行软链接:`php artisan storage:link`  虚拟机如果报错 关闭虚拟机 以管理员身份运行`git.bash`进入虚拟机重置命令



后台上传图片逻辑:

```
0.新拉富文本编辑器:
在 admin.php
/*
     * 扩展设置.
     */
    'extensions' => [
        // 新增编辑器配置开始
        'wang-editor' => [

            // 如果要关掉这个扩展，设置为false
            'enable' => true,

            // 编辑器的配置
            'config' => [

            ]
        ]
        // 新增编辑器配置结束
    ],




1.新建ImageUploadHandler工具类

<?php

namespace App\Handlers;

use  Illuminate\Support\Str;

class ImageUploadHandler
{
    // 只允许以下后缀名的图片文件上传
    protected $allowed_ext = ["png", "jpg", "gif", 'jpeg'];

    public function save($file, $folder, $file_prefix)
    {
        // 构建存储的文件夹规则，值如：uploads/images/avatars/202006/5/
        // 文件夹切割能让查找效率更高。
        $folder_name = "uploads/images/$folder/" . date("Ym/d", time());

        // 文件具体存储的物理路径，`storage_path()` 获取的是 `storage` 文件夹的物理路径。
        // 值如：/home/vagrant/Code/pttq/storage/uploads/images/avatars/201709/21/
        $upload_path = storage_path('app/public'). '/' . $folder_name;

        // 获取文件的后缀名，因图片从剪贴板里黏贴时后缀名为空，所以此处确保后缀一直存在
        $extension = strtolower($file->getClientOriginalExtension()) ?: 'png';

        // 拼接文件名，加前缀是为了增加辨析度，前缀可以是相关数据模型的 ID
        // 值如：1_1493521050_7BVc9v9ujP.png
        $filename = $file_prefix . '_' . time() . '_' . Str::random(10) . '.' . $extension;

        // 如果上传的不是图片将终止操作
        if ( ! in_array($extension, $this->allowed_ext)) {
            return false;
        }

        // 将图片移动到目标存储路径中
        $file->move($upload_path, $filename);

        return [
            'path' =>asset('storage').'/'.$folder_name.'/'.$filename
        ];
    }
}

后台创建上传接口,定义路由
创建:Route::post('journalism/upload', 'UploadController@uploadImg');
编辑:Route::post('journalism/{journalism}/upload', 'UploadController@uploadImg');

<?php

namespace App\Admin\Controllers;

use App\Handlers\ImageUploadHandler;
use App\Http\Controllers\Controller;
use Illuminate\Http\Request;

class UploadController extends Controller {

    public function uploadImg(Request $request,ImageUploadHandler $upload)
    {
        // 初始化返回数据，默认是失败的
        $data = [
            'errno'   => 1,
        ];
        // 判断是否有上传文件，并赋值给 $file
       if($file = $request->upload_file){
           //保存图片到本地
           $result = $upload->save($request->upload_file,'admin','editor');

           //图片保存成功
           if($result){
               $data['data'][]=$result['path'];
               $data['errno']=0;
           }
       }

       return $data;
    }
}

修改富文本底层文件
vendor/laravel-admin-ext/wang-editor/src/Editor.php
<?php

namespace Encore\WangEditor;

use Encore\Admin\Form\Field;

class Editor extends Field
{
    protected $view = 'laravel-admin-wangEditor::editor';

    protected static $css = [
        'vendor/laravel-admin-ext/wang-editor/wangEditor-3.0.10/release/wangEditor.css',
    ];

    protected static $js = [
        'vendor/laravel-admin-ext/wang-editor/wangEditor-3.0.10/release/wangEditor.js',
    ];

    public function render()
    {
        $id = $this->formatName($this->id);

        $config = (array) WangEditor::config('config');

        $config = json_encode(array_merge([
            'zIndex'              => 0,
            'uploadImgShowBase64' => true,
        ], $config, $this->options));

        $token = csrf_token();

        $this->script = <<<EOT
(function ($) {

    if ($('#{$this->id}').attr('initialized')) {
        return;
    }

    var E = window.wangEditor
    var editor = new E('#{$this->id}');
    editor.customConfig.uploadImgServer = 'upload'
    editor.customConfig.uploadImgParams = {_token: '$token'}
    editor.customConfig.uploadFileName='upload_file'
    Object.assign(editor.customConfig, {$config})

    editor.customConfig.onchange = function (html) {
        $('#input-$id').val(html);
    }
    editor.create();

    $('#{$this->id}').attr('initialized', 1);
})(jQuery);
EOT;
        return parent::render();
    }
}


```



#### 

##### 验证错误信息

模板复用:

```
@include('layouts._errors')

@if(count($errors)>0)
	@foreach($errors->all() as $error)   //遍历获取$error集合
		{{ #error }}
	@endfoeach
@endif


表单内 value = "{{ old()}}" //闪存上一次输入的数据
闪存 : 浏览器只保存一次

```



#### 自动登录

创建用户后: 

```
User::create();

//自动登录 返回一个 Boole类型的状态,保存在session里面
\Auth::attempt(['email'=>request->email,'password'=>$request->password]);

//重定向
return redirect()->route();


模板内使用当前用户
@auth
	//当前用户code
@else
@endauth

```



闪存数据操作:

```
定义一个闪存操作
session()->flash('success','注册成功'); //第一个参数为状态可以是warning,error 第二个参数为提示

模板复用:
@include('layouts._message')

@foreach(['success','warning','error','danger'])
	@if(session()->has($t))
		<div class = "alert alert-{{ $t}}"  role="alert">
			<strong>{{ $session()->get($t) }}</strong>
		</div>
	@endif
@endforeach


```



#### 验证规则

```
$this->validate($request,[
	'password'=>'nullable|min:5|confirmed' //nullable如果输入则验证后面的,反之不验证
]);

bcrypt($requset->password) //哈希加盐
```





#### 中间件

> 通俗来讲中间件就是在运行的过程中插入一个钩子,由中间件来控制某一个环节

​	可以写在构造函数内

```
//定义一个验证的中间件把路由控制,如果不是当前用户则跳转到登录页面,排除控制器内的方法也就是路由指向;
$this->middleware('auth',[
	'except'=>['index','show','create','store']
]);

//定义一个游客的中间件,游客可以访问那些路由
$this->middleware('guest',[
	'only'=>['create','store']
]);

```



#### 模型策略

> 模型策略可以解决登录用户篡改数据问题, 也就是说解决用户可以使用哪些方法,权限控制的一种解决方法

```
//定义一个模型策略
php artisan make:policy --model = User UserPolicy 会生成一些简单的方法,也可以定制

//操作一个修改的方法
pulic function update(User $user, User $Modle){
	return $user->id == $model->id
}

//方法内使用
$this->authorize('update',$user)

//注册模型策略  通过这个类寻找到模型
AuthServiceProvider.php
protected $policies = [
'App\User'=>UserPolicy::class
]

//操作一个delete,管理员可以操作全部的权限,自己只能操作自己
pulic function delete(User $user, User $Modle){
	return $user->is_admin && $user->id !=$Model->id
}

//模板使用
@can('delete',$user)
	//code
@endcan

```



#### 关注事件

> 关注时显示取消关注反之关注,同时必须是登录的当前用户,  同类型也可以这样使用

```
//在模型定义一个Toggle方法 laravel提供了一个这样的方法toggle()
public funtion followToggle($ids){
	$ids = is_array($ids)?:[$ids]; //判断$ids 是否为数组,如果不是就给它定义为数组
	return $this->follower()->toggle($ids);
}
```















#### 命令

```
php artisan make:controller -resource --model  =User ABController //资源控制器指定模型会自动注入模型数据

打印集合使用 dd($user->toArray());

```





普天同签新闻页: 数据库模型

新建迁移文件: `php artisan make:migration add_avatar_into_users`

new 表

| 字段名称    | 描述                 | 类型             | 加索引缘由 |
| ----------- | -------------------- | ---------------- | ---------- |
| id          | 自增长 ID            | unsigned big int | 主键       |
| title       | 新闻标题             | varchar          | 无         |
| subhead     | 新闻父标题           | varchar          | 无         |
| description | 新闻详情             | text             | 无         |
| image       | 新闻封面图片文件路径 | varchar          | 无         |
| on_push     | 是否推送             | boolean          | 无         |