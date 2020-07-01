####   laravel 开发公众号 





- 使用overtrue/laravel-wechat包开发
  - git自述:https://github.com/overtrue/laravel-wechat
  - 遇到的问题:安装后未在serviceProvider和Facade注册
  - c创建配置文件:`php artisan vendor:publish --provider="Overtrue\LaravelWeChat\ServiceProvider"`后按文档操作出现 `Class 'Overtrue\LaravelWeChat\ServiceProvider' not found`报错,解决方法:

```php
<?php

namespace App\Http\Controllers;
use EasyWeChat\Factory;
use Illuminate\Http\Request;
class WechatController extends Controller
{
    public function server()
    {
        $app= config('wechat');
        $response=  Factory::officialAccount($app);
        return $response->server->serve();
    }
}

php artisan route:clear

php artisan config:clear

php artisan cache:clear
```



- 微信后台配置:
  - 服务器配置
  - 绑定白名单IP地址
  - 接口权限接入网页授权



公众号服务器提供的服务故障:  csrf要排除

```php
 在中间件 App\Http\Middleware\VerifyCsrfToken 
 protected $except = [
    // ...
    'wechat',
];
```



无法写入laravel日志问题:

```
首先在日志的配置文件中 修改为'default' => env('LOG_CHANNEL', 'daily'), 在
'daily' => [
            'driver' => 'daily',
            'path' => storage_path('logs/laravel.log'),
            'level' => 'debug',
            'days' => 14,
            'permission'=>0666, //添加这一行解除写入权限
        ],
然后在服务器php artisan config:clear清除配置缓存
```



多个公众号管理

```
use Overtrue\LaravelWeChat\Facade;
$app = Facade::officialAccount('wechat.officialAccount.default');//默认是配置文件中的default,要添加多个就需要cope一份改名
也可以写成Facade::officialAccount();
```





请求消息的属性

```php
ToUserName 接收方帐号（该公众号 ID）
FromUserName 发送方帐号（OpenID, 代表用户的唯一标识）
CreateTime 消息创建时间（时间戳）
MsgId 消息 ID（64位整型）
```



图文消息回复

```php
<?php

namespace App\Http\Controllers;
use EasyWeChat\Factory;
use EasyWeChat\Kernel\Messages\News;
use EasyWeChat\Kernel\Messages\NewsItem;
use Illuminate\Http\Request;
class WechatController extends Controller
{
    public function server()
    {
        $app= config('wechat');
        $response=  Factory::officialAccount($app);



        $response->server->push(function ($message) {
            $item = new NewsItem([
                'title'       => '这是图文的标题',
                'description' => '这是图文的简介',
                'url'         => 'http://www.signatorychain.com/',
                'image'       => 'http://www.signatorychain.com/images/2020_5_14_jx.png',
            ]);
           return new News([$item]);
        });

        return $response->server->serve();
    }
}

```

常用消息回复

```
$app->server->push(function ($message) {
    switch ($message['MsgType']) {
        case 'event':
            return '收到事件消息';
            break;
        case 'text':
            return '收到文字消息';
            break;
        case 'image':
            return '收到图片消息';
            break;
        case 'voice':
            return '收到语音消息';
            break;
        case 'video':
            return '收到视频消息';
            break;
        case 'location':
            return '收到坐标消息';
            break;
        case 'link':
            return '收到链接消息';
            break;
        case 'file':
            return '收到文件消息';
        // ... 其它消息
        default:
            return '收到其它消息';
            break;
    }

    // ...
});
```



完整项目源码

```
<?php

namespace App\Http\Controllers;

use App\Models\Journalism;
use EasyWeChat\Kernel\Messages\Article;
use EasyWeChat\Kernel\Messages\Text;
use Overtrue\LaravelWeChat\Facade;

class WechatController extends Controller
{

    protected $journalism;


    public function __construct(Journalism $journalism)
    {
        //获取数据库新闻资讯倒序取一条
        $this->journalism = $journalism
            ->query()
            ->where('on_push', true)
            ->orderBy('id', 'desc')
            ->first();
    }

    /*
     * 推送公众号管理
     */
    public function server()
    {

        //实例化公众号
        $app = Facade::officialAccount();

        // 关注 事件推送
        $app->server->push(function ($message) use ($app) {
            //用户昵称
            $userName = $app->user->get($message['FromUserName'])['nickname'];
            // 事件类型为关注回复的message
            if ($message['MsgType'] == 'event') {
                switch ($message['Event']) {
                    case 'subscribe':
                        return 'Hi，' . $userName . " 欢迎您关注普天同签\n\n" . "您可以通过公众号底部菜单体验\n\n或者您可以拨打我们的客服热线\n\n400-966-8351";
                        break;
                    case 'CLICK':
                        switch ($message['EventKey']) {
                            case 'product':
                                $text = new Text('“普天同签”是杭州玺湖科技有限公司为个人、企业和机构搭建的基于区块链和去中心化电子签名的可信、无接触、可保全、可追溯的移动数字化平台。“普天同签”实现办公、政务、交易和社交电子文件和记录的全生命周期管理、保全和保真 (在线生成、签发、审批、签约、审计、区块链存证举证、保真保全、在线搜寻、和查阅具有司法效果的文件和记录)。“普天同签”重点打造可信、无接触数字化办公生态、为政务、交易、和社交生态强信任背书，克服线下方式处理和签署文件易丢失、难携带、难寻找、易假冒伪造、和高成本的问题。“普天同签”解决传统数字化固有的中心化、易篡改、无法律效力等痛点。');
                                return $text;
                                break;
                            case 'relation':
                                $text = new Text("公司名称：杭州玺湖科技有限公司\n\n公司地址：浙江省杭州市西湖区文一西路460号文娱中心170室\n\n邮箱：service@signatorychain.com\n\n服务电话：4 0 0 - 9 6 6 - 8 3 5 1\n\n服务时间：周一到周五（09.00~21.00）");
                                return $text;
                                break;
                        }
                }
            }

            //返回菜单
            return $this->WeChatMenu();
        });

        //群发消息
        if ($this->journalism->wechat_push && !$this->journalism->data_push) {
            //正式发送 每月4次
            //$status = $app->broadcasting->sendNews($this->ArticleMediaId());
            //预览接口
          $status=  $app->broadcasting->previewNews($this->ArticleMediaId(), 'ona74vrgeAqeWYXXKoWH5CGtKljs');
           if ($status['errcode'] == 0){
               $this->journalism->data_push=1;
               $this->journalism->save();
           }
        }

        $response = $app->server->serve();

        return $response;
    }


    /*
       * 后台富文本转化微信文本并上传到微信图文永久素材获取mediaId
       *
      */
    protected function ArticleMediaId()
    {
        //实例化公众号
        $app = Facade::officialAccount();


        //上传图文消息内的图片获取URL
        $description = $this->journalism->description;

        $preg = "/<[img|IMG].*?src=[\'|\"](.*?(?:[\.jpg|\.jpeg|\.png|\.gif|\.bmp]))[\'|\"].*?[\/]?>/";
        preg_match_all($preg, $description, $descriptionAll);

        //替换前 array
        $front = $descriptionAll[1];
        if (count($front) > 1) {
            $arr = [];
            //微信上传素材获取的是服务器本地文件
            $pattern = '/http:\/\/www\.signatorychain\.com\/storage\/uploads\/images\/admin\//';
            foreach ($front as $item) {
                $arr[] = preg_replace($pattern, '', $item);
            }

            $resultImg = [];
            foreach ($arr as $item) {
                //上传到微信素材
                $resultImg [] = $app->material->uploadImage(storage_path('app/public/uploads/images/admin/') . $item);
            }

            //取出图片素材的URL
            $rearUrl = [];
            foreach ($resultImg as $item) {
                $rearUrl[] = $item['url'];
            }

            //        微信获取的图片url替换后台富文本内容图片的url
            $newDescription = preg_replace_callback($preg, function ($match) {
                return str_replace($match[1], '%s', $match[0]);
            }, $description);

            $replace = vsprintf($newDescription, $rearUrl);
        } elseif (empty($front)) {
            // empty code
            $replace = $description;
        } else {
            // count==1 code
            $str = $front[0];
            $pattern = '/http:\/\/www\.signatorychain\.com\/storage\/uploads\/images\/admin\//';
            $newStr = preg_replace($pattern, '', $str);
            $resultImg = $app->material->uploadImage(storage_path('app/public/uploads/images/admin/') . $newStr);
            $rearUrl = $resultImg['url'];
            $newDescription = preg_replace_callback($preg, function ($match) {
                return str_replace($match[1], '%s', $match[0]);
            }, $description);
            $replace = sprintf($newDescription, $rearUrl);
        }


//        获取上传的图片模板ID
        $result = $app->material->uploadImage(storage_path('app/public/') . $this->journalism->image);
        $imageMediaId = $result['media_id'];

        //新建文章素材
        $article = new Article([
            "thumb_media_id" => $imageMediaId,
            "author" => "普天同签",
            "title" => $this->journalism->title,
            "content_source_url" => asset('journalism/' . $this->journalism->id),
            "content" => $replace,
            "digest" => $this->journalism->subhead,
            "show_cover" => 1,
        ]);
        $result = $app->material->uploadArticle($article);

        $articleMediaId = $result['media_id'];

        return $articleMediaId;
    }


    //创建菜单
    protected function WeChatMenu()
    {
        //实例化公众号
        $app = Facade::officialAccount();
        $buttons = [
            [
                "name" => "了解产品",
                "sub_button" => [
                    [
                        "type" => "click",
                        "name" => "产品介绍",
                        "key" => "product"
                    ],
                ],
            ],
            [
                "name" => "使用产品",
                "sub_button" => [
                    [
                        "type" => "view",
                        "name" => "应用下载",
                        "url" => "http://www.signatorychain.com/product"
                    ],
                    [
                        "type" => "view",
                        "name" => "使用帮助",
                        "url" => "http://www.signatorychain.com/help"
                    ],
                    [
                        "type" => "view",
                        "name" => "用户手册",
                        "url" => "http://www.signatorychain.com/phoneManual"
                    ],
                ],
            ],
            [
                "name" => "关于我们",
                "sub_button" => [
                    [
                        "type" => "view",
                        "name" => "关于我们",
                        "url" => "http://www.signatorychain.com/about"
                    ],
                    [
                        "type" => "click",
                        "name" => "联系我们",
                        "key" => "relation"
                    ],
                ],
            ]
        ];

        return $app->menu->create($buttons);
    }

}

```











```
ide-helper:代码提示:
    "php artisan clear-compiled",
    "php artisan ide-helper:generate",
    "php artisan optimize"
```

