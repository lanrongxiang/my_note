# vue&laravel



artian命令缩写文件在 homestead目录下 aliases文件里

虚拟机重新编译后基础配置可以在 after.sh 文件修改初始配置 比如composer阿里镜像源   插件(https://github.com/spatie/homestead-custom/blob/master/after.sh)



### Vue

​	在laravel 的JS模块引入 vue: 

 1. ```vue
    <script
      src="https://cdn.bootcdn.net/ajax/libs/vue/2.6.11/vue.esm.browser.min.js"></script>
    ```

    实例化 vue

    ```
    new Vue({
    	el: //元素的意思选择器 类似jq$('')
    	data:{
    	//obj 定义数据
    	}
    })
    ```

2. 渲染页面 :vue和blade的渲染方式都是{{}},会有冲突,laravel提供 @{{}}表示vue

3. 表单绑定  在input标签内添加`v-model` 绑定数据用,可以绑定data数据内的变量就变为了双向绑定

4. 谷歌浏览器安装vue插件

5. 列表渲染 : `v-for` 在需要遍历的元素标签类定义  例子: <li v-for="test in tests">

6. 事件处理:  绑定事件在标签内 绑定 @

   ```
   例子 : 键盘enter抬起
    <input @keyup.enter="方法名">
    在vue实例化内 methods:{}定义方法内要做什么
    在数组添加数据 使用 push()方法
    
   ```

7. 表单提交事件: 在form标签内 阻止表单默认刷新操作@subimt.prevent="方法名"

8. 计算属性: data同级 `computed:{}`  简单理解:把某一个属性计算过滤后的属性  js-`filter()`方法 如果为`ture`留下,为`fales`排除掉 ; 定义一个方法名过滤后的属性 通常可以运用到遍历数据 在方法名前需要`return`不要遗漏

9. 删除元素 : 删除数组元素 使用js-`splice('索引','要操作几个元素','替换的元素,不需要变化则不需要')`方法, `indexOf(目标元素)`返回索引

10. 选择标签: 在标签内定义一个ref属性  ref="", 然后在vue实例就可以用 $refs获得 ref集合 ;

11.  laravel命令行生成 model,resource资源,controller migrate迁移文件 `php artisan make:model  modelName -a`, 使用laravel mix 加载vue.js,模块化操作 

    - 执行命令`yarn add vue`,package.json 文件会添加 vue

    - 在resources/app.js文件调用,` window.Vue=require('vue')`

    - 重构vue组件, 在resource/js/ 新建components目录存放vue组件,后缀为.vue .vue文件分为三个部分 <template>  <script> <style>  blade文件引入vue:引入.vue文件名为标签就引入了, 在<script> 需要先引入 export default{}为容器, data()为一个function,内部需要return{},根实例 ,

      ```
      new Vue({
      	el:'',绑定的元素
      	components:{
      	'.vue文件名': require('./components/vue文件').dafault
      	}
      })
      ```

    - 引入axios : laravel 默认引入 可以直接使用,用于向后台传递数据

    - vue.js生命周期函数 created(),当前这个实例创建了之后要做的事

      一般是向后台发送请求 axios.get('url').then(()=>{})  .then就是响应成功后的操作 如果created()函数需要调用el就需要切换为 mounted()生命周期函数; 

      - 参数传递:在blade模板使用 route方法在vue标签内定义属性名传递
      - data同级创建 props:{'属性名':'TYPE'}

    

12. 创建事件总线: 创建一个新的vue文件来掌管所有的事件 event-bus.vue

    ```
     import Vue form 'vue';
     export const Hub=new Vue()
    ```

    接着要import到组件并调用在父子组件中传递信息时相同的方法,发送事件

    ```
    import { Hub } from './event-bus.js';  这里的导入{}表示常量
    在methods:{
    	Hub.$emit('事件名','要传递的参数')//$emit 表示发送事件,在生命周期函数内使用$on接收  
    }
    子组件调用:
    在生命周期函数内 $on('事件名',$this->'子组件方法名')
    
    ```

    

