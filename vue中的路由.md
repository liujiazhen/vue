## 什么是路由
1. **后端路由：** 对于普通的网站，所有的超链接都是URL地址，所有的URL地址都对应服务器上对应的资源；

2. **前端路由：** 对于单页面应用程序来说，主要通过URL中的hash(#号)来实现不同页面之间的切换，同时，hash有一个特点：HTTP请求中不会包含hash相关的内容；所以，单页面程序中的页面跳转主要用hash实现；

3. 在单页面应用程序中，这种通过hash改变来切换页面的方式，称作前端路由（区别于后端路由）；

## 在 vue 中使用 vue-router
1. 导入 vue-router 组件类库：
```javascript
<!-- 1. 导入 vue-router 组件类库 -->
<script src="./lib/vue-router-2.7.0.js"></script>
```
2. 使用 router-link 组件来导航
```javascript
<!-- 2. 使用 router-link 组件来导航 -->
<router-link to="/login">登录</router-link>
<router-link to="/register">注册</router-link>
```
3. 使用 router-view 组件来显示匹配到的组件
```javascript
<!-- 3. 使用 router-view 组件来显示匹配到的组件 -->
<router-view></router-view>
```
4. 创建使用`Vue.extend`创建组件
```javascript
    // 4.1 使用 Vue.extend 来创建登录组件
    var login = Vue.extend({
      template: '<h1>登录组件</h1>'
    });

    // 4.2 使用 Vue.extend 来创建注册组件
    var register = Vue.extend({
      template: '<h1>注册组件</h1>'
    });
```
5. 创建一个路由 router 实例，通过 routers 属性来定义路由匹配规则
```javascript
  // 5. 创建一个路由 router 实例，通过 routers 属性来定义路由匹配规则
  var router = new VueRouter({
    routes: [
      { path: '/login', component: login },
      { path: '/register', component: register }
    ]
  });
```
6. 使用 router 属性来使用路由规则
```javascript
  // 6. 创建 Vue 实例，得到 ViewModel
  var vm = new Vue({
    el: '#app',
    router: router // 使用 router 属性来使用路由规则
  });
```

## 使用tag属性指定router-link渲染的标签类型

## 设置路由重定向

## 设置路由高亮

## 设置路由切换动效

## 在路由规则中定义参数
1. 在规则中定义参数：
```javascript
{ path: '/register/:id', component: register }
```
2. 通过 `this.$route.params`来获取路由中的参数：
```javascript
  var register = Vue.extend({
    template: '<h1>注册组件 --- {{this.$route.params.id}}</h1>'
  });
```

## 使用 `children` 属性实现路由嵌套
```javascript
  <div id="app">
    <router-link to="/account">Account</router-link>

    <router-view></router-view>
  </div>

  <script>
    // 父路由中的组件
    const account = Vue.extend({
      template: `<div>
        这是account组件
        <router-link to="/account/login">login</router-link> | 
        <router-link to="/account/register">register</router-link>
        <router-view></router-view>
      </div>`
    });

    // 子路由中的 login 组件
    const login = Vue.extend({
      template: '<div>登录组件</div>'
    });

    // 子路由中的 register 组件
    const register = Vue.extend({
      template: '<div>注册组件</div>'
    });

    // 路由实例
    var router = new VueRouter({
      routes: [
        { path: '/', redirect: '/account/login' }, // 使用 redirect 实现路由重定向
        {
          path: '/account',
          component: account,
          children: [ // 通过 children 数组属性，来实现路由的嵌套
            { path: 'login', component: login }, // 注意，子路由的开头位置，不要加 / 路径符
            { path: 'register', component: register }
          ]
        }
      ]
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {},
      components: {
        account
      },
      router: router
    });
  </script>
```

## 命名视图实现经典布局
1. 标签代码结构：
```javascript
<div id="app">
    <router-view></router-view>
    <div class="content">
        <router-view name="a"></router-view>
        <router-view name="b"></router-view>
    </div>
</div>
```
2. JS代码：
```javascript
  <script>
    var header = Vue.component('header', {
      template: '<div class="header">header</div>'
    });

    var sidebar = Vue.component('sidebar', {
      template: '<div class="sidebar">sidebar</div>'
    });

    var mainbox = Vue.component('mainbox', {
      template: '<div class="mainbox">mainbox</div>'
    });

    // 创建路由对象
    var router = new VueRouter({
      routes: [
        {
          path: '/', components: {
            default: header,
            a: sidebar,
            b: mainbox
          }
        }
      ]
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {},
      router
    });
  </script>
```
3. CSS 样式：
```javascript
  <style>
    .header {
      border: 1px solid red;
    }

    .content{
      display: flex;
    }
    .sidebar {
      flex: 2;
      border: 1px solid green;
      height: 500px;
    }
    .mainbox{
      flex: 8;
      border: 1px solid blue;
      height: 500px;
    }
  </style>
```
