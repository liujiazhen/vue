## `watch`属性的使用
考虑一个问题：想要实现 `名` 和 `姓` 两个文本框的内容改变，则全名的文本框中的值也跟着改变；

1. 监听`data`中属性的改变：
```javascript
<div id="app">
  <input type="text" v-model="firstName"> +
  <input type="text" v-model="lastName"> =
  <span>{{fullName}}</span>
</div>

<script>
  // 创建 Vue 实例，得到 ViewModel
  var vm = new Vue({
    el: '#app',
    data: {
      firstName: 'jack',
      lastName: 'chen',
      fullName: 'jack - chen'
    },
    methods: {},
    watch: {
      'firstName': function (newVal, oldVal) { // 第一个参数是新数据，第二个参数是旧数据
        this.fullName = newVal + ' - ' + this.lastName;
      },
      'lastName': function (newVal, oldVal) {
        this.fullName = this.firstName + ' - ' + newVal;
      }
    }
  });
</script>
```
2. 监听路由对象的改变：
```javascript
<div id="app">
  <router-link to="/login">登录</router-link>
  <router-link to="/register">注册</router-link>

  <router-view></router-view>
</div>

<script>
  var login = Vue.extend({
    template: '<h1>登录组件</h1>'
  });

  var register = Vue.extend({
    template: '<h1>注册组件</h1>'
  });

  var router = new VueRouter({
    routes: [
      { path: "/login", component: login },
      { path: "/register", component: register }
    ]
  });

  // 创建 Vue 实例，得到 ViewModel
  var vm = new Vue({
    el: '#app',
    data: {},
    methods: {},
    router: router,
    watch: {
      '$route': function (newVal, oldVal) {
        if (newVal.path === '/login') {
          console.log('这是登录组件');
        }
      }
    }
  });
</script>
```

## `computed`计算属性的使用
1. 默认只有`getter`的计算属性：
```javascript
<div id="app">
  <input type="text" v-model="firstName"> +
  <input type="text" v-model="lastName"> =
  <span>{{fullName}}</span>
</div>

<script>
  // 创建 Vue 实例，得到 ViewModel
  var vm = new Vue({
    el: '#app',
    data: {
      firstName: 'jack',
      lastName: 'chen'
    },
    methods: {},
    computed: { // 计算属性； 特点：当计算属性中所以来的任何一个 data 属性改变之后，都会重新触发 本计算属性 的重新计算，从而更新 fullName 的值
      fullName() {
        return this.firstName + ' - ' + this.lastName;
      }
    }
  });
</script>
```
2. 定义有`getter`和`setter`的计算属性：
```javascript
<div id="app">
  <input type="text" v-model="firstName">
  <input type="text" v-model="lastName">
  <!-- 点击按钮重新为 计算属性 fullName 赋值 -->
  <input type="button" value="修改fullName" @click="changeName">

  <span>{{fullName}}</span>
</div>

<script>
  // 创建 Vue 实例，得到 ViewModel
  var vm = new Vue({
    el: '#app',
    data: {
      firstName: 'jack',
      lastName: 'chen'
    },
    methods: {
      changeName() {
        this.fullName = 'TOM - chen2';
      }
    },
    computed: {
      fullName: {
        get: function () {
          return this.firstName + ' - ' + this.lastName;
        },
        set: function (newVal) {
          var parts = newVal.split(' - ');
          this.firstName = parts[0];
          this.lastName = parts[1];
        }
      }
    }
  });
</script>
```

## `watch`、`computed`和`methods`之间的对比
1. `computed`属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算，主要当作属性来使用，在方法体内部，必须要 return 返回值；
2. `methods`方法表示一个具体的操作，主要书写业务逻辑；
3. `watch`一个对象，键是需要观察的表达式，值是对应回调函数。主要用来监听某些特定数据的变化，从而进行某些具体的业务逻辑操作；可以看作是`computed`和`methods`的结合体；
