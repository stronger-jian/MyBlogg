[vueVue2.5-2.6-3.0 开发去哪儿网App 从零基础入门到实战项目开发](https://coding.imooc.com/lesson/203.html#mid=12981)

### Vue基础

#### hello world

html页面输出’hello world‘

```<div id="app"></div>```

```javascript
<script >
    var dom = document.getElementById('app');
    dom.innerHTML='hello world';
</script>
```
Vue实现hello world

引入Vue.js

`<script src="./vue.js"></script>`

`<div id="app">{{content}}</div>`

    <script >
        var app = new Vue({
            el: '#app',
            data: {
                content: 'hello world'
            }
        })
        //var dom = document.getElementById('app');
        //dom.innerHTML='hello world';
    </script>
输出’hello world‘ 2秒后 变成’bye world‘

`<div id="app">{{content}}</div>`

`<script >
    var app = new Vue({
        el: '#app',
        data: {
            content: 'hello world'
        }
    })
    setTimeout(function(){
        app.$data.content = 'bye world'
    }, 2000)
</script>`

这一节主要介绍，Vue和html的区别是html是操作dom，Vue是直接操作数据。

我最喜换这个课是，这个老师会在结尾说句，加油。

#### 使用vue.js实现TodoList

	<div id="app">
	    <input type="text" v-model="inputValue">
	    <button v-on:click="handleBtnClick">提交</button>
    	 <ul>
            <li v-for="item in list">{{item}}</li>
        </ul>
    </div>	
```<script >
    var app = new Vue({
        el: '#app',
        data: {
            list: [],
            inputValue:''
        },
        methods:{
            handleBtnClick: function(){
                this.list.push(this.inputValue)
                this.inputValue = ''
            }
        }
    })
</script>```
```

#### MVVM模式

View ViewModel Model 模式，省去了对dom的操作，大量减少前端代码，面向数据编程。

#### 前端组件化

每一个组件就是页面上的一个区域。

#### 使用组件改造TodoList

1. 使用全局组件

   ```vue
   <div id="app">
       <input type="text" v-model="inputValue">
       <button v-on:click="handleBtnClick">提交</button>
       <ul>
           <todo-item  v-bind:content="item" 
                       v-for="item in list">
           </todo-item>
       </ul>
   </div>
   ```
   ```vue
   <script >
       // 定义全局组件
       Vue.component("TodoItem", {
           // 从父组件接收的值
           props: ['content'],
           template: "<li>{{content}}</li>"
       })
       var app = new Vue({
           el: '#app',
           data: {
               list: [],
               inputValue:''
           },
           methods:{
               handleBtnClick: function(){
                   this.list.push(this.inputValue)
                   this.inputValue = ''
               }
           }
       })
   </script>
   ```

2. 局部组件定义

   ```vue
   <div id="app">
       <input type="text" v-model="inputValue">
       <button v-on:click="handleBtnClick">提交</button>
       <ul>
           <todo-item  v-bind:content="item" 
                       v-for="item in list">
           </todo-item>
       </ul>
   </div>
   ```
   ```vue
   <script >
       // 定义全局组件
       // Vue.component("TodoItem", {
       //     // 从父组件接收的值
       //     props: ['content'],
       //     template: "<li>{{content}}</li>"
       // })
       // 局部组件
       var TodoItem = {
           props: ['content'],
           template: "<li>{{content}}</li>"
       }
       var app = new Vue({
           el: '#app',
           components: {
               TodoItem: TodoItem
           },
           data: {
               list: [],
               inputValue:''
           },
           methods:{
               handleBtnClick: function(){
                   this.list.push(this.inputValue)
                   this.inputValue = ''
               }
           }
       })
   </script>
   ```

#### 简单的组件传值

```vue
<div id="app">
    <input type="text" v-model="inputValue">
    <button v-on:click="handleBtnClick">提交</button>
    <ul>
        <todo-item  :content="item" 
                    :index="index"
                    v-for="(item, index) in list"
                    @delete="handleItemDelete">
        </todo-item>
    </ul>
</div>
```
```vue
<script >
    // 局部组件
    var TodoItem = {
        //接受父组件传值
        props: ['content', 'index'],
        template: "<li @click='handleItemClick'>{{content}}</li>",
        methods: {
            handleItemClick: function() {
                //emit(发出，射出)向外触发事件
                this.$emit("delete", this.index)
            }
        }
    }
    var app = new Vue({
        el: '#app',
        components: {
            TodoItem: TodoItem
        },
        data: {
            list: [],
            inputValue:''
        },
        methods:{
            handleBtnClick: function() {
                this.list.push(this.inputValue)
                this.inputValue = ''
            },
            handleItemDelete: function(index) {
                this.list.splice(index,1)
            }
        }
    })
</script>
```
### 基础精讲

#### 生命周期钩子

这个建议慢慢理解

#### Vue的模板语法

```vue
<div id="app">
    <div>{{name + ' Li'}}</div>
    <div v-text="name + ' Li'"></div>
    <div v-html="name + ' Li'"></div>
</div>
```
```vue
<script >
    var vm = new Vue({
        el: "#app",
        data: {
            name: "Dell"
        }
    })
</script>
```
{{变量名}}这是差值表达式等同于v-text

#### 计算属性，方法，侦听器

```vue
<div id="app">
   {{fullName}}
   {{age}}
</div>
```
使用计算属性：

```vue
<script >
    var vm = new Vue({
        el: "#app",
        data: {
            firstName: "hello",
            lastName: "world",
            age: 28
        },
        // 计算属性
        computed: {
            fullName: function(){
                console.log("计算了一次");
                return this.firstName + " " + this.lastName
            }
        }
    })
</script>
```
使用方法：

{{fullName()}}

```vue
<script >
    var vm = new Vue({
        el: "#app",
        data: {
            firstName: "hello",
            lastName: "world",
            age: 28
        },
        methods: {
            fullName: function() {
                console.log("计算了一次");
                return this.firstName + " " + this.lastName
            }
        }
    })
</script>
```
使用侦听器：

```vue
<script >
    var vm = new Vue({
        el: "#app",
        data: {
            firstName: "hello",
            lastName: "world",
            fullName: "hello world",
            age: 28
        },
        watch: {
            firstName: function() {
                console.log("计算了一次")
                this.fullName = this.firstName + " " + this.lastName
            },
            lastName: function() {
                console.log("计算了一次")
                this.fullName = this.firstName + " " + this.lastName
            }
        }
    })
</script>
```
当同一功能，能用计算属性，方法，监听器使用时优先使用计算属性。

#### 计算属的get和set

```vue
<div id="app">
   {{fullName}}
</div>
```
```vue
<script >
    var vm = new Vue({
        el: "#app",
        data: {
            firstName: "hello",
            lastName: "world",
        },
        computed: {
            fullName: {
                get: function() {
                    return this.firstName + " " + this.lastName
                },
                set: function(value) {
                    var arr = value.split(" ")
                    this.firstName = arr[0]
                    this.lastName = arr[1]
                }
            }
        }
    })
</script>
```
#### Vue的样式绑定

点击字变色:

方法1：

```css
<style>
    .activated{
        color: red
    }
</style>
```
```vue
<div id="app">
    <div @click="handleDivClick"
         :class="{activated: isActivated}"
    >
        hello world
    </div>
</div>
```
```vue
<script >
    var vm = new Vue({
        el: "#app",
        data: {
            isActivated: false
        },
        methods: {
            handleDivClick: function() {
                this.isActivated = !this.isActivated
            }
        }
    })
</script>
```
方法2：

```vue
<div id="app">
    <div @click="handleDivClick"
         :class="[activated]"
    >
        hello world
    </div>
</div>
```
```vue
<script >
    var vm = new Vue({
        el: "#app",
        data: {
            activated: ""
        },
        methods: {
            handleDivClick: function() {
                this.activated = this.activated === "activated"? "": "activated"
            }
        }
    })
</script>
```
方法3：

```vue
<div id="app">
    <div @click="handleDivClick"
         :style="styleObj"
    >
        hello world
    </div>
</div>
```
```vue
<script >
    var vm = new Vue({
        el: "#app",
        data: {
            styleObj: {
                color: "black"
            }
        },
        methods: {
            handleDivClick: function() {
                this.styleObj.color = this.styleObj.color === "black"? "red": "black"
            }
        }
    })
</script>
```
#### Vue中的条件渲染

```vue
<div id="app">
    <div v-if="show" >
        用户名：<input key="username">
    </div>
    <div v-else >
        邮箱：<input key="email">
    </div>
</div>
```
key值会防止Vue对相同dom的复用。

```vue
<script >
    var vm = new Vue({
        el: "#app",
        data: {
            show: false,
            message: "hello wolrd"
        }
    })
</script>
```
#### Vuex:状态管理模式

应用场景：

1. 多个视图依赖于同一状态
2. 来自不同视图的行为需要改变同一状态

组成介绍：

* State：数据仓库

* getter：用来获取数据
* Mutation：用来修改数据的
* Action：用来提交mutation

[vue调试工具vue-devtools的安装及使用](https://www.cnblogs.com/yuqing6/p/7440549.html)我是没能成功我服，下载下来目录和教程不一样，所以我想别的方法了。

安装Vuex

* 安装包：npm install vuex
* 创建vuex实例：new Vuex.Store()
* main.js中将 vuex实例挂载到vue对象上

实现count++

基本demo：

* state中创建count字段

  main.js中

  `import Vuex from 'vuex'`

  `Vue.use(Vuex)
  const store = new Vuex.Store({
    state: {
      count: 0
    }
  })`

* mutation中创建一个count++的mutation

  `const store = new Vuex.Store({
    state: {
      count: 0
    },
    mutations: {
      countIncrease (state) {
        state.count++
      }
    }
  })`

* button新增click事件触发mutation改变count

  <template>
    <div id="app">
      <router-view/>
      <h1>count:{{count}}</h1>
      <button @click='countIncrease'>点我自增</button>
    </div>
  </template>

  <script>
  export default {
    name: 'App',
    computed: {
      count () {
        return this.$store.state.count
      }
    },
    methods: {
      countIncrease () {
        this.$store.commit('countIncrease')
      }
    }
  }
  </script>

### vuex小案例

* 通过state.userInfo控制用户登录路由限制
* 多组间共享state.userStatus和state.vipLevel状态
* 多组件修改state.userStatus和state.vipLevel