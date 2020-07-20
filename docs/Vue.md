[Vue2.5-2.6-3.0 开发去哪儿网App 从零基础入门到实战项目开发](https://coding.imooc.com/lesson/203.html#mid=12981)

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