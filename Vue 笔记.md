#### Vue 笔记

[toc]

##### 环境部署

下载 nodejs最新版 lts版本 https://nodejs.org/zh-cn/;

修改服务器为淘宝镜像 :

```
npm config set registry=https://registry.npm.taobao.org

//定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm
npm install -g cnpm --registry=https://registry.npm.taobao.org

//使用一步安装三大包管理器 npx cnpm i -g pnpm cnpm yarn

//下载vue cli 命令行工具
npm install -g @vue/cli

```

环境变量配置: 在C:\Users\dan\AppData\Roaming\npm\node_modules\包\bin;

根目录下:

```
//新建项目,预设Babel
vue create name ? Please pick a preset:default (babel, eslint)

//启动服务 yarn serve

//安装预处理器  pug 是一款模板引擎，sass-loader 和 node-sass 用来处理 Sass（一种 CSS 扩展语言），--save-dev 用来标识当前模块是开发所需的模块。
cnpm install pug-plain-loader pug sass-loader node-sass --save-dev
```

初始化 git:

```
git init 

git add -A

git commit -m "初始化 vue"

//新建github仓库 并选择远程仓库
git remote add origin git@github.com：lanrongxiang / vuejs-essential.git

//添加到github pages 作用是直接可以使用github的服务器在线上看到页面,但不支持php服务端的代码
.gitignore 文件中忽略的 /dist

//新建文件 vue.config.js

module.exports = {
  publicPath: process.env.NODE_ENV === 'production'
    ? '/vuejs-essential/dist/'
    : '/'
}

//生成生产环境可用的代码 代码会生成在 dist 目录使用该目录作为静态站点目录
npm run build

//提交代码 git add

//在 Github 仓库进行设置： 切换到Settings 找到GitHub Pages 设置来源为『master branch』

//使用https://lanrongxiang.github.io/vuejs-essential/dist/  访问线上页面
```

引入bootstrap,font-awesome:

```
cnpm install bootstrap-sass --save   //--save 用来标识当前模块是项目所需的模块。

npm install font-awesome --save
```



##### 基本操作

| 文件夹和文件名称                 | 简介                      |
| -------------------------------- | ------------------------- |
| babel.config.js                  | babel 配置文件            |
| node_modules                     | 存放 NPM 依赖模块         |
| package.json                     | 应用所需的 NPM 包配置文件 |
| public                           | 公共目录                  |
| public/favicon.ico               | 网站图标                  |
| public/index.html                | 入口页面                  |
| README.md                        | 项目介绍说明文件          |
| src                              | 源码目录                  |
| src/App.vue                      | 根组件                    |
| src/assets                       | 应用资源目录              |
| src/components                   | 公共组件目录              |
| src/main.js                      | 入口 js 文件              |
|                                  | 打包文件,给服务器用       |
| yarn.lock 或者 package-lock.json | 锁文件                    |

> 命名方式始终以单词大写开头，或者始终以横线连接,单文件组件使用单词大写或连接
>
> `import` 可以加载其他 JavaScript 文件中，使用 `export` 命令定义的对外接口。
>
> 在使用 `import` 引入模块时，使用 `@` 符号，该别名指向的是 `src` 目录，这是 Vue CLI 的默认配置。

style样式`scoped`：添加此属性，则样式只在当前组件起作用；

###### 引入相关样式:

```
//在src新建styles目录

//新建 main.scss 文件，复制贴入 [原站样式](https://raw.githubusercontent.com/summerblue/phphub5/master/resources/assets/sass/main.scss) 

//新建 extra.scss 文件，复制贴入 https://learnku.com/courses/vuejs-essential/2.6/website-logo/7936
```

###### 指令相关:

```
v-model 指令可以在表单 <input> 及 <textarea> 元素上创建双向数据绑定

v-show 是一个条件渲染指令，它只切换元素 CSS 属性的 display，当 show 值为 true 时，就显示该元素。

:key :v-for遍历时使用 vue底层用的diff算法也就是一一替换的算法,使用:key可以增加性能,vue最新版强制使用;如果值是唯一性的就直接使用value,反之index,常用value

v-on: 用于绑定事件,例如点击事件clink,简写@  修饰符 .stop阻止冒泡现象,调用envent.stopPropagaiton() , .prevent 阻止默认行为,用于表单操作 , .{keyCode|keyAlias}当时间是从特定键触发才触发回调 .native 监听, .once 只执行一次

v-model: 用于绑定表单,实现双向绑定  修饰符 .lazy 可以让数据在失去焦点或回车才更新,  .number 输入框默认都是字符串,如果希望是数字类型,则使用,  .trim 去除两边空格 


```

es6&js相关:

```
const : 定义常量,一般值不改变的情况下使用

let: var是js的一种缺陷,它是没有作用域的概念,以后开发尽量使用let

...:可变量 ,一般用于传值到方法,如果值有上百个需要传那么多用...


commenJs导入导出  
导出 .exports= {}
导入 .require('moduleA')

es6 导入导出
导出 exports{}   
导入 import 

```

webpack相关:

```
JavaScript 应用的 静态 模块化 打包工具 可以使.vue .scss 等文件转化成浏览器可以编译的代码

必须依赖node环境

packge.json 依赖包文件

webpage.config.js 配置文件
{
output:{
	path: path.resolve(__dirname,'dist')//打包好的文件存放位置
	publicPath:'dist/' //有关url路径的前缀添加dist/
}
 scripts: 脚本,用于执行某个命令
}
//loader加载器的使用
像less,scss,tpyejs等都是需用转编译给浏览器,所以要用到loader加载,在webpack官网找loader栏目
例子: url 首先需用安装
 module: {
    rules: [
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'url-loader', //加载器
            options: {
              limit: 8192,  //文件上传的大小,如果超过限定值将使用fallback-loader
              name:'img/[name].[hash:8].[ext]' //存放的位置和命名规则
            }
          }
        ]
      }
    ]
  }
  
 //babel也是一个loader,用于把es6转换为es5
```

npm相关:

```
npm install 包名 -g 全局安装

npm run serve 

npm run build 打包,构建项目执行webpage命令
```



###### 函数相关:

```
.trim 是一个修饰符，用于过滤用户输入的首尾空白字符。一般用于表单,可以和v-model指令一起执行;

.scrollIntoView() 方法让当前的元素滚动到浏览器窗口的可视区域内。如果为true，元素的顶端将和其所在滚动区的可视区域的顶端对齐。如果为false，元素的底端将和其所在滚动区的可视区域的底端对齐。

push : 添加一个元素到数组末尾

pop() 删除数组最后一个元素

shift() 删除数组第一个元素

unshift() 在数组最前面添加元素

splice() 删除元素 第一个参数为替换目标,第二个参数为0则不删除,第三个参数||...为数据

filter():  是js的函数式编程中的一种高阶函数,接受第一个参数为回调函数,必须返回布尔值,返回true,函数内部自动将这次回调加入新数组,反之则过滤;遍历的一种简单方式,该函数返回过滤后的新数组

map() : 也是一种高阶函数,第一个参数是回调函数,如果想数组中某一个值需要变换则使用这个

reduce() 作用是对数组中所有的内容进行汇总,回调接受两个参数,第一个为之前的值,第二个为新值

```

###### api相关:

```
$el:提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。可以是 CSS 选择器，也可以是一个 HTMLElement 实例。在实例挂载之后，元素可以用 vm.$el 访问。

$emit:监听当前实例上的自定义事件。事件可以由 vm.$emit 触发。回调函数会接收所有传入事件触发函数的额外参数。第一个参数是事件名称，后面还可以附加若干参数。

$nextTick:将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法 Vue.nextTick 一样，不同的是回调的 this 自动绑定到调用它的实例上。

$options:实例的 $options 是用于当前 Vue 实例的初始化选项：
```

###### 脚本相关:

```
export default {//export 用于从模块中导出函数、对象或原始值，以便其他程序使用 import 引入它们。
	name://组件的名称,
	
	components: {//引入组件后，还需在 components 中进行局部注册：
    	//组件name ,例如TheHeader
  },
  	
  	computed:{
  		//计算属性用来处理相对复杂的逻辑，并返回一个新的属性，它会根据其依赖的变化而变化。计算属性和methods的区别是计算属性有缓存,如果属性没有发生变化只调用一次,反之方法则会调用一次或多次
  	}
  	
  	props:{//props 是用来传递数据的,父组件传递给子组件
  		//必须要声明类型
  		'属性名':'TYPE',|| '属性名':{
  		type:'',
  		defaule:'',
  		}
  		
  		//props 的绑定默认是单向的，要在组件内部更新 show 值，需要在父组件上添加 show.sync 修饰符，以创建『双向绑定』：
  	},
  	
  	watch: {//侦听器 watch 选项提供了一个方法来响应数据的变化
  		例子:
  		 show(value,oldval) { //监听show的新值value,oldval是旧值
            if (value) {
              this.$nextTick(() => { //实例的 $nextTick 方法用于在下次 DOM 更新循环结束之后执行延迟回调，它有一个对应的全局方法 Vue.nextTick。
                this.$el.scrollIntoView(true)
              })
            }
          }
  	}
  
	data(){//数据对象，在组件里必须是返回一个初始数据对象的函数，可以在这里添加所需的数据,为什么是函数,因为对象使用的是同一个内存,会导致数据操作有误,同时绑定了,所以使用函数;
		必须 return{
		}
	},
	
	beforeRouteEnter(to,form,next){
		//beforeRouteEnter 是组件内的路由导航守卫，在确认渲染该组件的对应路由前调用。该守卫不能访问 this，但我们通过传一个回调给 next，就可以使用上面的 vm 来访问组件实例。守卫的参数说明如下：
		to：即将要进入的目标路由；
		from：当前导航正要离开的路由，from.name 是路由的名称，对应路由配置中的 name；
		next：一个用来 resolve 当前钩子的方法，需要调用该方法来确认或者中断导航；
		其常用的属性有：
            name：路由的名称，如 'Register'；
            path：路由的路径，如 '/auth/register'；
            params：路由参数对象，如 { id: "1" }；
            query：URL 查询参数对象，如 { page: "1" }；
            meta：元信息对象，如 { auth: true }；
	}
	
	beforeCreate://beforeCreate： 生命周期钩子的一部分，在实例初始化之后，数据观测之前被调用
	
	created(){
	//当前这个实例创建了之后要做的事
一般是向后台发送请求 axios.get('url').then(()=>{})  .then就是响应成功后的操作 如果created()函数需要调用el就需要切换为 mounted()生命周期函数; 
	}
	
	methods:{
	//methods 选项用来存放当前实例的方法，这些方法可以通过当前实例进行访问，如 this.changeNavIndex(1)，在指令表达式中，直接使用 changeNavIndex(1) 进行访问。
	},
	
}
```

###### 组件相关:

```
创建组件构造器: Vue.extend()

全局组件:注册全局组件需要使用 Vue.component，第一个参数 'Message' 是组件名称，第二个参数 Message 是一个对象或者函数;

局部组件: 在某个实例下 components属性:

传值操作:
	父传子 需要定义一个v-bind绑定方法,其值是父组件中的定义的属性一般是从服务器获取的,在子组件props属性内定义,定义的方式可以是数组和对象 '属性名':['值1','值2'],'属性名'{type:'TYPE',default:'默认值',require:Boolean如果设定此值为true则必须传值,}   命名方式如果是驼峰命名则使用-分割 
	
	子传父  子组件通过this->$emit('事件名','需要发送的值')触发发射事件,父组件通过使用自定义事件,v-on定义,简写@接收数据 @'事件名'(不需要添加参数,vue默认添加,和系统事件没传值会默认传event对象);
	
	父访问子属性 : 如果是访问全部组件则使用$children ,单个则使用$refs,同时标签内添加ref属性
	
	子访问父属性 通过$parent,一般开发不会使用
	
	访问根实例 $root

插槽:
	单个组件使用<slot></slot>
	插槽的默认值,用于组件有重复的模块
	如果是多个值,则全部替换
	具名性,可以定义一个name名用于多个元素替换

> 组件必须挂载在vue实例下
```

 

###### 自定义指令:

```
//钩子函数(均为可选)
bind：只调用一次，指令第一次绑定到元素时调用，在这里可以进行一次性的初始化设置；
inserted：被绑定元素插入父节点时调用；
update：所在组件的 VNode（虚拟节点）更新时调用，但是可能发生在其子 VNode 更新之前；
componentUpdated：指令所在组件的 VNode 及其子 VNode 全部更新后调用；
unbind：只调用一次，指令与元素解绑时调用，在这里可以移除绑定的事件和其他数据；

//指令钩子函数会被传入以下参数：
el：指令所绑定的元素，可以用来操作 DOM ；
binding：一个对象，binding.value 表示指令的绑定值，如 v-title="'我是标题'" 中，绑定值为'我是标题'；
vnode：Vue 编译生成的虚拟节点；
oldVnode：上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。


```



###### 使用 vue-router 来切换路由:

```
//安装vue-route	
npm install vue-router --save

//在 src 新建 router 文件夹新建 index.js 文件
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router) //使用 Vue.use 来使用插件：

const routes = [
    {
        path: '/auth/register', //路由的路径
        name: 'Register', //路由的名称
        component: () => import('@/views/auth/Register') //对应的视图组件;component: () => import实现路由懒加载，即当路由被访问时才加载对应的组件
    },
    
]

const router = new Router({
    mode: 'history', //路由模式，默认值 'hash' 使用井号（ # ）作路由，值 'history' 可利用 History API 来完成页面跳转且无须重新加载；
    routes
})

// 全局前置守卫
router.beforeEach((to, from, next) => {//使用 router.beforeEach 注册一个全局前置守卫，它在导航被触发后调用，可以通过跳转或取消的方式守卫导航

	//使用 router.app 可以获取 router 对应的 Vue 根实例，使用实例的 $options.store 可以从选项中访问仓库；
  const auth = router.app.$options.store.state.auth

  if (auth && to.path.indexOf('/auth/') !== -1) {
    next('/')
  } else {
    next()
  }
})


export default router

//引入路由配置
src/main.js 在当前实例注入路由

//添加 <router-view>
src/App.vue 引入 <router-view/>

//添加 <router-link>
<router-link> 组件支持用户在具有路由功能的应用中导航，通过 <router-link> 上的 to 属性可以指定目标地址，这里是一个字符串 /auth/register，对应路由配置中的 path。

```



###### 验证处理

```
//注册全局验证指令
在 src/directives 下新建 index.js 文件 
import Vue from 'vue'
import validator from './validator'
Vue.directive('validator', validator)

注册全局指令需要使用 Vue.directive，第一个参数 'validator' 是指令名称，第二个参数 validator 是指令对象或者指令函数，我们这里是指令对象。全局注册的好处是，可以在实例内部的所有组件中使用，而不用在每个组件内部单独引用和注册。

//引入全局验证指令
在 src/main.js 文件，引入 ./directives

//先在外层元素上，添加 data-validator-form 属性，使其成为所有验证项的父级：
```



