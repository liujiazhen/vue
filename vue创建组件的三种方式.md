## 1、使用 Vue.extend 来创建全局的Vue组件

```javascript
  <div id="app">
    <!-- 如果要使用组件，直接，把组件的名称，以 HTML 标签的形式，引入到页面中，即可 -->
    <mycom1></mycom1>
  </div>

  <script>
    // 1.1 使用 Vue.extend 来创建全局的Vue组件
    // var com1 = Vue.extend({
    //   template: '<h3>这是使用 Vue.extend 创建的组件</h3>' // 通过 template 属性，指定了组件要展示的HTML结构
    // })
    // 1.2 使用 Vue.component('组件的名称', 创建出来的组件模板对象)
    // Vue.component('myCom1', com1)
    // 如果使用 Vue.component 定义全局组件的时候，组件名称使用了 驼峰命名，则在引用组件的时候，需要把 大写的驼峰改为小写的字母，同时，两个单词之前，使用 - 链接；
    // 如果不使用驼峰,则直接拿名称来使用即可;
    // Vue.component('mycom1', com1)

    // Vue.component 第一个参数:组件的名称,将来在引用组件的时候,就是一个 标签形式 来引入 它的
    // 第二个参数: Vue.extend 创建的组件  ,其中 template 就是组件将来要展示的HTML内容
    Vue.component('mycom1', Vue.extend({
      template: '<h3>这是使用 Vue.extend 创建的组件</h3>'
    }))


    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {}
    });
  </script>
```
## 2、直接使用 Vue.component 创建
```javascript
  <div id="app">
    <!-- 还是使用 标签形式,引入自己的组件 -->
    <mycom2></mycom2>
  </div>

  <script>
    // 注意:不论是哪种方式创建出来的组件,组件的 template 属性指向的模板内容,必须有且只能有唯一的一个根元素
    Vue.component('mycom2', {
      template: '<div><h3>这是直接使用 Vue.component 创建出来的组件</h3><span>123</span></div>'
    })

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {}
    });
  </script>
```
## 3、 在被控制的 #app 外面,使用 template 元素,定义组件的HTML模板结构 
```javascript
  <div id="app">
    <mycom3></mycom3>
    <!-- <login></login> -->
  </div>


  <div id="app2">
    <mycom3></mycom3>
    <login></login>
  </div>

  <!-- 在被控制的 #app 外面,使用 template 元素,定义组件的HTML模板结构  -->
  <template id="tmpl">
    <div>
      <h1>这是通过 template 元素,在外部定义的组件结构,这个方式,有代码的只能提示和高亮</h1>
      <h4>好用,不错!</h4>
    </div>
  </template>

  <template id="tmpl2">
    <h1>这是私有的 login 组件</h1>
  </template>

  <script>
    Vue.component('mycom3', {
      template: '#tmpl'
    })

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {}
    });


    var vm2 = new Vue({
      el: '#app2',
      data: {},
      methods: {},
      filters: {},
      directives: {},
      components: { // 定义实例内部私有组件的
        login: {
          template: '#tmpl2'
        }
      },
    })
  </script>
```
