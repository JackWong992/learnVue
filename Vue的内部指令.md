# Vue的内部指令
* helloworld实例
* v-if/v-else/v-show
*
*
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