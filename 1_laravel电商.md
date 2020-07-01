[toc]





### 一丶需求分析:

> **主要从以下三种元素入手:  1.角色;2.信息;3.动作;**

#### 	1.角色:

+ 将会出现的角色:
+ 游客 —— 没有登录的用户；
+ 用户 —— 注册用户， 可以购买商品；
+ 运营 —— 可以上架、下架商品，处理订单；
+ 管理员 —— 权限最高的用户角色，可以管理运营。

####      2.信息结构(主要信息):

- 用户 —— 模型名称 `User`；

 - 收货地址 —— 模型名称 `UserAddress`，包含地址和收货人姓名、电话；
 - 商品 —— 模型名称 `Product`，比如 `iPhone X` 就是一个商品；
 - 商品 `SKU` —— 模型名称 `ProductSKU`，同一商品下有个别属性可能有不同的值，比如 `iPhone X 256G` 和 `iPhone X 64G` 就是同一个商品的不同 `SKU`，每个 `SKU` 都有各自独立的库存；
 - 订单 —— 模型名称 `Order`；
 - 订单项 —— 模型名称 `OrderItem`，一个订单会包含一个或多个订单项，每个订单项都会与一个商品 `SKU` 关联；
 - 优惠券 —— 模型名称 `CouponCode`，订单可以使用优惠券来扣减最终需要支付的金额；
 - 运营人员 —— 模型名称 `Operator`，管理员也属于运营人员。

#### 3.动作:

- 角色和信息之间的互动称之为动作,主要动作:
- 创建 `Create`
- 查看 `Read`
- 编辑 `Update`
- 删除 `Delete`

***用例:***

> 1. 游客
>    - 游客可以查看商品列表；
>    - 游客可以查看单个商品内容。
> 2. 用户
>    - 用户可以查看自己的收货地址列表；
>    - 用户可以新增收货地址；
>    - 用户可以修改自己的收货地址；
>    - 用户可以删除自己的收货地址；
>    - 用户可以收藏商品；
>    - 用户可以将商品加入购物车；
>    - 用户可以将购物车中的商品打包下单；
>    - 用户可以在下单时使用优惠券；
>    - 用户可以通过微信、支付宝支付订单；
>    - 用户可以查看自己的订单信息；
>    - 用户可以对已支付的订单申请退款；
>    - 用户可以将已发货的订单标记为确认收货；
>    - 用户可以对已购买的商品发布评价。
> 3. 运营
>    - 运营可以看到所有的用户列表；
>    - 运营可以发布商品；
>    - 运营可以编辑商品内容；
>    - 运营可以编辑商品 SKU 及其库存；
>    - 运营可以下架商品；
>    - 运营可以将用户已支付的订单标记为已发货；
>    - 运营可以对申请退款的订单执行退款；
>    - 运营可以创建、编辑、删除优惠券。
> 4. 管理员
>    - 管理员可以查看运营人员列表；
>    - 管理员可以新增运营人员；
>    - 管理员可以编辑运营人员；
>    - 管理员可以删除运营人员。

____

#### 4开发思路:

*很多时候,当开发团队开始启动一个项目时,区分功能模块的优先顺序尤其重要,否则你都不知道从何下手.使用一个简单的分析框架,来决策功能模块的开发优先级,也可以使用其对大部分web项目进行模块开发的优先级分析*

1. **模块清单**
   *首先,基于需求分析,将系统拆分为:*
   - 用户模块
   - 商品模块
   - 订单模块
   - 支付模块
   - 优惠券模块
   - 管理模块
2. **依赖关系**
   *有了模块清单,接下来需要思考他们之间的依赖关系是怎样的,在功能清单中订单模块依赖于用户模块和商品模块,支付模块和优惠券模块依赖订单模块,各个模块之间的依赖关系*

![mwSasI9sxg](D:\360MoveData\Users\我爱丹丹\Desktop\laravel电商mdImg\mwSasI9sxg.png)

*上层的模块依赖于下层的模块,因此在开发过程中会优先构建下层模块*

3. **开发顺序**

   - 用户模块
   - 商品模块
   - 订单模块
   - 支付模块

      - 优惠券模块

        *管理模块是一个特殊的模块,既包含本身的逻辑（管理后台的权限控制等），又与其他业务模块都有关联，因此在开发过程中会与其他模块穿插开发。*

4. **``mvp``产品**

      >  ​	作为工程师不需要了解产品的方方面面,那是产品经理的工作,但是作为一位优秀的开发者,在开发项目时,对将要完成的产品mvp要了然于胸,MVP是Minimum Viable Product （最小化可行性产品）的简称。如何得出产品的mvp产品?可以先问:对于这个产品来讲，哪些功能是必不可缺的？
      >
      > ​    电商产品是一个用户购买商品的地方,产品存在的核心价值是『用户购买商品』,首先需要用户,然后需要商品,购买需要付款,所以在电商项目中,用户,商品,订单和支付模块都是必不可少的.
      >
      > ​    优惠券功能并不是购物流程中必备的一环,属于附加的功能,锦上添花的东西,在设计和开发项目时,应优先完成基础的功能,让流程能尽快跑起来,今早交付,快速迭代.
      >
      > ​    Web 开发是个速度至上的领域，最小产品功能先上，测试的工作量也不会太大。不能憋大招，一个上线就是一大堆功能，复杂度增加的是无限的开发和调错时间，项目上线期限无尽延长。另一方面，用户能在最短时间内接触到产品，产品经理也可以尽快听到用户的反馈，及时调整产品战略，产品离成功会更进一步，这是一个多赢的方案。
      >
      > ​    这个思路与敏捷开发的思路不谋而合:敏捷开发即是以用户的需求进化为核心，采用迭代、循序渐进的方法进行软件开发。



***



### 二丶创建应用:

1. 配置好homestead的前提;

2. 在Homestead的配置文件中做好站点配置和数据库配置:
   **``Homestead.yaml``文件:**

   ```
   sites()
   
      - map: shop.test
        to: /home/vagrant/Code/laravel-shop/public
        .
        .
        .
        databases:
           - homestead
             - laravel-shop
               项目将使用laravel-shop数据库,还需要配置本机Hosts文件,将shop.test指向homestead虚拟机IP
                  	启动和登录Homestead:在homestead目录执行:vagrant provision && vagrant up && vagrant ssh; cd ~/code
                  	composer加速:使用阿里云镜像:composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
                  	创建laravel-shop项目: 在 ~/code目录执行:composer create-project laravel/laravel laravel-shop --prefer-dist "6.*"
                  	配置.env:
                  	APP_NAME="Laravel Shop"
               .
               .
               .
               APP_URL=http://shop.test
               .
               .
               .
               DB_DATABASE=laravel-shop
               DB_USERNAME=homestead
               DB_PASSWORD=secret	
   ```

   *如果环境变量的值中包含空格，需要用双引号将值包含起来*



##### 1.辅助函数:

- 创建自己的辅助函数

- 把所有的『自定义辅助函数』存放于 `bootstrap/helpers.php` 文件中，创建这个文件，并且放入如下内容：

  ```php+HTML
  <?php
  function test_helper() 
      return 'OK';
  }
  ```

  

- 在虚拟机中进入`tinker`测试*(`tinker` 是 `Laravel` 内置的一个交互式控制台，可以很方便地调试 `PHP` 代码。)*`art tinker`;

- 在tinker执行test_helper()函数,报错找不到这个函数,是因为没有引入`helpers.php`文件,可以使用composer的`autoload`功能来引入 按 `ctrl + d` 退出 tinker 程序。打开`composer.json`文件,并找到`autoload`字段添加"files"数组:参数是"`bootstrap/helpers.php`",注意逗号不要多写和漏写,然后在虚拟机项目根执行`:composer dumpautoload`;再次到tinker测试

  



##### 2.基础布局:

- 构建一个基础的页面布局,布局文件统一存放在resources/views/layouts目录下,涉及的文件

   - `app.blade.php` —— 主要布局文件，项目的所有页面都将继承于此页面；
   -  `_header.blade.php` —— 布局的头部区域文件，负责顶部导航栏区块；
   -  `_footer.blade.php` —— 布局的尾部区域文件，负责底部导航区块；

-  执行命令创建:

  ```
  $ mkdir -p resources/views/layouts/
  $ touch resources/views/layouts/app.blade.php
  ```

  *`resources/views/layouts/app.blade.php:`*

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- CSRF Token -->
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>@yield('title', 'Laravel Shop') - Laravel 电商教程</title>
    <!-- 样式 -->
    <link href="{{ mix('css/app.css') }}" rel="stylesheet">
</head>
<body>
    <div id="app" class="{{ route_class() }}-page">
        @include('layouts._header')
        <div class="container">
            @yield('content')
        </div>
        @include('layouts._footer')
    </div>
    <!-- JS 脚本 -->
    <script src="{{ mix('js/app.js') }}"></script>
</body>
</html>
```
- route_class() 是自定义的辅助方法，需要在 helpers.php 文件中添加此方法：

  ```php
    function route_class()
          {
          //此方法会将当前请求的路由名称转换为 CSS 类名称，作用是允许 针对某个页面做页面样式定制。
              return str_replace('.', '-', Route::currentRouteName());
          }
  ```

- 首页展示:

 1. 创建控制器:使用控制器 `PagesController` 来处理所有自定义页面的逻辑，并使用 root() 方法来处理首页的展示。 命令`:php artisan make:controller PagesController`;新增root方法:`return view('pages.root')`;

 2. 视图:

    - 控制器 `root()` 方法中加载了视图 `pages.root` 
    - 创建命令: `mkdir -p resources/views/pages/ ; touch``resources/views/pages/root.blade.php;`
    - `Laravel` 自带了一个主页视图 `welcome.blade.php`，既然已经自定义了主页视图，即可将废弃的主页视图删除：`rm resources/views/welcome.blade.php`;

 3. 绑定路由:将`web.php`里内容替换成:
        `Route::get('/', 'PagesController@root')->name('root');`访问站点发现应用抛出异常,因为在`resources/views/layouts/app.blade.php` 中使用 mix() 方法,而未运行`Laravel Mix` 进行编译，找不到 `mix-manifest.json` 文件，所以报错;
     
 4.  集成`bootstrap`:执行命令:`composer require laravel/ui:"^1.0" --dev`

        - 引入`bootstrap:php artisan ui vue;`

          *以上命令做了:*

             1. 在 `npm` 依赖配置文件 `package.json` 中引入 `bootstrap、jquery、popper.js、vue` 作为依赖；

             2. 修改 `resources/js/bootstrap.js` ，在此文件中初始化 `Bootstrap UI` 框架的 `JS` 部分；

             3. 修改 `resources/sass/app.scss` 以加载 Bootstrap 的样式文件；

             4. 新增 `resources/sass/_variables.scss` 样式配置文件。

             5. 运行 `laravel mix:Laravel Mix` 一款前端任务自动化管理工具，使用了工作流的模式对制定好的任务依次执行。Mix 提供了简洁流畅的 `API`，让你能够为你的 `Laravel` 应用定义 `Webpack` 编译任务。Mix 支持许多常见的 `CSS` 与 JavaScript 预处理器，通过简单的调用，你可以轻松地管理前端资源。
                    使用mix:安装`npm`依赖,使用yarn安装,因为国内网络原因,需给`yarn`配置加速:

                ```
                yarn config set registry https://registry.npm.taobao.org
                SASS_BINARY_SITE=http://npm.taobao.org/mirrors/node-sass yarn
                ```

                - 在 yarn 命令前添加 SASS_BINARY_SITE=[http://npm.taobao.org/mirrors/node-sass] 的目的是告诉 yarn 到淘宝的镜像去下载 node-sass 二进制文件。win使用yarn安装必须要在命令后面加上  `--no-bin-links`参数
                - 修改`webpack.mix.js` 末尾加一个.version()使 Mix 每次生成的静态文件后面加上一个类似版本号的参数，避免浏览器缓存。
                - 然后运行:`npm run watch-poll` 简写命令 `nrwp` 会在终端持续运行,监控resources文件夹的资源文件是否发生改变,在 watch-poll 命令运行的情况下，一旦资源文件发生变化，`Webpack` 会自动重新编译。
                - 样式代码在 `resources/sass/app.scss`，是用 SASS 编写的，最终会由 Mix 来编译成 `CSS`。
                - 在加入版本库之前执行 `git status` 可以看到 Mix 在 public 目录下生成了 `js / css` 目录以及 `mix-manifest.json` 这个文件，这也是不需要加入版本库的，在 `.gitignore` 中添加几行：

                ```php+HTML
                   /public/mix-manifest.json
                   /public/js
                   /public/css
                ```



***



### 三丶用户模块:

> 用户模块是绝大多数网站都需要的功能，电商系统也不例外，在o这个电商系统里用户即买家。

#### 	1. 注册和登录	

**`laravel`自带了用户认证功能,利用此功能快速构建用户中心**

- 执行:`php artisan ui:auth` 认证脚手架命令生成代码;命令会询问是否要覆盖 `app.blade.php`,因为经自定义了『主要布局文件』—— `app.blade.php`，所以此处输入 no
- `routes/web.php` 查看修改了哪些内容：`Auth::routes();` 是 `Laravel` 的用户认证路由，这里不需要去修改。`Route::get('/home', 'HomeController@index')->name('home');`已经有自己的主页了，不需要再次设置主页，直接删除即可。同时删除 `app/Http/Controllers/HomeController.php 和 resources/views/home.blade.php` 两个文件:`rm -f app/Http/Controllers/HomeController.php resources/views/home.blade.php`;由于删除了 `/home` 这个路由，因此需要将 `app/Http/Providers/RouteServiceProvider.php` 的类常量 HOME 改为 '/'。
- 把顶部导航的登录和注册按钮指向真实的地址:在_`herader.blade.php`文件

 - 执行数据库迁移创建对应的数据库表结构:`php artisan migrate`
   	

#### 	2. 验证邮箱:

- 验证邮箱是各种系统很常见的一个功能。用户注册时，系统会往用户邮箱发送一封带有验证链接的邮件，用户点击该链接即可证明这个邮箱是真实存在并且被对应的用户所拥有。

 - 在开始开发之前，为了遵循开发规范，需要将 `App\User` 这个类移动到 `App\Models\User`，全局搜索 `App\User`：全部替换为`App\Models\User`,创建`app/Models` 目录，并将 `app/User.php` 移动到该目录下： `mkdir -p app/Models && mv app/User.php app/Models`;最后修改 `app/Models/User.php` 的命名空间，将 `namespace App`; 改成 `namespace App\Models;`
   	从 `Laravel` `5.7` 起，`Laravel` 自带了邮箱验证的相关字段和功能，只需要稍微调整即可。
   首先调整一下 User 模型类：加上`class User extends Authenticatable implements MustVerifyEmail;`由于 `Laravel` 在创建 User 类时就默认引入了 `MustVerifyEmail` 的命名空间，因此 不需要再次引入。
 - 调整路由:在`web.php`
   	- 在之前的路由里加上一个 verify 参数`Auth::routes(['verify' => true]);`
      	- 验证邮箱中间件:`Laravel` 自带了一个名为 verified 的中间件，如果一个未验证邮箱的用户尝试访问一个配置了 verified 中间件的路由，`Laravel` 就会提示该用户邮箱未激活。
      	- 配置`MailHog:MailHog` 是 Homestead 自带的一个组件，可以很方便地调试发送邮件。在站点后加上:8025端口即可;修改`.env`文件,把 SMTP 服务器地址设置成 127.0.0.1，端口设为 1025：

#### 	3. 收货地址列表

> 先整理好user_addresses表的字段名称和类型

| 字段名称      | 描述             | 类型               | 加索引缘由 |
| ------------- | ---------------- | ------------------ | ---------- |
| id            | 自增长 ID        | `unsigned big int` | 主键       |
| user_id       | 该地址所属的用户 | `unsigned big int` | 外键       |
| province      | 省               | `varchar`          | 无         |
| city          | 市               | `varchar`          | 无         |
| district      | 区               | `varchar`          | 无         |
| address       | 具体地址         | `varchar`          | 无         |
| zip           | 邮编             | `unsigned int`     | 无         |
| contact_name  | 联系人姓名       | `varchar`          | 无         |
| contact_phone | 联系人电话       | `varchar`          | 无         |
| last_used_at  | 最后一次使用时间 | `datetime null`    | 无         |

1. **创建模型:** 通过`make:model`命令:`art make:modle Models/UserAddress -fm ; -fm` 参数代表同时生成 `factory` 工厂文件和 `migration` 数据库迁移文件; 按照字段编写迁移文件 

   ```
   public function up()
       {
           Schema::create('user_addresses', function (Blueprint $table) {
               $table->bigIncrements('id');
               $table->unsignedBigInteger('user_id');
               $table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');
               $table->string('province');
               $table->string('city');
               $table->string('district');
               $table->string('address');
               $table->unsignedInteger('zip');
               $table->string('contact_name');
               $table->string('contact_phone');
               $table->dateTime('last_used_at')->nullable();
               $table->timestamps();
           });
       }
   ```

   

2. 默认的自增ID的字段类型从原本的int 改成了 big int，相应的 migration 代码应该使用 `bigIncrements()` 方法，定义对应自增 ID 的外键时也需要使用 `unsignedBigInteger()` 方法。
       修改模型文件:
       `protected $dates = ['last_used_at'];` 表示 `last_used_at` 字段是一个时间日期类型，在之后的代码中 `$address->last_used_at` 返回的就是一个时间日期对象（确切说是 Carbon 对象，Carbon 是 `Laravel` 默认使用的时间日期处理类）。user()方法 与 User 模型关联，关联关系是一对多（一个 User 可以有多个 `UserAddress`，一个 `UserAddress` 只能属于一个 User）。`return $this->belongsTo(User::class);`
   `getFullAddressAttribute()`方法 创建了一个访问器，在之后的代码里可以直接通过 `$address->full_address` 来获取完整的地址，而不用每次都去拼接。 `return "{$this->province}{$this->city}{$this->district}{$this->address}";`

3. **在`Uesr`模型关联`UserAddress`模型,之后执行迁移:`art migrate;` **

    - 创建控制器,添加index方法返回view()方法;创建视图,创建路由:

    - `auth` 中间件代表需要登录，verified中间件代表需要经过邮箱验证

      ```php
      Route::group(['middleware' => ['auth', 'verified']], function() {
          Route::get('user_addresses', 'UserAddressesController@index')->name('user_addresses.index');
      });
      ```

4. **工厂文件:**在编写工厂文件之前需要修改一项配置,因为factory工厂文件会使用faker来自动生成字段的内容,而默认的情况下生成的数据都是英文的,需要修改为中文:在`config/app.php` 修改`'faker_locale' => 'zh_CN',`
   	**编辑工厂文件:**

   ```php
   1. <?php
   
   use App\Models\UserAddress;
   use Faker\Generator as Faker;
   
   $factory->define(UserAddress::class, function (Faker $faker) {
       $addresses = [
           ["北京市", "市辖区", "东城区"],
           ["河北省", "石家庄市", "长安区"],
           ["江苏省", "南京市", "浦口区"],
           ["江苏省", "苏州市", "相城区"],
           ["广东省", "深圳市", "福田区"],
       ];
       $address   = $faker->randomElement($addresses);
   
   ​    return [
   ​        'province'      => $address[0],
   ​        'city'          => $address[1],
   ​        'district'      => $address[2],
   ​        'address'       => sprintf('第%d街道第%d号', $faker->randomNumber(2), $faker->randomNumber(3)),
   ​        'zip'           => $faker->postcode,
   ​        'contact_name'  => $faker->name,
   ​        'contact_phone' => $faker->phoneNumber,
   ​    ];
   });	
   ```

   > 预先设置了一批省市区,通过`randomElement()`方法随机取出一个.

	5. **新增收货地址**

 - 在`UserAddressController`控制器中添加`create()`方法: 由于新增页面和编辑页面类似,公用一个模板文件,`create_and_edit`,返回视图渲染; 添加对应的路由:在`auth`认证内`route::get('user_address/create')`

 - 省市区联动组件:`yarn add china-area-data`; 这个 `nodejs` 库包含了（基本上）最新的中国省市区的数据，通过类似邮编的方式将上下级关联起来,创建一个`Vue`组件: `touch resources/js/components/SelectDistrict.js:`

   ```
    - // 从刚刚安装的库中加载数据
      const addressData = require('china-area-data/v3/data');
      // 引入 lodash，lodash 是一个实用工具库，提供了很多常用的方法
      import _ from 'lodash';
   
   // 注册一个名为 select-district 的 Vue 组件
   Vue.component('select-district', {
     // 定义组件的属性
     props: {
       // 用来初始化省市区的值，在编辑时会用到
       initValue: {
         type: Array, // 格式是数组
         default: () => ([]), // 默认是个空数组
       }
     },
     // 定义了这个组件内的数据
     data() {
       return {
         provinces: addressData['86'], // 省列表
         cities: {}, // 城市列表
         districts: {}, // 地区列表
         provinceId: '', // 当前选中的省
         cityId: '', // 当前选中的市
         districtId: '', // 当前选中的区
       };
     },
     // 定义观察器，对应属性变更时会触发对应的观察器函数
     watch: {
       // 当选择的省发生改变时触发
       provinceId(newVal) {
         if (!newVal) {
           this.cities = {};
           this.cityId = '';
           return;
         }
         // 将城市列表设为当前省下的城市
         this.cities = addressData[newVal];
         // 如果当前选中的城市不在当前省下，则将选中城市清空
         if (!this.cities[this.cityId]) {
           this.cityId = '';
         }
       },
       // 当选择的市发生改变时触发
       cityId(newVal) {
         if (!newVal) {
           this.districts = {};
           this.districtId = '';
           return;
         }
         // 将地区列表设为当前城市下的地区
         this.districts = addressData[newVal];
         // 如果当前选中的地区不在当前城市下，则将选中地区清空
         if (!this.districts[this.districtId]) {
           this.districtId = '';
         }
       },
       // 当选择的区发生改变时触发
       districtId() {
         // 触发一个名为 change 的 Vue 事件，事件的值就是当前选中的省市区名称，格式为数组
         this.$emit('change', [this.provinces[this.provinceId], this.cities[this.cityId], this.districts[this.districtId]]);
       },
     },
     // 组件初始化时会调用这个方法
     created() {
       this.setFromValue(this.initValue);
     },
     methods: {
       // 
       setFromValue(value) {
         // 过滤掉空值
         value = _.filter(value);
         // 如果数组长度为0，则将省清空（由于 定义了观察器，会联动触发将城市和地区清空）
         if (value.length === 0) {
           this.provinceId = '';
           return;
         }
         // 从当前省列表中找到与数组第一个元素同名的项的索引
         const provinceId = _.findKey(this.provinces, o => o === value[0]);
         // 没找到，清空省的值
         if (!provinceId) {
           this.provinceId = '';
           return;
         }
         // 找到了，将当前省设置成对应的ID
         this.provinceId = provinceId;
         // 由于观察器的作用，这个时候城市列表已经变成了对应省的城市列表
         // 从当前城市列表找到与数组第二个元素同名的项的索引
         const cityId = _.findKey(addressData[provinceId], o => o === value[1]);
         // 没找到，清空城市的值
         if (!cityId) {
           this.cityId = '';
           return;
         }
         // 找到了，将当前城市设置成对应的ID
         this.cityId = cityId;
         // 由于观察器的作用，这个时候地区列表已经变成了对应城市的地区列表
         // 从当前地区列表找到与数组第三个元素同名的项的索引
         const districtId = _.findKey(addressData[cityId], o => o === value[2]);
         // 没找到，清空地区的值
         if (!districtId) {
           this.districtId = '';
           return;
         }
         // 找到了，将当前地区设置成对应的ID
         this.districtId = districtId;
       }
     }
   });
   在app.js中引入这个组件: resources/js/app.js:
   // 此处需在引入 Vue 之后引入
   require('./components/SelectDistrict');
   
   const app = new Vue({
       el: '#app'
   });
   ```

   

 - 把`Vue`组件放到模板:`<!-- inline-template 代表通过内联方式引入组件 -->:<select-district inline-template>` 这一行，`select-district` 就是组件名称，之前在 `JS` 代码里通过 `Vue.component('select-district', { // })` 定义的。`inline-template` 则代表被 `<select-district inline-template></select-district>` 包裹的代码会作为这个组件的模板，也称为 内联模板。

 - 将省市区组件的数据放到表单里提交给后端,创建一个新的`Vue`组件来监听`select-district` 的 `change` 事件：

   ```vue
   // 注册一个名为 user-addresses-create-and-edit 的组件
   Vue.component('user-addresses-create-and-edit', {
     // 组件的数据
     data() {
       return {
         province: '', // 省
         city: '', // 市
         district: '', // 区
       }
     },
     methods: {
       // 把参数 val 中的值保存到组件的数据中
       onDistrictChanged(val) {
         if (val.length === 3) {
           this.province = val[0];
           this.city = val[1];
           this.district = val[2];
         }
       }
     }
   });
   ```

      		*其中 `onDistrictChanged`() 方法将用于处理 select-district 组件抛出的 change 事件，把事件的数据复制到本组件中。然后在`app.js`中引入这个组件;*

 - 把组件放到视图:`<select-district @change="onDistrictChanged" inline-template>` 可以看到比之前多了一个 `@change="onDistrictChanged"`，代表 `select-district` 的 `change` 事件将由 `onDistrictChanged` 这个方法来处理。

 - 当用户选择了省市区之后，触发了 `select-district` 组件的 `change` 事件，被 `user-addresses-create-and-edit` 的 `onDistrictChanged` 捕获之后就会更新 `user-addresses-create-and-edit` 组件内的数据，然后就映射到 3 个隐藏的 `input` 中去

 - 提交表单: 保存用户提交的数据,根据编码规范,需要在`Request`类中完成数据校验,先创建一个Request基类:`art make:request` `Request;authorize()`方法`return true`;创建`UserAddressRequest`类并继承基类:`art make:request UserAddressRequest` 在rules()方法定义规则;

 - 在控制器中创建store()方法注入`UserAddressRequest` 会自动调用 `UserAddressRequest` 中的 rules() 方法获取校验规则来对用户提交的数据进行校验，因此这里不需要手动调用 `$this->validate()` 方法。:`$request->user()->address()->create($request->only([data])) $request->user()` 获取当前登录用户。`user()->addresses()` 获取当前用户与地址的关联关系（注意：这里并不是获取当前用户的地址列表）`addresses()->create()` 在关联关系里创建一个新的记录`$request->only()` 通过白名单的方式从用户提交的数据里获取所需要的数据。`return redirect()->route('user_addresses.index')`; 跳转回 的地址列表页面。

 - 创建相应的路由:在`auth`认证里面 `Route::post('user_addresses', 'UserAddressesController@store')->name('user_addresses.store')`;
   	中文语言包`composer require overtrue/laravel-lang`  在安装成功后，在 `config/app.php` 替换 `Illuminate\Translation\TranslationServiceProvider::class,` 替换为`Overtrue\LaravelLang\TranslationServiceProvider::class`,将`config/app.php:'locale' => 'zh-CN'`,

 -  在Request类定义attributes()方法指定字段的名称;在前端页面添加页面入口;

6. **修改和删除收货地址**

 - 在`UseraddressController`类新增`edit()`方法注入`UserAddress`模型,返回视图;新增路由:在`auth`认证 `Route::get('user_addresses/{user_address}', 'UserAddressesController@edit')->name('user_addresses.edit')`;控制器参数名必须和路由中参数名一致;

 - 修改收货地址页面路由 ,顶部标题判断`id`是修改还是新增;调整省市区组件,由于 `Html` 里的标签和属性不区分大小写，因此不能用 `:initValue="xxxx"` 来给组件传属性，这样传到 `Vue` 里就变成了 `initvalue=xxx`。而 `Vue` 有个特性则是把 `Html` 属性名称里的 -{小写字母} 变成大写字母，也就是 `Html` 属性 `init-value` 对应 `Vue` 里的 `initValue` 属性。在 `init-value` 前面有一个 :，这是 `Vue` 中 `v-bind:` 的缩写，展开来就是 `v-bind:initValue="xxxx"`，这用来指明后面的值是一个 表达式 而非 字符串。在定义 `initValue` 属性的时候指定了这个属性是一个数组，所以传入的是一个由省市区名称组成的数组的 `JSON` 字符串，`Vue` 会把这个 `JSON` 字符串解析成数组赋值给组件内的 `initValue`。需要修改一下表单的提交地址:如果当前的 `$address` 变量有 `id` 属性，说明正在进行地址修改，需要把表单的 `action` 改成对应的 `PUT` 地址。

 - `method_field('PUT')` 会在页面中插入一个隐藏的 `input` 来告诉 `Laravel` 把这个表单的请求方式当成 `PUT`。

 - 控制器添加`update()`方法,注入`UserAddress`模型,和`UserAddressRequest`验证类`:$user_addres->update($request->only([data]));return redirect()->route('')`成功跳转页面地址;新增路由在`Route::put('user_addresses/{user_address}', 'UserAddressesController@update')->name('user_addresses.update');`
   	删除地址:新增`destroy()`方法注入`UserAddre`模型,使用`delete()`方法,返回跳转页面;新增按钮`Route::delete('user_addresses/{user_address}', 'UserAddressesController@destroy')->name('user_addresses.destroy')`;在页面FORM表单提交的方式`delete`使用`{{ csrf_field() }}`
       `{{ method_field('DELETE') }}`

 - 优化交互:但是如果用户不小心点了删除按钮，对应的地址就立即被删除了，这样的用户体验很不好，所以 需要加一个二次确认的操作。通过 `yarn` 引入 `sweetalert` 这个库，`sweetalert` 可以用来展示比较美观的弹出提示框： `yarn add sweetalert`;然后在 `bootstrap.js` 引入这个库：`require('sweetalert')`;保持`nrwp`运行,在页面上通过 `JS` 来调用 `sweetalert` 弹出二次确认提示框;为了让页面的 `Html` 结构更合理，在 `app.blade.php` 中添加一个 @yield 命令:`<script src="{{ mix('js/app.js') }}"></script>`
   `@yield('scriptsAfterJs')`然后就可以在模板中使用 `@section` 命令，使 `@section` 命令的内容出现在刚刚 `@yield` 命令所在的位置：

   ```
   .
   .
   .
   <a href="{{ route('user_addresses.edit', ['user_address' => $address->id]) }}" class="btn btn-primary">修改</a>
   <!-- 把之前删除按钮的表单替换成这个按钮，data-id 属性保存了这个地址的 id，在 js 里会用到 -->
   <button class="btn btn-danger btn-del-address" type="button" data-id="{{ $address->id }}">删除</button>
   .
   .
   .
   @section('scriptsAfterJs')
   <script>
   $(document).ready(function() {
     // 删除按钮点击事件
     $('.btn-del-address').click(function() {
       // 获取按钮上 data-id 属性的值，也就是地址 ID
       var id = $(this).data('id');
       // 调用 sweetalert
       swal({
           title: "确认要删除该地址？",
           icon: "warning",
           buttons: ['取消', '确定'],
           dangerMode: true,
         })
       .then(function(willDelete) { // 用户点击按钮后会触发这个回调函数
         // 用户点击确定 willDelete 值为 true， 否则为 false
         // 用户点了取消，啥也不做
         if (!willDelete) {
           return;
         }
         // 调用删除接口，用 id 来拼接出请求的 url
         axios.delete('/user_addresses/' + id)
           .then(function () {
             // 请求成功之后重新加载页面
             location.reload();
           })
       });
     });
   });
   </script>
   @endsection
   ```



- 删除接口的请求方式从表单提交改成了 AJAX 请求，因此还需调整一下删除接口的返回值，否则 AJAX 请求会有异常：在控制器内 跳转更换return []空数组;

- 检查权限:增加权限控制，只允许地址的拥有者来修改和删除地址，通过授权策略类（Policy）来实现权限控制：`art make:policy UserAddressPolicy`新创建的 Policy 文件位于 `app/Policies` 目录下。在 `UserAddressPolicy` 类中新建一个 `own()` 方法注入User模型,`Useraddress`模型：`return $address->user_id == $user->id`;当 `own()` 方法返回 `true` 时代表当前登录用户可以修改对应的地址。接下来还需要在 `AuthServiceProvider` 注册这个授权策略，从 `Laravel 5.8` 起，可以定义一个回调函数来让 `Laravel` 自己去寻找模型所对应的授权策略文件：`AuthServiceProvider.php`文件boot()方法:

  ```
  $this->registerPolicies();
          // 使用 Gate::guessPolicyNamesUsing 方法来自定义策略文件的寻找逻辑
          Gate::guessPolicyNamesUsing(function ($class) {
              // class_basename 是 Laravel 提供的一个辅助函数，可以获取类的简短名称
              // 例如传入 \App\Models\User 会返回 User
              return '\\App\\Policies\\'.class_basename($class).'Policy';
  ```

  在控制器添加检验权限代码: `$this->authorize('own',第二个参数会获取类名`)然后执行之前在 `AuthServiceProvider` 类中定义的自动寻找逻辑，在这里找到的类就是 `App\Policies\UserAddressPolicy`，之后会实例化这个策略类，再调用名为 `own()` 方法，如果 own() 方法返回 false 则会抛出一个未授权的异常。



***



### 四丶管理后台:

#### 	1. 安装`laravel-admin`扩展包

- composer require encore/laravel-admin "1.7.7"`:`encore/laravel-admin` 是一个可以快速构建后台管理的扩展包，它提供了页面组件和表单元素等功能，只需要使用很少的代码就实现功能完善的后台管理功能。之后需要执行:

 `php artisan vendor:publish --provider="Encore\Admin\AdminServiceProvider"`命令会将 `Laravel-Admin` 的一些文件发布到项目目录中，比如前端 `JS/CSS` 文件、配置文件等。

- `php artisan admin:install`:执行数据库迁移、创建默认管理员账号、默认菜单、默认权限以及创建一些必要的目录。
- `app/Admin/` 是用来放置管理后台的控制器和路由的目录；
- `config/admin.php` 是 `laravel-admin` 的配置文件;
- `database/migrations/2016_01_04_173148_create_admin_tables.php` 用来创建与后台用户、角色、权限相关的数据库表；
- `public/vendor/` 是 `laravel-admin` 会用到的一些前端库；
- `resources/lang/*` 是语言文件,不需要除简体中文以外的语言，可以用下面命令删掉：

除简体中文其他语言文件都不要

`rm -rf resources/lang/ar/ resources/lang/az/ resources/lang/en/admin.php resources/lang/es/ resources/lang/fr/ resources/lang/he/ resources/lang/ja/ resources/lang/nl/ resources/lang/pl/ resources/lang/pt/ resources/lang/ru/ resources/lang/tr/ resources/lang/zh-TW/ resources/lang/pt-BR/ resources/lang/fa/ resources/lang/id/ resources/lang/ms/ resources/lang/uk/ resources/lang/ko/`

- `Laravel-Admin` 的控制器创建方式与普通的控制器创建方式不太一样，要用 `admin:make` 来创建：
  `php artisan admin:make UsersController --model=App\\Models\\User`其中 --`model=App\\Models\\User` 代表新创建的这个控制器是要对 `App\Models\User` 这个模型做增删改查。
- 管理后台的路由文件路径是 `app/Admin/routes.php`

#### 	2.用户列表

- 后台创建控制器的方式和普通的不一样,要用`admin:make`创建:`php artisan admin:make UsersController --model=App\\Models\\User ;--model=App\\Models\\User`参数 代表新创建的这个控制器是要对 `App\Models\User` 这个模型做增删改查。控制器内`protected $title = 'App\Models\User'`; 表示这个页面的标题。

- 其中 `form()` 方法用于编辑和创建用户，由于不会在管理后台去新增和编辑用户，所以可以把这个方法删除。

- `detail()` 方法用来展示用户详情页，通过调用 detail() 方法来决定要展示哪些字段，`Laravel-Admin` 会通过读取数据库自动把所有的字段都列出来，由于用户表没有太多多余的字段，在列表页就可以直接展示，因此可以把 detail() 方法也删掉。

- `grid()` 方法来决定列表页要展示哪些列，以及各个字段对应的名称，`Laravel-Admin` 会通过读取数据库自动把所有的字段都列出来。

  1. 调整列表页面:在`grid()`方法 创建一个列名为 ID 的列，内容是用户的 id 字段:`$grid->id('ID');`创建一个列名为 用户名 的列，内容是用户的 `name` 字段。下面的 email() 和 created_at() 同理

     ```
     - $grid->name('用户名');
               $grid->email('邮箱');
               $grid->email_verified_at('已验证邮箱')->display(function ($value) {
                   return $value ? '是' : '否';
               });
     
     ​        $grid->created_at('注册时间');
     ```

     

  2. 不在页面显示 `新建` 按钮，因为不需要在后台新建用户:`$grid->disableCreateButton()`;
     同时在每一行也不显示 `编辑` 按钮: `$grid->disableActions()`;

     ```php
     $grid->tools(function ($tools) {
                 // 禁用批量删除按钮
                 $tools->batch(function ($batch) {
                     $batch->disableDelete();
                 });
             });
     	return $gird
     ```

  3. 由于并不关心用户验证邮箱的时间点，即 `email_verified_at` 字段的具体值，只需要知道用户是否已经验证过邮箱，所以调用了 `display()` 方法来更优雅地展示。`display()` 方法接受一个匿名函数作为参数，在展示时会把对应字段值当成参数传给匿名函数，把匿名函数的返回值作为页面输出的内容。在这个例子里就是当 `email_verified_at` 有值时展示 是，即验证过邮箱，否则展示 否

  4. 添加路由: 后台的路由文件路径是`app/Admin/routes.php;$router->get('users', 'UsersController@index');`

  5. 添加菜单:可以直接在 `Laravel-Admin` 后台添加菜单：打开左侧菜单的 系统管理 -> 菜单，进入菜单管理页面。页面拉到下方的 新增 版块，标题填入 用户管理，图表选择 `fa-users`，路径填 `/users`，点击提交。

#### 3. 权限设置

	1. 新建权限:进入管理后台，点击左侧菜单的 系统管理 -> 权限，点击 新增 按钮：标识 是用来标记权限的唯一标识，全局唯一。 名称 是这个权限的展示名称，要让人一眼看明白这个权限是做什么用的。如果用户访问的路由与 HTTP方法 和 HTTP路径 相匹配，则会检查该用户是否拥有本权限， 这里只需要使用 /users* 即可匹配所有用户管理相关的路由。然后点击 提交 按钮。
 	2. 新增角色:点击左侧菜单的 系统管理 -> 角色，点击 新增 按钮.标识 是用来标记角色的唯一标识，全局唯一。 名称 是这个角色的展示名称。权限 要点击选择 Login / User setting / Dashboard / 用户管理，前两个权限是必须的，否则该用户将无法登录后台和修改资料，第三个权限是管理后台的首页，如果没有这个权限，在登录的时候会报错.然后点击 提交 按钮。
 	3. 新增管理后台用户:点击左侧菜单的 系统管理 -> 管理员，点击 新增 按钮：用户名 即该用户的登录用户名，全局唯一。 名称 是这个用户的展示名称。角色 选择 刚刚创建的 运营。权限 留空。这里通过角色来把用户和权限关联起来，而不是直接把用户和权限关联，这是因为运营的角色可能会有多个用户，假如运营角色的权限有变化， 只需要修改运营角色的权限而不需要去修改每个运营用户的权限。然后点击 提交 按钮。

### 三丶商品模块:

#### 1.商品的数据结构设计

 1. **商品的数据结构和设计**

    - `sku:Stock Keeping Unit（`库存量单位），也可以称为『单品』。对一种商品而言，当其品牌、型号、配置、等级、花色、包装容量、单位、生产日期、保质期、用途、价格、产地等属性中任一属性与其他商品存在不同时，可称为一个单品。

    - 商品就是 `iPhone 8`，不同的版本、不同的存储容量所对应的具体型号就是这个商品的 `SKU`，比如 `iPhone 8 - 无需合约版 - 红色 - 64G` 就是一个 `SKU`，`iPhone 8 - 无需合约版 - 红色 - 256G` 是另外一个 `SKU`，不同的 `SKU` 价格可能不一样，库存也不一样。

      商家在管理库存的时候就是以 `SKU` 为单位来操作的，比如入库时会写清楚 “购入 `iPhone` 8 - 无需合约版 - 红色 - `256G` 10 台，购入 `iPhone` 8 - 无需合约版 - 红色 - `64G` 10 台” 而不是 “购入 `iPhone` 20 台”。

 2. **数据表设计**

​	`products` 表：

| 字段名称     | 描述                 | 类型                    | 加索引缘由 |
| ------------ | -------------------- | ----------------------- | ---------- |
| id           | 自增长 ID            | unsigned big int        | 主键       |
| title        | 商品名称             | `varchar`               | 无         |
| description  | 商品详情             | text                    | 无         |
| image        | 商品封面图片文件路径 | `varchar`               | 无         |
| on_sale      | 商品是否正在售卖     | tiny int, default 1     | 无         |
| rating       | 商品平均评分         | float, default 5        | 无         |
| sold_count   | 销量                 | unsigned int, default 0 | 无         |
| review_count | 评价数量             | unsigned int, default 0 | 无         |
| price        | SKU 最低价格         | decimal                 | 无         |

​	`product_skus` 表：

| 字段名称    | 描述        | 类型              | 加索引缘由 |
| ----------- | ----------- | ----------------- | ---------- |
| id          | 自增长 ID   | unsigned big int  | 主键       |
| title       | `SKU` 名称  | `varchar`         | 无         |
| description | `SKU` 描述  | `varchar`         | 无         |
| price       | `SKU` 价格  | decimal           | 无         |
| stock       | 库存        | `unsigne` int     | 无         |
| product_id  | 所属商品 id | `unsigne` big int | 外键       |

- 电商项目中与钱相关的有小数点的字段一律使用 decimal 类型，而不是 float 和 double，后面两种类型在做小数运算时有可能出现精度丢失的问题，这在电商系统里是绝对不允许出现的。
- 定义 decimal 字段时需要两个参数，一个是数值总的精度（整数位 + 小数位），另一个参数则是小数位。对于 这个系统来说总精度 10、小数位精度 2 即可满足需求（约 1 亿）。

​	3. **创建模型:**

`php artisan make:model Models/Product -mf`;`php artisan make:model Models/ProductSku -mf`
按照字段类型修改迁移文件;修改模型文件:`product.php`: 可访问的属性 `$faillable,`$casts`声明`on_sale`是一个布尔类型的字段,与商品`sku`关联:``return $this->hasMany(ProductSku::class); productSku:``新建``product()``方法: ``return $this->belongsTo(Product::class)``;然后执行迁移: ``art migrate``

​	4. **后台商品列表**

- 创建控制器:`art admin:make ProductsController --model=App\\Models\\Product`;解决商品列表的展示,调整grid()方法:每一个$grid->调用,对应后台表格里的一个列显示;配置路由:`$router->get('products', 'ProductsController@index')`;在后台添加商品管理的菜单;

- 后台创建和编辑商品

   - 开发的是 `B2C` 商城，可以理解为『互联网超市』。管理员在后台录入商品、品种和价格，用户在网站页面上购买。实现后台创建商品页面以及处理创建商品逻辑;

   - 修改`ProductsController的form()`方法:通过 `rules('required')` 方法可以定义对应字段在提交时的校验规则，验证规则与 `Laravel` 的验证规则一致。

   - `$form->radio('on_sale', '上架')` 在表单中创建一组单选框，`options(['1' => '是', '0'=> '否'])` 设置两个选项，`default('0')` 代表默认选择值为 0 的框，在 这里就是默认为 否。

   - `$form->hasMany('skus', 'SKU 列表', /**/)` 可以在表单中直接添加一对多的关联模型，商品和商品 `SKU` 的关系就是一对多，第一个参数必须和主模型中定义此关联关系的方法同名， 之前在 `App\Models\Product 类中定义了 skus()` 方法来关联 `SKU`，因此这里 需要填入 `skus`，第二个参数是对这个关联关系的描述，第三个参数是一个匿名函数，用来定义关联模型的字段。

   - `$form->saving()` 用来定义一个事件回调，当模型即将保存时会触发这个回调。 需要在保存商品之前拿到所有 `SKU` 中最低的价格作为商品的价格，然后通过 `$form->model()->price` 存入到商品模型中。
     `collect()` 函数是 `Laravel` 提供的一个辅助函数，可以快速创建一个 `Collection` 对象。在这里 把用户提交上来的 `SKU` 数据放到 `Collection` 中，利用 `Collection` 提供的 `min()` 方法求出所有 `SKU` 中最小的 `price`，后面的 `?: 0` 则是保证当 `SKU` 数据为空时 `price` 字段被赋值 0。`where(Form::REMOVE_FLAG_NAME, 0)` 这个代码的详细解释可以查看这篇帖子： https://learnku.com/laravel/t/13432/once-the-product-is-created-edit-it-again-find-the-lowest-price-sku-directly-and-save-it-the-price-of-the-commodity-database-will-not-be-updated-how-can-i-solve-it-thank-you;

   - 添加路由: 

     ```php
     $router->get('products/create', 'ProductsController@create');
     $router->post('products', 'ProductsController@store');
     ```

   - 在 `ProductsController` 中并没有 `store` 方法，这是因为 `Laravel-Admin` 在控制器的父类 `Encore\Admin\Controllers\AdminController` 引入了 `HasResourceActions` 这个 `Trait`，打开 `Encore\Admin\Controllers\HasResourceActions` 这个类可以看到里面定义了 `store` 方法：

   -  后台点击新增按钮会报错:这是因为 `Laravel-Admin` 为了避免加载太多前端静态文件，默认禁用了 `editor` 这个表单组件， 可以在 `app/Admin/bootstrap.php` 中把这个禁用解除：把`Encore\Admin\Form::forget(['map', 'editor'])`修改为`Encore\Admin\Form::forget(['map'])`;发现还是报错，通过查看 `Laravel-Admin` 的文档发现，富文本编辑框组件在 `v1.7.0` 版本之后移除，需要手动安装，执行:`composer require jxlwqq/quill;php artisan vendor:publish --tag=laravel-admin-quill;`然后在 `config/admin.php` 的最下方找到 `extensions` 段，新增文档`quill`数组;然后把 `ProductsController` 的 `form()` 方法中的 `editor` 替换为 `quill`：上传文件的都是存储在 `storage` 目录下，而 HTTP 服务器指向的根目录是 `public` 目录，要想用户能通过浏览器访问 `storage` 目录下的文件，需要创建一个软链接，`Laravel` 内置了这个命令：`php artisan storage:link` OR 参考:[https://learnku.com/laravel/t/15545/a-solution-to-upload-symlink-inputoutput-error-is-presented]

> 由于 `Boostrap4` 默认没有引入 icon 相关的 `css`:需要自己引入，这里使用的是 `FontAwesome`:`yarn add @fortawesome/fontawesome-free`

### 四丶购物车&订单模块

#### 1.关闭未支付订单:

- 创建一个任务:`php artisan make:job CloseOrder`,进入创建好的任务类 代表这个类需要被放到队列中执行,而不是触发时立即执行
- 定义一个受保护的变量; 构造函数类注入订单类,并且给定延时参数, 在`handle()`函数内执行逻辑,当队列处理器从队列取出任务时,会调用`handel()`方法  `方法内判断对应的订单是否已经被支付,如果已经支付则不需要关闭订单,直接退出` `通过事务执行sql \Db::transaction() 此方法执行闭包函数 将订单的closed 字段标记为true,即关闭订单使用更新的方法使数据得到改变 循环遍历订单中的商品sku,将订单的数量加回到SKU的库存中 `
- 触发任务: `$this->dispatch(new '任务类'(第一个参数是资源,第二个参数是延迟时间可以在config/app.php设置方便测试))`;
- 需要在.`env`文件先把队列的驱动改成`redis:QUEUE_CONNECTION=redis`;
- 启动队列处理器: `art queue:work`

#### 2.用户订单列表

- 在控制器新增`index()`方法注入`Request`类 `Illuminate\Http\Requet;` `App\Http\Requests\Requset` 是`Illuminate\Http\Request` 的子类，需要校验数据的时候，根据代码规范，是要放在 `Request` 子类中进行的，不需要数据校验的时候就用 `Illuminate\Http\Request`; 
-  `Model::query(`)是一个`Builder`的操作,`Builder` 提供了方便的操作符来处理数据库中的数据。这些操作符大多数可以组合在一起，以充分利用单个查询。例如把数据数组传给 `insert` 操作符，来告诉 `Query Builder` 插入新行,`Query Builder` 将 `insert` 命令转换为特定于 `database.php` 配置文件中指定的数据库的 `SQL` 查询。 作为参数传递给 `insert` 命令的数据将以参数的形式放入 `SQL` 查询中。
- `with()`方法预加载,避免`N+1`问题 用模型关联进行查询获得数据,参数就是模型名字

------

#### 3.订单详情页

- 在控制器中新增`show()`方法注入`Order`模型类;
- 输出数据给前端页面,使用延迟预加载`load()`方法,预延迟加载和预加载的区别是单条数据使用延迟预加载,列表数据使用预加载`with()`方法,
- 前端页面 `php`原生方法: `number_format()`方法是转化成`number`类型加小数点,第一个参数是`number`,第二个参数是小数点几位数,第三个是分隔符,第四个参数为空字符串,没有3个参数只有4个参数,如果一个参数默认去掉小数点;`join()`方法是将一个一维数组转换为字符串,第一个参数是默认为空的字符串,第二个参数是目标数组
- 权限控制 为了安全起见值允许订单的创建者可以看到对应的订单信息,通过授权策略类(Policy)实现 命令是 `art make:policy name`; 需要定义模型策略的自动加载逻辑;在控制器中校检权限 `$this->authorize(name,模型id)` ;



#### 4.业务封装

- `Controller`控制器里包含大量的复制逻辑代码,这不是合理的,应该对逻辑和业务代码进行封装,基于 `SOLID` 原则，使用 `Service` 模式辅助 `Controller`，将相关的业务逻辑封装在不同的 `Service`，方便项目的后期维护。https://www.kancloud.cn/curder/laravel/408485


### 五丶支付模块

#### 	1.扩展包`yansongda/pay`

- 这个支付库封装了支付宝和微信支付的接口,通过这个库不需要去关注不同支付平台的接口差异,使用相同的方法,参数完成支付功能;

- 引入: `composer require yansongda/pay`

- 配置参数: 需要创建一个新的配置文件来保存支付所需的参数,在`config`目录下, 直接`return`所需的参数,例如`app_id,key,log`;

- 容器:将支付操作类实例注入到容器中,就可以直接通过`app('alipay')`来取得相应的实例,不需要每次都重新创建,通常在`AppServiceProvider`的`register()`方法注入实例:往服务容器中注入一个名为`alipay`的单例对象 `$this->app->singleton(name,回调函数)`,第一次从容器取出对象会调用回调函数来生成对应的对象并保存在容器中,之后再去取的时候直接将容器中的对象返回,回调函数内先获取参数,即`config`目录下配置文件,`config('pay.alipay')`;然后需要判断当前项目是否是线上环境, `app()->environment() !== 'production'`,对于支付宝,如果项目运行环境不是线上环境,则启用开发模式`$config['mode']='dev'`,并且将日志级别设置为`DEBUG`,由于微信没有开发模式,所以仅将日志级别设置为`DEBUG`;

  > `ps:laravel-cashier`国外集成扩展包可用于visa支付



#### 	2.订单的支付宝支付

- 获取支付宝沙箱参数:访问 https://openhome.alipay.com/platform/appDaliy.htm?tab=info 设置应用公钥,点击查看秘钥生成方法,可以下载`PSA`秘钥生成工具,复制生成的公钥到支付宝设置应用公钥的窗口;

- 配置参数:在`app/pay.php`放入刚刚生成的公钥 `app_id` 和秘钥;

- 创建控制器:添加`payByAlipay`方法,判断是否属于当前用户:`$this->authorzie()`; 当订单已支付或者关闭 抛出异常:`throw new InvalidRequestException('订单状态错误')`; 调用支付宝的网关支付: `return app('alipay')->web([`
  `'out_trade_no' //订单编号,需保证在客户端不重复`
  `'tatal_amount' //订单金额,单位元,支持小数点后两位`
  `'subject' //订单标题`
  `])`;

- 添加相应的路由:`Route::get('payment/{order}/alipay')`;

- 这时候配置好按钮之后支付成功后会一直停留在支付宝页面没有调整,这是因为没有配置支付的前端回调,到查看订单页面状态还是未支付,这是因为没写更新状态的逻辑:支付宝的回调分为前端和服务器回调; 前端回调是指当用户支付成功后支付宝会让用户浏览器跳转回页面并且带上支付成功的参数,也就是说前端回调依赖用户浏览器,如果用户在跳转之前关闭浏览器,将无法收到前端回调; 服务器回调是指支付成功后支付宝的服务器会用订单相关数据作为参数请求项目的接口,不依赖用户浏览器.因此判断支付是否成功要以服务器端回调为准;

- 前端回调页面:控制内新建方法`alipayReturn()` 检验提交的参数是否合法 `$data = app('alipay')->verify(这个方法用于校检提交的参数是否合法,支付宝前端跳转回带有数据签名,通过校检数据签名可以判断参数是否被恶意用户篡改.同时该方法会返回解析后的参数)`,之后`dd()`看返回什么结果
  	服务端回调:新建方法`alipayNotify() $data=app('alipay')->verify();\Log::debug('Alipay notify',$data->all())`:由于服务端的请求无法看到返回值,所以要通过日志的方式保存;

- 然后注册到路由 认证:`Route::get('payment/alipay/return')`; 因为服务器回调的路由不能放到带有auth()中间件的路由组中,因为支付宝的服务器请求不会带有认证信息:`Route::get('payment/alipay/notify')`;

- 之后把回调地址配置到支付宝的支付实例里: 在`AppServiceProvider.php`的`register()`方法:`$config['notify_url']=route('payment.alipay.notify');$config['return_url']=route('payment.alipay.return')` 因为回调地址必须是完整的带有域名的URL,不可以是相对路径,使用`route()`函数生成的URL默认的就是带有域名的完整地址;

- 因为`shop.test`运行在本地,支付宝服务器无法请求到本地服务器端回调地址,可以通过`requestbin`服务来捕获服务器端回调的数据:`requestbin` 是一个免费开源的网站，任何人都可以在上面申请一个专属的 URL（通常有效期 48 小时），对这个 URL 的任何类型的请求都会被记录下来，URL 的创建者可以看到请求的具体信息，包含请求时间、请求头、请求具体数据等。

- 从`requesbin`的`URL`复制到之前放服务器端调用地址的参数上;然后走一笔订单支付流程之后,在终端使用`CURL`来请求本地服务器端``url`: curl -XPOST http://shop.test/payment/alipay/notify -d`` '复制 `raw body` 内容' 请求内容的两端都有加上单引号;如果返回了一个`html`页面,标题是页面会话已超时,在`laravel`里页面会话已超时通常代表`POST`请求缺少`CSRF TOKEN` 或者过期了,由于这个URL是给支付宝服务器端调用的,所以不会有`CSRF TOKEN` 所以需要把这个`URL`加到`CSRF`白名单里:在`VerifyCsrfToken.php` 的私有变量`$except= ['payment/alipay/notify']`,如果URL能匹配上`$except`里任意一项,`laravel`就不会去检查`CSRF TOKEN` ,在终端再次执行CURL请求;然后看一下日志保存了什么数据 一定要用 `less` 命令查看日志文件，而不是 `vim` 或者其他编辑器。通常来说日志文件会特别大，特别是线上的服务器，如果用 `vim` 或者其他编辑器打开，会把整个日志文件加载到内存中，有可能立马将服务器内存撑爆导致系统故障。`less` 命令则不会，可以理解为按需加载文件内容到内存中。 `less storage/logs/laravel-{当前日期}.log;shift + g`可以跳转到日志文件的末尾;

- 然后完善两个回调接口:
  	`alupayReturn():异常处理 try{app('alipay')->verify()}catch(\Excption $e){return view()}`
  	`alupayNotify():校检输入参数 $data =app('alipay')->verify();`   如果订单状态不是成功或者结束,则不走后续的逻辑;所有交易状态:https://docs.open.alipay.com/59/103672:  

  ```php
  if(!in_array($data->trade_status, ['TRADE_SUCCESS', 'TRADE_FINISHED'])) {
              return app('alipay')->success();
          }
          $data->out_trade_no拿到订单流水号,并在数据库查询;
          
          // 正常来说不太可能出现支付了一笔不存在的订单，这个判断只是加强系统健壮性。
          if (!$order) { return 'fail';}
           // 如果这笔订单的状态已经是已支付
          if ($order->paid_at) {// 返回数据给支付宝
              return app('alipay')->success(); }
              $order->update([
              'paid_at'        => Carbon::now(), // 支付时间
              'payment_method' => 'alipay', // 支付方式
              'payment_no'     => $data->trade_no, // 支付宝订单号
          ]);
   app('alipay')->success() 
  ```

- 返回数据给支付宝，支付宝得到这个返回之后就认为已经处理好这笔订单，不会再发生这笔订单的回调了。如果返回其他数据给支付宝，支付宝就会每隔一段时间就发送一次服务器端回调，直到返回了正确的数据为准。创建一个对应的前端模块再到终端发起回调请求;订单状态就存入数据库中了

#### 3.集成微信支付

  - 访问微信支付商户平台 https://pay.weixin.qq.com/ 微信支付开发需要公众号并且开通了微信支付才能正常进行,微信支付需要公司资质 登录之后点击顶部导航的产品中心,再点击扫码支付,在开发配置上会显示商户号,再到账号中心点击`api`安全,点击下载证书需要后缀为.`pem`的文件,把这两个文件放入项目中,新建目录 `resources/wechat_pay` ,设置秘钥如果是已经在使用的支付号,就不需要设置,密钥可以通过 http://www.unit-conversion.info/texttools/random-string-generator/,长度为32然后创建; 然后把参数写进配置文件 `config/pay.php` ; 由于微信支付配置是正式的参数,如果泄露会导致资金损失,所以不能把参数的文件提交到公共代码块中 命令是 `git update-index --assume-unchanged config/pay.php` 以及修改 `.gitignored`文件

- 订单的微信支付
  - 在`paymentController` 控制器新增`payByWechat()`方法与支付宝类似: 注入订单ID Order;检验权限是否是当前用户,检验订单状态 是否支付或者关闭 抛出异常 throw; scan 方法拉起微信扫码支付,此方法是`yansongda/pay`扩展包的; 回调接口,微信的扫码支付没有前端回调只有服务器端回调:检验回调参数是否正确,找到对应的订单,`Order::where('no',)->first()`;订单不存在则告知微信支付 `return 'fail'`;订单已支付告知微信支付已处理`return app('wechat_pay')->success()`;将订单标记为已支付 `$order->update(['paid_at'=>Carbon::now(),'payment'=>'支付方式','payment_no'=>'支付订单'])` 最后更新状态 `return app('wechat_pay')->success()`;配置路由同支付宝;
  - 配置回调地址:和支付宝一样,回调用 `requestbin` 捕获,回调地址加入`CSRF`校验白名单;前端配置接入
  - 生成二维码: 需要通过代码将 `code_url`转成二维码并输出; 引入`endroid/qr-code`库:`composer require endroid/qr-code` ;在控制器内`payByWechat`方法 把之前返回值放入变量里,把转换的字符串作为`Qrcode`的构造函数参数 `new QrCode($val->code_url)`;将生成的二维码图片以字符串的形式输出,并带上相应的响应类型`:response($val->writeString(),200,['Content-type'=>$val->getContentType()])`; 但是直接跳转到一个二维码图片的页面体验也不好,改成弹框 ,使用`JQ`动态生成弹框`DOM`元素创建`<img>`标签以`[0]`的方法获取数据,`.then`方法内点击按钮重置页面 `location.reload()`;



> `$order->items()` 是获取关联关系，这个时候还没有发生 `SQL` 查询，通常是准备做进一步的查询。`$order->items` 则是获取关联的模型，`SQL` 已经执行完毕，已经从数据库中取到了所有关联的数据。

#### 4.支付后逻辑

	- 支付之后需要补充一些逻辑,比如支付后给订单中的商品增加销量,邮件或短信通知给用户订单支付成功.
	- 支付成功事件: `art make:event OrderPaid` 会自动在`app`目录下新建`event`目录,事件本身不需要有逻辑,只要包含相关的信息即可,  在构造函数内注入订单对象 `Order $order` 然后创建一个方法返回信息; 然后在支付成功的回调里触发这个事件 新建方法注入订单信息 触发的方法是`event(new OrderPaid($order));` 然后实现支付完成事件对应的监听器 创建一个更新商品销量的监听器: `art make:listener UpadateProductSoldCount --envent=OrderPaid` 后面`--envent`参数会自动注入到`handel()`方法内, 会自动在`app`目录下新建`Listener`目录: `implement ShouldQueue` 代表此监听器是异步执行的 异步:同时执行;同步:按先后执行,`laravel` 会默认执行监听器的`handle`方法,触发的事件会作为`handle`方法的参数 `OrderPaid` `$event` ,从事件对象中取出对应的订单 `$order = $event->getOrder()`;预加载商品数据,因为是单条数据使用`load`方法 `load()` 方法支持用 `.` 来加载关联对象的关联对象;循环遍历订单的商品 `$order->items` 订单对应的项目; `$product=$item->product` 项目对应的商品; 计算对应的商品销量 `OrderItem::query()->where('product_id',$product->id) ->whereHas` 关联关系条件 `('order',function($query){$query->whereNotNull('paid_at') 该字段不为空 })->sum 总和('amout')`;更新商品销量 `$product->update(['sold_count'=>$soldCount])`; 然后关联事件和监听器 在`EventServiceProvider`中的`$listen`数组中;
	- 发送通知邮件: 创建通知类,通过`laravel`内置的消息系统(`Notification`)来发送邮件,使用: `art make:notification OrderPaidNotification` 类: 定义保护有变量`$order`;在构造函数内注入订单信息 `Order` ;只需要邮件通知,在`via`方法内`return['mail']`;发送邮件`toMail()` `return(new mailMessage)->subject(邮件标题)->greeting(欢迎词)->line(邮件内容)->action(邮件的按钮及对应的链接)->success(按钮的色调);`
	- 然后创建监听器:`art make:listener SendOrderPaidMail --event=orderPaid`;在`handle()`内:从事件对象中取出相应的订单 `$order=$event->getOrder()`;调用`notify`方法发送通知 `$order->user->notify(new OrderPaidNotification($order))`; 关联事件和监听器 在`EventServiceProvider`中的`$listen`数组中:`SendOrderPaidmail::class` 
	- 由于定义的时间监听器都是异步的,因此在测试之前需要开启队列处理器: `art queue:work` 在终端进入`tinker` 在`tinker`中触发订单支付成功的事件,事件对应的就是在数据库中已经支付成功的订单ID: `event(new App\Events\OrderPaid(App\Models\Order::find()))`;邮件可以在 域名后面加端口`:8025`进入`MailHog`

### 六丶完善订单列表:

#### 1.后台-订单列表:

  - 创建一个管理后台的控制器: `art admin:make OrdersController --model=App\\Models\\Order` 后面`--model`参数自动注入订单的模型, 修改受保护的变量`$title='订单' grid()`方法决定列表页主要展示哪些列,以及各个字段对应的名称,Laravel-Admin会通过读取数据库自动把所有字段列出来;在`grid()`方法: 首先实例化`Grid`之前的`art`命令自动注入了相关的模型: 只展示已支付的订单,并且默认按支付时间倒序排序 `$grid->model()->whereNotNull('paid_at')->orderBy('paid_at','desc');$grid->no('订单流水号')`第一列;展示关联关系的字段时,使用`column`方法`$gird->column('user.name','卖家')`第二列;`$grid->total_coumt('总金额')->sortable()第三列 sortable()`方法是页面可以选择排序方式;`$ship_status('物流')->display(function(){}) dispay()`方法接受一个匿名函数作为参数,在展示时会把对应的字段值当成参数传给匿名函数,把匿名函数的返回值作为页面输出的内容;`$gird->disableCreateButton()`方法是禁用创建按钮,订单页不需要创建;

    ```php
    $grid->actions(function ($actions){
               //禁用删除和编辑按钮
                $actions->disableDelete();
                $actions->disableEdit();
            });
            $grid->tools(function ($tools){
                //禁用批量删除按钮
                $tools->batch(function ($batch){
                    $batch->disableDelete();
                });
            });
    ```

  - 之后添加对应路由:管理后台的路由路径是 :`app/Admin/routes.php`:`get('orders','OrdersController@index')`

  - 然后在后台页面添加订单页面

#### 2.订单详情:

- 订单信息比较多,laravel-Admin的表单形式不能很好的满足需求,采用自定义页面展示订单;
- 在控制器中新增show(给定$id参数会从路由获取,注入Content类)方法 `;ruturn $content->header('查看订单')->body(view('admin.orders.show',['order'=>Order::find($id)]))`; 
- body方法可以接受`laravel`视图作为参数;这样的效果就是页面顶部和左侧都还是`laravel-Admin`原本的菜单,而页面主要内容就变成了`admin.orders.show`渲染的内容;
- 添加对应的路由 `get('orders/{order}')`;

#### 3.订单发货:

- 控制器内新增`ship(注入Order模型id,注入Request不需要认证Illuminate\Http\Request)`方法:判断当前订单是否支付 `$order->paid_at`取反{抛出异常('该订单未付款')};判断当前订单发货状态是否为未发货`$order->ship_status !== Order::SHIP_STATUS_PENDING{抛出异常('该订单已发货'}`;`validate` 方法可以返回校验过的值: `$data = $this->validate(第一个参数是请求的验证数组,第二个参数是$message,第三个参数验证通过返回的数据)`,将订单发货状态改为已发货，并存入物流信息`$order->update(['ship_status'=>Order::SHIP_STATUS_DELIVERED,在 Order 模型的 $casts 属性里指明了 ship_data 是一个数组因此可以直接传过去'ship_data'=>$data])`返回上一页:return `redirect()->back();`定义对应的路由 `post('orders/{order}/ship');`前端页面渲染;因为`Laravel-Admin` 的 `Controller` 基类并没有像 `Laravel` 默认的 `Controller` 基类那样提供了 `validate` 方法，从 `Laravel` 的 `Controller` 基类中把相关的代码复制`use ValidatesRequests;`

#### 4.用户界面:确认收货

- 前端控制器内新增`received(Order,Request)`方法:校检权限 `$this->authorize('own',$order)`;判断订单的发货状态是否为已发货`$order->ship_status !== Order::SHIP_STATUS_DELIVERED{抛出异常('发货状态不正确')}`;更新发货状态为已收到 `$order->update(['ship_status' => Order::SHIP_STATUS_RECEIVED])`;返回订单信息:`return $order`;添加对应的路由`post('orders/{order}/received')`;前端模板:调整一下用户界面的订单详情页模板，展示物流状态，并且当订单中有物流信息时将其展示出来用户端确认收货的时候没有二次确认就直接提交了，这样用户很容易因为误操作就够确认收货了，因此 需要在用户点击确认收货时候弹出一个框让用户确认。

#### 5.评价商品

- 需要对用户提交的数据进行校检,先创建一个Request类:`art make:request SendReviewRequest;`

  ```php
  - public function rules()
        {
            return [
                'reviews'          => ['required', 'array'],
                'reviews.*.id'     => [
                    'required',
                    Rule::exists('order_items', 'id')->where('order_id', $this->route('order')->id)
                ],
                'reviews.*.rating' => ['required', 'integer', 'between:1,5'],
                'reviews.*.review' => ['required'],
            ];
        }
  
  ​    public function attributes()
  ​    {
  ​        return [
  ​            'reviews.*.rating' => '评分',
  ​            'reviews.*.review' => '评价',
  ​        ];
  ​    }
  ```

  

- 涉及验证字段`reviwes,reviews.*.id,reviews.*.rating,review.*.review`,其中.*.是验证数组的形式*就是通配数组的,`Rule::exists()`验证的字段必须在给定的数组中,第二个参数是别名,其中 `$this->route('order')` 可以获得当前路由对应的订单对象，`attributes()`是自定义属性名称,验证消息返回数据.

- 在`OrdersController` 里添加 `review()` 和 `sendReview()` 方法，分别代表展示评价页面和提交评价接口：
  	`review(注入Order)`方法: 1.检验权限,2判断是否已经支付,3使用load方法预加载关联数据,避免N+1性能问题`load('items.productSku','items.product')`;
  	`sendReview(注入Order,SendReviewRequst验证类)`方法:

  	1. 检验权限,
   	2. 判断是否支付,判断是否评价,
   	3. 获取用户提交的数据`$request->input()`;
   	4. 开启事务`\DB::transaction(fuction()use(){ 遍历用户提交的数据,查找order关联的items表id为用户提交的字段$order->items()->find($review['id']),保存评分和评价})`,
   	5. 将订单标记为已评价,
   	6. 返回上一页`return redirect()->back();`

- 创建对应的路由:`review()`方法对应的`get(orders/{order}/review)`;`sendReview()`对应的`post('orders/{order}/review')`;

- 创建前端模板展示评价页面:`str_repeat()`方法是`php`原生的一个方法,作用是重复一个字符串,第一个参数是字符串,第二个参数是被重复的次数必须大于0;

- 更新商品评分:用户用户给商品打完分之后，系统需要重新计算对应商品的评分数据，还是通过事件系统来实现。创建一个订单已评价的时间:`php artisan make:event OrderReviewed`,只需要包含订单数据;监听器:`php artisan make:listener UpdateProductRating --event=OrderReviewed`:

  1. 引入`shouldqueue:impleents ShouldQueue`代表这个事件处理器是异步的,

  2. 通过with()方法提前预加载`$items = $event->getOrder()->items()->with(['product'])->get()`;

  3. 遍历数据,遍历体计算商品的评价数量和评分:

     ```php
     $result = OrderItem::query()
                     ->where('product_id', $item->product_id)
                     ->whereNotNull('reviewed_at')
                     ->whereHas('order', function ($query) {
                         $query->whereNotNull('paid_at');
                     })
                     ->first([
                         DB::raw('count(*) as review_count'),
                         DB::raw('avg(rating) as rating')
                     ]);
                      //first()方法接受一个数组作为参数，代表此次 SQL 要查询出来的字段，默认情况下 Laravel 会给数组里面的值的两边加上 ` 这个符号;所以如果直接传入 first(['count(*) as review_count', 'avg(rating) as rating'])，最后生成的 SQL 肯定是不正确的。这里 用 DB::raw() 方法来解决这个问题，Laravel 在构建 SQL 的时候如果遇到 DB::raw() 就会把 DB::raw() 的参数原样拼接到 SQL 里。
     ```

  4. 更新商品的评分和评价数
      	- 注册事件和处理的关联:   `OrderReviewed::class` => [
                 `UpdateProductRating::class,`
             ],
       - 在控制器触发这个事件:在事务后面加上`event(new OrderReviewd)`;开启队列处理器:`art queue:work`;

- 添加入口,把评价的入口放在订单列表页面;之前在商品展示页面预留了商品评价的列表,在`ProductsController`控制器`show()`方法加上 查询商品评价数量和评分按评价时间倒序取出10条`->get()`方法;注入模板;

#### 6.申请退款

- 创建一个`HandleRefundRequest ApplyRefundRequest` 来校验用户提交的退款申请，要求用户提交退款理由：`php artisan make:request ApplyRefundRequest,`退款请求只需要用户提交退款理由即可;接下来在 `OrdersController` 中添加 `applyRefund(Order $order, ApplyRefundRequest $request)` 方法作为提交退款申请的接口：

  - 1.校检订单是否属于当前用户,

  - 2.判断订单是否已付款,判断订单退款状态是否正确,

  - 3.将用户输入的退款理由放到订单的 `extra` 字段中,判断`$order->extra`是否是数组可以用三元`$order->extra ?: []`,

  - 4.将退款状态改为已申请退款 `$order->update()`;

  - 5.`retrurn $order`;添加对应的路由:`post('orders/{order}/apply_refund')`;

  - 前端页面逻辑:

    - 1.点击按钮事件,`swal`弹框定义`text`,`content`;

    - 2.当用户点击`swal`弹框的按钮触发的函数

      ```javascript
      .then(function (input) {
              // 当用户点击 swal 弹出框上的按钮时触发这个函数
              if(!input) {
                swal('退款理由不可空', '', 'error');
                return;
              }
       // 请求退款接口
              axios.post('{{ route('orders.apply_refund', [$order->id]) }}', {reason: input})
                .then(function () {
                  swal('申请退款成功', '', 'success').then(function () {
                    // 用户点击弹框上按钮时重新加载页面
                    location.reload();
                  });
                });
      ```

- 后台拒绝测试:

  1. 创建一个 `HandleRefundRequest` 来校验运营人员处理退款的请求`:php artisan make:request Admin/HandleRefundRequest` 要继承`App\Http\Requests\Request`类,涉及验证字段`agree`:唯一而且是布尔类型,`reason`:拒绝需要理由`required_if:agree,false`;然后在 `OrdersController` 中添加 `handleRefund(Order $order, HandleRefundRequest $request)` 方法作为处理退款的接口：

     - 判断订单状态是否正确`$order->refund_status !== Order::REFUND_STATUS_APPLIED`,抛出('订单状态不正确')

     - 是否同意退款

       ```php
        if ($request->input('agree')) {
                   // 同意退款的逻辑这里先留空
                   // todo
               } else {
                   // 将拒绝退款理由放到订单的 extra 字段中
                   $extra = $order->extra ?: [];
                   $extra['refund_disagree_reason'] = $request->input('reason');
                   // 将订单的退款状态改为未退款
                   $order->update([
                       'refund_status' => Order::REFUND_STATUS_PENDING,
                       'extra'         => $extra,
                   ]);
               }
       ```

  2. 添加相应的路由:post('order/{order}/refund');

  3.  前端页面逻辑:判断状态是否正确,正确输出状态和理由,判断订单退款状态是已申请，则展示处理按钮`js`逻辑:不同意点击按钮事件{`1.swal弹框 Laravel-Admin 使用的 SweetAlert` 版本与 在前台使用的版本不一样，因此参数也不太一样}
              

     ```javascript
     swal({
           title: '输入拒绝退款理由',
           input: 'text',
           showCancelButton: true,
           confirmButtonText: "确认",
           cancelButtonText: "取消",
           showLoaderOnConfirm: true,
           preConfirm: function(inputValue) {
             if (!inputValue) {
               swal('理由不能为空', '', 'error')
               return false;
             }
             // Laravel-Admin 没有 axios，使用 jQuery 的 ajax 方法来请求
             return $.ajax({
               url: '{{ route('admin.orders.handle_refund', [$order->id]) }}',
               type: 'POST',
               data: JSON.stringify({   // 将请求变成 JSON 字符串
                 agree: false,  // 拒绝申请
                 reason: inputValue,
                 // 带上 CSRF Token
                 // Laravel-Admin 页面里可以通过 LA.token 获得 CSRF Token
                 _token: LA.token,
               }),
               contentType: 'application/json',  // 请求的数据格式为 JSON
             });
           },
           allowOutsideClick: false
         }).then(function (ret) {
           // 如果用户点击了『取消』按钮，则不做任何操作
           if (ret.dismiss === 'cancel') {
             return;
           }
           swal({
             title: '操作成功',
             type: 'success'
           }).then(function() {
             // 用户点击 swal 上的按钮时刷新页面
             location.reload();
           });
         });
       });
     ```

  4. 在用户界面展示退款理由:判断是否设置退款不同意原因

#### 	7.后台-同意付款(支付宝)

 - 支付宝和微信,在申请退款的时候都需要 提交一个唯一字符串作为退款订单号，之后可以通过退款订单号来查询退款进度，退款的回调也会带上退款订单号。在`Order`模型写逻辑:新建`getAvailableRefundNo`:使用

  - `do{1.Uuid`类可以用来生成大概率不重复的字符串`$no = Uuid::uuid4()->getHex();  }while(` `// 为了避免重复 在生成之后在数据库中查询看看是否已经存在相同的退款订单号 self::query()->where('refund_no', $no)->exists())循环 先执行do里面的满足条件循环`
    完善一下之前在`OrdersController` 里的 `handleRefund()` 方法:

    ```php
    在if ($request->input('agree')){
    // 清空拒绝退款理由
            $extra = $order->extra ?: [];
            unset($extra['refund_disagree_reason']);//unset销毁指定的变量。 
            $order->update([
                'extra' => $extra,
            ]);
            // 调用退款逻辑
            $this->_refundOrder($order);
    }，由于调用退款的逻辑比较多，因此单独拆出一个方法 _refundOrder() 来处理：
     protected function _refundOrder(Order $order)
    {
        // 判断该订单的支付方式
        switch ($order->payment_method) {
            case 'wechat':
                // 微信的先留空
                // todo
                break;
            case 'alipay':
                // 用 刚刚写的方法来生成一个退款订单号
                $refundNo = Order::getAvailableRefundNo();
                // 调用支付宝支付实例的 refund 方法
                $ret = app('alipay')->refund([
                    'out_trade_no' => $order->no, // 之前的订单流水号
                    'refund_amount' => $order->total_amount, // 退款金额，单位元
                    'out_request_no' => $refundNo, // 退款订单号
                ]);
                // 根据支付宝的文档，如果返回值里有 sub_code 字段说明退款失败
                if ($ret->sub_code) {
                    // 将退款失败的保存存入 extra 字段
                    $extra = $order->extra;
                    $extra['refund_failed_code'] = $ret->sub_code;
                    // 将订单的退款状态标记为退款失败
                    $order->update([
                        'refund_no' => $refundNo,
                        'refund_status' => Order::REFUND_STATUS_FAILED,
                        'extra' => $extra,
                    ]);
                } else {
                    // 将订单的退款状态标记为退款成功并保存退款订单号
                    $order->update([
                        'refund_no' => $refundNo,
                        'refund_status' => Order::REFUND_STATUS_SUCCESS,
                    ]);
                }
                break;
            default:
                // 原则上不可能出现，这个只是为了代码健壮性
                throw new InternalException('未知订单支付方式：'.$order->payment_method);
                break;
        }
    ```

	-    前端模板:`js`逻辑 
	
	```javascript
      // 同意 按钮的点击事件
        $('#btn-refund-agree').click(function() {
          swal({
            title: '确认要将款项退还给用户？',//弹窗的标题。
            type: 'warning',//弹窗的类型。SweetAlert有四个内置类型，可以展示相应的图标动画: "warning","error", "success" and "info"。你也可以设置为"input"类型变成输入弹窗。可以通过对象的”type”属性或第三个参数进行传递。
            showCancelButton: true,//如果设置为true，“取消”按钮将会显示，用户点击取消按钮会关闭弹窗。
            confirmButtonText: "确认",//使用该参数来修改“确认”按钮的显示文本。如果showCancelButton设置为true，确定按钮的显示文本将会自动使用“Confirm”而不是“OK”。
	        cancelButtonText: "取消",//使用该参数来修改“取消”按钮的显示文本。
	        showLoaderOnConfirm: true,//设置为true，当处于加载时会禁用确认按钮并显示为加载样式。
            preConfirm: function() {//确认之前执行的函数，应返回Promise，
              return $.ajax({
                url: '{{ route('admin.orders.handle_refund', [$order->id]) }}',
                type: 'POST',
                data: JSON.stringify({
                  agree: true, // 代表同意退款
                  _token: LA.token,
                }),
                contentType: 'application/json',
              });
            },
            allowOutsideClick: false//如果设置为false，用户无法通过点击弹窗外部关闭弹窗。
          }).then(function (ret) {
            // 如果用户点击了『取消』按钮，则不做任何操作
            if (ret.dismiss === 'cancel') {
              return;
            }
            swal({
              title: '操作成功',
              type: 'success'
            }).then(function() {
              // 用户点击 swal 上的按钮时刷新页面
              location.reload();
            });
          });
        });
        退款成功之后， 在后台的订单详情页里面还能看到发货的表单，这个不符合逻辑，需要把它隐藏掉:
        <!-- 加上这个判断条件 -->
      @if($order->refund_status !== \App\Models\Order::REFUND_STATUS_SUCCESS)
    ```

#### 	8.后台同意微信退款

	- 完善 `OrdersController` 里面 `_refundOrder()` 之前预留给微信支付退款的地方：
 
     ```php
     case 'wechat':
                // 生成退款订单号
                $refundNo = Order::getAvailableRefundNo();
                app('wechat_pay')->refund([
                    'out_trade_no' => $order->no, // 之前的订单流水号
                    'total_fee' => $order->total_amount * 100, //原订单金额，单位分
                    'refund_fee' => $order->total_amount * 100, // 要退款的订单金额，单位分
                    'out_refund_no' => $refundNo, // 退款订单号
                    // 微信支付的退款结果并不是实时返回的，而是通过退款回调来通知，因此这里需要配上退款回调接口地址
                    'notify_url' => 'http://requestbin.fullcontact.com/******' // 由于是开发环境，需要配成 requestbin 地址
                ]);
                // 将订单状态改成退款中
                $order->update([
                    'refund_no' => $refundNo,
                    'refund_status' => Order::REFUND_STATUS_PROCESSING,
                ]);
                break;
     ```
     
	- 退款回调控制器:由于微信支付的退款结果并不是实时返回的，而是通过退款回调来通知的，所以接下来实现微信退款回调的接口。`PaymentController` 里添加一个 `wechatRefundNotify(Request)` 方法 
 
     ```
      -    // 给微信的失败响应
                $failXml = '<xml><return_code><![CDATA[FAIL]]></return_code><return_msg><![CDATA[FAIL]]></return_msg></xml>';
                $data = app('wechat_pay')->verify(null, true);
     
     ​        // 没有找到对应的订单，原则上不可能发生，保证代码健壮性
     ​        if(!$order = Order::where('no', $data['out_trade_no'])->first()) {
     ​            return $failXml;
     ​        }
     
     ​        if ($data['refund_status'] === 'SUCCESS') {
     ​            // 退款成功，将订单退款状态改成退款成功
     ​            $order->update([
     ​                'refund_status' => Order::REFUND_STATUS_SUCCESS,
     ​            ]);
     ​        } else {
     ​            // 退款失败，将具体状态存入 extra 字段，并表退款状态改成失败
     ​            $extra = $order->extra;
     ​            $extra['refund_failed_code'] = $data['refund_status'];
     ​            $order->update([
     ​                'refund_status' => Order::REFUND_STATUS_FAILED,
     ​                'extra' => $extra
     ​            ]);
     ​        }
     
     ​        return app('wechat_pay')->success();
     ```

   - 退款回调路由:不能放在`auth`中间件中,`post('payment/wechat/refund_notify')`;把路由加到`CSRF`白名单`:app/Http/Middleware/VerifyCsrfToken.php`;在测试支付和退款的时候为了能够获取到回调请求，在很多地方配置了 `requestbin` 的地址，现在需要把这些地方改回正确的地址。

### 七丶优惠卷

### 	1.后台-优惠券列表

| 字段名称   | 描述                                 | 类型                    | 加索引缘由 |
| ---------- | ------------------------------------ | ----------------------- | ---------- |
| id         | 自增长 ID                            | unsigned big int        | 主键       |
| name       | 优惠券的标题                         | `varchar`               | 无         |
| code       | 优惠码，用户下单时输入               | `varchar`               | 唯一       |
| type       | 优惠券类型，支持固定金额和百分比折扣 | `varchar`               | 无         |
| value      | 折扣值，根据不同类型含义不同         | decimal                 | 无         |
| total      | 全站可兑换的数量                     | unsigned int            | 无         |
| used       | 当前已兑换的数量                     | unsigned int, default 0 | 无         |
| min_amount | 使用该优惠券的最低订单金额           | decimal                 | 无         |
| not_before | 在这个时间之前不可用                 | `datetime`, null        | 无         |
| not_after  | 在这个时间之后不可用                 | `datetime`, null        | 无         |
| enabled    | 优惠券是否生效                       | `tinyint`               | 无         |

- 创建一个模型: `php artisan make:model Models/CouponCode -mf,`修改对应的迁移文件
  修改模型文件:加入一些必要的字段信息:用常量定义 `const TYPE_FIXED = 'fixed';const TYPE_PERCENT = 'percent';`新建静态方法  `static $typeMap=[self::TYPE_FIXED   => '固定金额', self::TYPE_PERCENT => '比例',]`,添加白名单: `protected $fillable=[];`声明类型:`protected $casts=['enabled' => 'boolean']`定义后传输的数据就会以布尔类型传输,指明这两个字段是日期类型`protected $dates = ['not_before', 'not_after']`;

- 修改`Order`表,添加一个`coupon_code_id` 字段。创建数据库迁移文件:`php artisan make:migration orders_add_coupon_code_id --table=orders`;在`up()`方法把`coupon_code_id`放到`order`表中,与id设置关联,`onDelete('set null')` 代表如果这个订单有关联优惠券并且该优惠券被删除时将自动把 `coupon_code_id` 设成 `null`。 不能因为删除了优惠券就把关联了这个优惠券的订单都删除了，这是绝对不允许的。在`down()`方法`dropForeign()` 删除外键关联，要早于 `dropColumn()` 删除字段调用，否则数据库会报错。`dropForeign()` 方法的参数可以是字符串也可以是一个数组，如果是字符串则代表删除外键名为该字符串的外键，而如果是数组的话则会删除该数组中字段所对应的外键。 这个 `coupon_code_id` 字段默认的外键名是 `orders_coupon_code_id_foreign`，因此需要通过数组的方式来删除。

- 在 `Order` 模型中新增与 `CouponCode` 的关联关系：新建`couponCode()`: `return $this->belongsTo(CouponCode::class)`;执行数据库迁移命令
  	创建一个管理后台的控制器，使用 `admin:make` 来自动生成：`php artisan admin:make CouponCodesController --model=App\\Models\\CouponCode`;要实现的是优惠券列表，因此只需要关注 `grid()` 方法：新建方法

  ```php
  - index(Content):return$content
                ->header('优惠券列表')
                ->body($this->grid());
          在grid()方法: $grid = new Grid(new CouponCode);//这是写命令自动生成的
          1.默认按创建时间倒序排序$grid->model()->orderBy('created_at', 'desc');
          $grid->model()->orderBy('created_at', 'desc');
            $grid->id('ID')->sortable();
            $grid->name('名称');
            $grid->code('优惠码');
            $grid->description('描述');
            $grid->column('usage', '用量')->display(function ($value) {
                return "{$this->used} / {$this->total}";
            });
            $grid->enabled('是否启用')->display(function ($value) {
                return $value ? '是' : '否';
            });
            $grid->created_at('创建时间');
            $grid->actions(function ($actions) {
                $actions->disableView();
            });
  
  ​        return $grid;其中 $grid->column('usage', '用量') 是 虚拟出来的一个字段，然后通过 display() 来输出这个虚拟字段的内容。
  ```

-  添加相应的路由:`get('coupon_codes')`;在后台页面添加菜单

-  生成测试优惠券:使用工厂文件测试在编写工厂文件之前， 需要先编写优惠码的生成逻辑， 把这个逻辑放在 `CouponCode` 模型文件里：

  ```php
  - public static function findAvailableCode($length = 16)
        {
            do {
                // 生成一个指定长度的随机字符串，并转成大写
                $code = strtoupper(Str::random($length));
            // 如果生成的码已存在就继续循环
            } while (self::query()->where('code', $code)->exists());
  
  ​        return $code;
  ​    }
  ```

- 修改对应的工厂文件:`database/factories/CouponCodeFactory.php`:

  ```php
  -  // 首先随机取得一个类型
        $type  = $faker->randomElement(array_keys(CouponCode::$typeMap));
        // 根据取得的类型生成对应折扣
        $value = $type === CouponCode::TYPE_FIXED ? random_int(1, 200) : random_int(1, 50);
  
  ​    // 如果是固定金额，则最低订单金额必须要比优惠金额高 0.01 元
  ​    if ($type === CouponCode::TYPE_FIXED) {
  ​        $minAmount = $value + 0.01;
  ​    } else {
  ​        // 如果是百分比折扣，有 50% 概率不需要最低订单金额
  ​        if (random_int(0, 100) < 50) {
  ​            $minAmount = 0;
  ​        } else {
  ​            $minAmount = random_int(100, 1000);
  ​        }
  ​    }
  
  ​    return [
  ​        'name'       => join(' ', $faker->words), // 随机生成名称
  ​        'code'       => CouponCode::findAvailableCode(), // 调用优惠码生成方法
  ​        'type'       => $type,
  ​        'value'      => $value,
  ​        'total'      => 1000,
  ​        'used'       => 0,
  ​        'min_amount' => $minAmount,
  ​        'not_before' => null,
  ​        'not_after'  => null,
  ​        'enabled'    => true,
  ​    ];
  ```

  

-  优化:类型、 折扣 和 最低金额 三个字段可以用更友好的形式输出，比如 满 100 减 50，已用 和 总量 可以用 `10 / 1000` 这样的形式。类型、 折扣 和 最低金额 这三个字段的友好输出可以在多个地方使用，因此可以放到 `CouponCode` 模型里，作为一个访问器：序列化`protected $appends = ['description']`,因为数据库没有这个字段,调用model的属性使得该字段序列化这样前端就可以访问到了; 新建方法:

  ```php 
  - getDescriptionAttribute(){
          $str = '';
  
  ​        if ($this->min_amount > 0) {
  ​            $str = '满'.str_replace('.00', '', $this->min_amount);
  ​        }
  ​        if ($this->type === self::TYPE_PERCENT) {
  ​            return $str.'优惠'.str_replace('.00', '', $this->value).'%';
  ​        }
  
  ​        return $str.'减'.str_replace('.00', '', $this->value);
  ​    }
  ​    
  ```

#### 2.后台-添加,修改,删除优惠券

- 新增只需要修改`form()`方法: 

  ```php
  - $form = new Form(new CouponCode);
  
  ​        $form->display('id', 'ID');
  ​        $form->text('name', '名称')->rules('required');
  ​         $form->text('code', '优惠码')->rules(function($form) {
  ​            // 如果 $form->model()->id 不为空，代表是编辑操作
  ​            if ($id = $form->model()->id) {
  ​                return 'nullable|unique:coupon_codes,code,'.$id.',id';
  ​            } else {
  ​                return 'nullable|unique:coupon_codes';
  ​            }
  ​        });
  ​        $form->radio('type', '类型')->options(CouponCode::$typeMap)->rules('required')->default(CouponCode::TYPE_FIXED);
  ​        $form->text('value', '折扣')->rules(function ($form) {
  ​            if (request()->input('type') === CouponCode::TYPE_PERCENT) {
  ​                // 如果选择了百分比折扣类型，那么折扣范围只能是 1 ~ 99
  ​                return 'required|numeric|between:1,99';
  ​            } else {
  ​                // 否则只要大等于 0.01 即可
  ​                return 'required|numeric|min:0.01';
  ​            }
  ​        });
  ​        $form->text('total', '总量')->rules('required|numeric|min:0');
  ​        $form->text('min_amount', '最低金额')->rules('required|numeric|min:0');
  ​        $form->datetime('not_before', '开始时间');
  ​        $form->datetime('not_after', '结束时间');
  ​        $form->radio('enabled', '启用')->options(['1' => '是', '0' => '否']);
  
  ​        $form->saving(function (Form $form) {
  ​            if (!$form->code) {
  ​                $form->code = CouponCode::findAvailableCode();
  ​            }
  ​        });
  
  ​        return $form;
  ```

   - 对于优惠码 `code` 字段， 的第一个校验规则是 `nullable`，允许用户不填，不填的情况优惠码将由系统生成。

   - 对于折扣 `value` 字段， 的校验规则是一个匿名函数，当 的校验规则比较复杂，或者需要根据用户提交的其他字段来判断时就可以用匿名函数的方式来定义校验规则。

   - $form->saving() 方法用来注册一个事件处理器，在表单的数据被保存前会被触发，这里 判断如果用户没有输入优惠码，就通过 `findAvailableCode()` 来自动生成。

  	- > 注：为了绕过 `Laravel-Admin` 的一个 Bug， 需要给 radio 类型的字段加上一个默认值。

- 添加路由:

  ```php
  $router->post('coupon_codes', 'CouponCodesController@store');
  $router->get('coupon_codes/create', 'CouponCodesController@create');
  $router->get('coupon_codes/{id}/edit', 'CouponCodesController@edit');
  $router->put('coupon_codes/{id}', 'CouponCodesController@update');
  $router->delete('coupon_codes/{id}', 'CouponCodesController@destroy');
  ```

  ​	

#### 3.用户界面-检查优惠券

- 新增控制器来提供优惠券的查询功能: `art make:controller CouponCodesController`

- 新增`show($code)`方法:

   - 1,判断优惠券是否存在`!$record = CouponCode::where('code', $code)->first() abort(404)` 直接中断 程序的运行抛出404错误,接受的参数会变成 `Http` 状态码返回,这里的错误不能按希望的格式输出,因此只能用`404`这种不需要`message`的异常上,其他通用的异常,即不需要特殊处理的通通走 `InvalidException` 和 `InternalException`;

   - 2,如果优惠券没有启用，则等同于优惠券不存在`!$record->enabled` 抛出`404`;

     ```php
     -  -  if ($record->total - $record->used <= 0) {
                      return response()->json(['msg' => '该优惠券已被兑完'], 403);
                  }
     
     ​        if ($record->not_before && $record->not_before->gt(Carbon::now())) {
     ​            return response()->json(['msg' => '该优惠券现在还不能使用'], 403);
     ​        }
     
     ​        if ($record->not_after && $record->not_after->lt(Carbon::now())) {
     ​            return response()->json(['msg' => '该优惠券已过期'], 403);
     ​        }
     
     ​        return $record;
     ```

- 路由:`get('coupon_codes/{code}')`;
- 前端模板:实现的效果是：用户输入一个优惠码，点击 检查 按钮，如果优惠码不正确则弹框提示，如果优惠码可以使用则输出优惠信息、将优惠码的输入框禁用、隐藏 检查 按钮、展示 取消 按钮。而点击 取消 按钮则反之，隐藏优惠信息、将优惠码的输入框启用、显示 检查 按钮、隐藏 取消 按钮。JS代码逻辑 一.检查按钮点击事件:
  1. 选中id点击事件,
  2. 获取用户输入的优惠码使用`.val()`;
  3. 如果没有输入则使用`swal`弹框提示;
  4. 调用检查接口使用`axios.get('/coupon_codes/' + encodeURIComponent(code))`防御式编程，假如后端生成的 `code` 里面带有 `/`，比如 `code` 是 `test/test`，如果不使用`encodeURIComponent` 那么最终请求的 `url` 就变成了 `/coupon_codes/test/test` ，这是一个不存在的路由就会报 `404` 错误。
  5. 调用 `encodeURIComponent` 之后就会变成 `/coupon_codes/test%2Ftest`，就是对 / 做了转义，这样就可以被认为是一个整体。`arbon` 这个类的 `API`，`gt` 是 `great than`，即大于;
  6. `.then(fuction(response))`方法的第一个参数是回调,请求成功时会被调用,输出优惠信息`$('#coupon_desc').text(response.data.description)`,禁用输入框`$('input[name=coupon_code]').prop('readonly', true)`, `$('#btn-cancel-coupon').show();` // 显示 取消 按钮;`$('#btn-check-coupon').hide()`; // 隐藏 检查 按钮,第二个匿名函数是错误回调`fuction(error){如果返回码是404,说明优惠券不存在,error.response.status === 404,swal弹框('优惠券不存在','',''error)}`,如果返回的是`403`,说明有其他条件不满足,其他错误;
  7. 隐藏按钮点击事件:隐藏优惠信息`$('#coupon_desc').text('')`,启用输入框`$('input[name=coupon_code]').prop('readonly', false)`,隐藏 取消 按钮`$('#btn-cancel-coupon').hide()`,显示 检查 按钮`$('#btn-check-coupon').show()`

#### 4.用户界面-使用优惠券下单

1. ##### 优惠券下单是优惠券模块最核心的部分
