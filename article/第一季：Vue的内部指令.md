# Vue的内部指令
* helloworld实例
* v-if/v-else/v-show实例
* v-for实例
* v-text和v-html实例
---

## helloworld实例
对于大牛来说学习Vue来说直接看文档就可以了，但是对于我这种小白来说看文档更加有益了我的睡眠，所以我选择了一种很慢的方式且很不被看好的方式来学习Vue-看视频。所以在学习的中后期选择看文档和刷项目也是学习好Vue的必走路径<br>
首先动手写一个Vue的实例（helloworld),在写之前你需要引入Vue的官方文档，引入的方式有很多：本地引入，CDN引入。本地引入操作起来比较麻烦，所以我选择了比较方便的CDN引入
CDN地址：
```javascript
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>  
为了初学者更好的理解，这是一个开发环境的CDN，所以在以后的实例中我都默认使用了这个CDN（代码中不会再展示·）  
```
我在本地创建了`assets`，`example`文件夹和`index.html`文件，`index.html`它就像一个目录可以方便你观察，而源代码都存放在`example`这个文件夹中
1. helloworld.html
```javascript
    <div id="app">
        {{message}}
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                message: 'helloworld'
            }
        })
    </script>
```
就这样点击`index.html`文件夹中`01_helloworld`文件你就可以查看这个`helloworld`

---
## v-if/v-else/v-show
原生的`JS`中的`if-else`和`Vue`中的`v-if`、`v-else`有点类似

02_v-if&v-else/v-show.html
```javascript

    <div v-if="isLogin">你好，技术胖</div>
    <div v-else>你好，黄飞龙</div>
    <div v-show="login">如果show为true你就可以看到这句话</div>

    var app = new Vue({
        el: '#app',
        data: {
            islogin: false,
            login: true
        }
    })
```
这里当`isLogin`的值为`false`时，会直接执行下一步操作，也就是`v-else`。如果`islogin`为`true`的话就不会接着往下执行了<br>
`v-show`的理解也可以是这样的。
但是`v-if`和`v-show`又有所不同：
`v-if`用来判断是否加载`html`的`DOM`,可以减轻服务器的压力，在需要时加载。<br>
`v-show`:相当于我们`CSS`中的`display`属性，可以使客户端操作起来更加流畅。

---
## v-for
都知道`v-for`是要循环的内容，所以下面将自己介绍一下`v-for`实例<br>
 `03_v-for.html`实例
 ```javascript
    <div id="app">
        <ul>
            <li v-for="item in items">
        </ul>
    </div>

    var app = new Vue({
        el: '#app',
        data: {
            items: [20,30,90,67,89]
        }
    })
    // 20 
    // 30
    // 90
    // 67
    // 89
 ```
 这里要注意的是，有`JS`有所不同的是`v-for`标签当你要作用与谁就直接在该元素的本身，而不是它的父级。这里的`v-for`是想让`li`元素循环实现`items`的展示，所以要作用与`li`元素的本身。<br>
 如果想要实现顺序排序该怎么做：
 ```
    var app = new Vue({
            el:'#app',
            data: {
                items: [20,10,90,78,50]
            },
            computed:{
                sortItems: function(){
                    return this.items.sort()
                }
            }
        })

        // 10
        // 20
        // 50
        // 78
        // 90
 ```
 我们在computed里新声明了一个对象sortItems，如果不重新声明会污染原来的数据源，这是Vue不允许的，所以你要重新声明一个对象。


 这样表面上看起来是可行的，但是会有一个潜在的bug，而这个bug是从js出生以来就有了而且没有消失。例如：10，30，4，50的顺序排序应该是4，10，30，50.而在JS中展示的则是：10，30，4，50，所以想要改变这个bug我们需要手动的写一个函数：
 ```
    function sortNumber(a,b){
        return a-b;
    }
 ```
 使用方法：
 ```
    computed: {
        sortItems:funtion(){
            return this.items.sort(sortNumber)
        }
    }
 ```
 ### 对象中循环输出
 如果是对象元素我们应该怎么实现循环输出，结果其实类似：
 ```javascript
    <ul>
        <li v-for="(student,index) in students">
        {{index+1}}{{ student.name}} :{{student.age}}
        </li>
    </ul>

    data: {
        students:[
            {name:'baby',age:'1'},
            {name:'children',age:'12'},
            {name:'youth',age:'21'},
            {name:'old',age:'50'},       
        ]
    }

    // 1 baby: 1
    // 2 children: 12
    // 3 youth: 21
    // 4 old: 50
 ```
 那么如何实现对象元素的排序呢？如前面类似你要实现写一个函数：
```
//数组对象方法排序:
function sortByKey(array,key){
    return array.sort(function(a,b){
      var x=a[key];
      var y=b[key];
      return ((x<y)?-1:((x>y)?1:0));
   });
}
```
有了对象排序你可以在`computed`写入以下：
```
    computed: {
        sortStudent: function(){
            return sortByKey(this.student,'age')
        }
    }
```
这样就可以完整的实现对象元素的顺序排序了。

---
## v-text、v-html实例
在第一小节中我们展示了`helloworld`的书写，但是在项目中实现这种`message`的展现比较少，因为当网速较慢的情况下或者当代码发生错误的时候，页面会出现`{{message}}`给用户的感觉非常突兀。在下面我们展示一下两种比较友好的方式：<br>

04.`v-text`、`v-html`:
```javascript
     <div id="app">
       <span>这是通过message展示的：{{message}}</span> <br>
      这是通过v-text展示的：<span v-text="sayWhere"></span><br>
      这是通过v-html展示的：<span v-html="doSomeThing"></span>
   </div>
   <script>
       var app = new Vue({
           el:'#app',
           data:{
               message: 'hello world',
               sayWhere:'你好，世界',
               doSomeThing:'你好这个世界'
           }
       })
   </script>
```
第一种就是最简单的`message`展示<br>
第二种使用最广泛的通过`v-text`展示<br>
第三张通过对代码进行编译进行展示<br>
上述都是对文本内容的展示，在实际开发中尽量多使用`v-text`，如果后面的代码出现错误，或者网速较慢`sayWhere`内容将不会被展示。
而对`v-html`来说，要慎用，特别是在提交表单的时候：因为会引起不必要的`xss`攻击

---

## v-on实例
v-on实例有点类似于我们学习`jQuery`中的绑定时间，这里的`v-on`也是监听一种事件，v可以用v-on指令监听DOM事件来触发一些javascript代码

05_v-on实例演示：
```javascript

    <div id="app">
        本场比赛得分：{{score}}
        <p>
            <button v-on:click="add">加分</button>
            <button v-on:click="del">减分</button>
        </p>
    </div>

    <script>
        var app = new Vue({
            el: '#app',
            data: {
                score: 0;
            },
            methods:{
                add:function(){
                    this.score++;
                },
                del:function(){
                    this.score--;
                }
            }
        })
    </script>
```
点击加分按钮就会监听一个加分的事件，点击减分就会监听一个减分的事件。

那如何实现自定义事件呢：
```javascript
自定义加分，点击键盘按键得到结果：<br>
<input type="text" v-on:keyup.13="custom_add" v-model="score2">
左边输入框输入你想自定义加的分数，点击enter就可以实现

var app = new Vue({
    el: '#app',
    data: {
        score: 0;
        score2: 1;
    },
    methods:{
        add:function(){
            this.score++;
        },
        del:function(){
            this.score--;
        },
        custom_add:function(){
            this.score = this.score+parseInt(this.score2);
        }
    }
})
```
![20170227001137.jpg](https://i.loli.net/2018/04/18/5ad6abafc13eb.jpg)

注意在键盘中`enter`对应的`keycode`是13。<br>
所以绑定的是13，也可以写`enter`,还有一点要注意的是在js代码中加法要注意转换，负责就是字符串相加了。