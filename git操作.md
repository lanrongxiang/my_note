### git操作



#### 配置

##### 全局配置(--global)	

名字和邮箱配置

```
git config --global user.email "532006864@qq.com"
git config --global user.name "lanrongxiang"
查看全局配置 ~/.gitconfig
```



#### 命令

```
pwd 					显示当前文件路径
git init 				新增一个本地库 初始化
ll 						列出详细信息
ls 						列出文件和目录
ls -a 					列出隐藏文件
git config user.name "lrx" 修改当前用户
git config user.email "lrx@qq.com" 修改当前邮箱
rm -rf /* 				删除命令 跑路命令
mkdit 					创建目录
touch 					创建文件
git status 				查看当前状态
git add a.php 			添加到推车
git commit -m "测试代码"  提交
```



#### 细节

> 新拉项目不要在有.git文件的目录下执行



设置代理:

```
git config --global https.proxy http://127.0.0.1:1080

git config --global https.proxy https://127.0.0.1:1080
```

