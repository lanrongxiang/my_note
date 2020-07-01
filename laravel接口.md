												laravel接口

1.PostMan 是一款跨平台的 API 调试工具，可以在 [PostMan 官网](https://www.getpostman.com/) 下载

2.[easy-sms](https://github.com/overtrue/easy-sms) 是安正超写的一个短信发送组件，利用这个组件，我们可以快速的实现短信发送功能。



- 200 OK - 对成功的 GET、PUT、PATCH 或 DELETE 操作进行响应。也可以被用在不创建新资源的 POST 操作上
- 201 Created - 对创建新资源的 POST 操作进行响应。应该带着指向新资源地址的 Location 头
- 202 Accepted - 服务器接受了请求，但是还未处理，响应中应该包含相应的指示信息，告诉客户端该去哪里查询关于本次请求的信息
- 204 No Content - 对不会返回响应体的成功请求进行响应（比如 DELETE 请求）
- 304 Not Modified - HTTP 缓存 header 生效的时候用
- 400 Bad Request - 请求异常，比如请求中的 body 无法解析
- 401 Unauthorized - 没有进行认证或者认证非法
- 403 Forbidden - 服务器已经理解请求，但是拒绝执行它
- 404 Not Found - 请求一个不存在的资源
- 405 Method Not Allowed - 所请求的 HTTP 方法不允许当前认证用户访问
- 410 Gone - 表示当前请求的资源不再可用。当调用老版本 API 的时候很有用
- 415 Unsupported Media Type - 如果请求中的内容类型是错误的
- 422 Unprocessable Entity - 用来表示校验错误
- 429 Too Many Requests - 由于请求频次达到上限而被拒绝访问



| 动词   | 描述                               | 是否幂等 |
| ------ | ---------------------------------- | -------- |
| GET    | 获取资源，单个或多个               | 是       |
| POST   | 创建资源                           | 否       |
| PUT    | 更新资源，客户端提供完整的资源数据 | 是       |
| PATCH  | 更新资源，客户端提供部分的资源数据 | 否       |
| DELETE | 删除资源                           | 是       |

- put 替换某个资源，需提供完整的资源信息；
- patch 部分修改资源，提供部分资源信息。

hash_equals 是可防止时序攻击的字符串比较,无论字符串是否相等，函数的时间消耗是恒定的，这样可以有效的防止时序攻击;



middleware 调用频率限制 写法->middleware('throttle:1,1') 表示1分钟一次;



在 API 的开发中，选择使用 [gregwar/captcha](https://github.com/Gregwar/Captcha) 来完成图片验证码的功能。



OAuth [理解 OAuth 2.0](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html):是一种授权机制.

[socialiteproviders](https://socialiteproviders.github.io/) :方便完成OAuth流程

[jwt-auth](https://github.com/tymondesigns/jwt-auth) 是 Laravel 和 lumen 的 JWT 组件 : jwt 是 JSON Web Token 的缩写,JWT 由头部（header）、载荷（payload）与签名（signature）组成.



php artisan larabbs:generate-token



eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC9sYXJhYmJzLnRlc3QiLCJpYXQiOjE1ODQ0MTAyMjcsImV4cCI6MTYxNTk0NjIyNywibmJmIjoxNTg0NDEwMjI3LCJqdGkiOiJ5eGhxZ1gxb01kRnhhMkVRIiwic3ViIjoxLCJwcnYiOiIyM2JkNWM4OTQ5ZjYwMGFkYjM5ZTcwMWM0MDA4NzJkYjdhNTk3NmY3In0.I414yN1vdb9B-jHk4TLczYYM-F0W5169VQI55Dpsdww

[spatie/laravel-query-builder](https://github.com/spatie/laravel-query-builder)引入 Include 机制



tail -f ./storage/logs/laravel.log 打开日志

-A INPUT -p tcp -m state --state NEW -m tcp --dport 3609 -j ACCEPT

 svn checkout svn://121.37.250.56/svn/www --username=admin