[toc]

## 开源项目 (网站、小程序、h5端)  名字为: 暖暖 (warm)

#### 开发步骤

1. 实现功能: 照片墙、生日墙、备忘录、笔记(后续添加)
2. 用例分析
   - 角色
     	- 游客 -没有登录的用户 
     	- 用户 -注册用户,可以增删改查数据
     	- 管理员- 权限最高的用户角色,可以添加删除用户等
   - 信息
     	- 用户--模型名称User
     	- 照片墙 -模型名称 Photo 可以用于发布照片,事件地点类似qq空间
     	- 生日墙 - 模型名称 Birthday  用于记录朋友之间的生日
     	- 备忘录 - 模型名称 Meom      用于记录生活中的一些事
     	- 笔记 - 模型名称 Note             分享一些笔记
     	- 记账本 -模型名称 Tallay         用来记录当天账单记录类似任务管理系统
   - 动作
      - 游客
        - 游客可以查看用户公开的照片 笔记
     - 用户
       - 增删查改数据
     - 管理员
       - 增删查改用户

#### 开发思路

 基于需求分析,拆分以下模块

- 用户模块
- 生日墙模块
- 备忘录模块
- 照片墙模块
- 记账本模块
- 笔记模块



#### 实现

1. 根目录下载laravel框架:composer create-project laravel/laravel warm --prefer-dist "7.*";修改.env文件
2. 辅助函数  helper.php 存放进bootstrap/helpers.php`composer.json` 文件，并找到 `autoload` 段注册,根目录执行composer dumpautoload
3. 基础布局 resources/views/layouts/app.blade.php,resources/views/layouts/_header.blade.php;resources/views/layouts/_footer.blade.php
4. 使用 php artisan make:controller PagesController处理所有自定义页面的逻辑并使用root()展示首页

