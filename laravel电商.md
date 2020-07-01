项目中使用 Bootstrap 前端框架，需要先执行以下命令：`composer require laravel/ui:"^1.0" --dev`

引入 Bootstrap ：`php artisan ui vue`

需为 Yarn 配置安装加速：`yarn config set registry https://registry.npm.taobao.org`

使用 Yarn 安装依赖：`SASS_BINARY_SITE=http://npm.taobao.org/mirrors/node-sass yarn`

`php artisan make:model  *** -fm`:`-fm`` 参数代表同时生成 `factory` 工厂文件和 `migration` 数据库迁移文件

`Vue` 一个中国省市区的库:yarn add china-area-data

软链接:yarn install --no-bin-links`

 语言转中文:`composer require overtrue/laravel-lang`  在安装成功后，在 `config/app.php` 替换 `Illuminate\Translation\TranslationServiceProvider::class,` 替换为`Overtrue\LaravelLang\TranslationServiceProvider::class`,

`composer require laravelcollective/html` :FORM表单插件

```
composer require encore/laravel-admin "1.7.7"`:`encore/laravel-admin` 是一个可以快速构建后台管理的扩展包，它提供了页面组件和表单元素等功能，只需要使用很少的代码就实现功能完善的后台管理功能。之后需要执行

 `php artisan vendor:publish --provider="Encore\Admin\AdminServiceProvider"`
 命令会将 Laravel-Admin 的一些文件发布到项目目录中，比如前端 JS/CSS 文件、配置文件等。

`php artisan admin:install`
执行数据库迁移、创建默认管理员账号、默认菜单、默认权限以及创建一些必要的目录。

app/Admin/ 是用来放置管理后台的控制器和路由的目录；
config/admin.php 是 laravel-admin 的配置文件
database/migrations/2016_01_04_173148_create_admin_tables.php 用来创建与后台用户、角色、权限相关的数据库表；
public/vendor/ 是 laravel-admin 会用到的一些前端库；
resources/lang/* 是语言文件,不需要除简体中文以外的语言，可以用下面命令删掉：

除简体中文其他语言文件都不要

`rm -rf resources/lang/ar/ resources/lang/az/ resources/lang/en/admin.php resources/lang/es/ resources/lang/fr/ resources/lang/he/ resources/lang/ja/ resources/lang/nl/ resources/lang/pl/ resources/lang/pt/ resources/lang/ru/ resources/lang/tr/ resources/lang/zh-TW/ resources/lang/pt-BR/ resources/lang/fa/ resources/lang/id/ resources/lang/ms/ resources/lang/uk/ resources/lang/ko/`

Laravel-Admin 的控制器创建方式与普通的控制器创建方式不太一样，要用 admin:make 来创建：
php artisan admin:make UsersController --model=App\\Models\\User
其中 --model=App\\Models\\User 代表新创建的这个控制器是要对 App\Models\User 这个模型做增删改查。

管理后台的路由文件路径是 app/Admin/routes.php


```



上传文件的都是存储在 `storage` 目录下，而 HTTP 服务器指向的根目录是 `public` 目录，要想用户能通过浏览器访问 `storage` 目录下的文件，需要创建一个软链接，`Laravel` 内置了这个命令：`php artisan storage:link` OR 参考:`https://learnku.com/laravel/t/15545/a-solution-to-upload-symlink-inputoutput-error-is-presented`