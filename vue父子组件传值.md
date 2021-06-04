## 父组件向子组件传值
1. 组件实例定义方式，注意：一定要使用`props`属性来定义父组件传递过来的数据
```javascript
<script>
  // 创建 Vue 实例，得到 ViewModel
  var vm = new Vue({
    el: '#app',
    data: {
      msg: '这是父组件中的消息'
    },
    components: {
      son: {
        template: '<h1>这是子组件 --- {{finfo}}</h1>',
        props: ['finfo']
      }
    }
  });
</script>
```
2. 使用`v-bind`或简化指令，将数据传递到子组件中：
```javascript
<div id="app">
    <son :finfo="msg"></son>
  </div>
```

## 子组件向父组件传值
1. 原理：父组件将方法的引用，传递到子组件内部，子组件在内部调用父组件传递过来的方法，同时把要发送给父组件的数据，在调用方法的时候当作参数传递进去；
2. 父组件将方法的引用传递给子组件，其中，`getMsg`是父组件中`methods`中定义的方法名称，`func`是子组件调用传递过来方法时候的方法名称
```javascript
<son @func="getMsg"></son>
```
3. 子组件内部通过`this.$emit('方法名', 要传递的数据)`方式，来调用父组件中的方法，同时把数据传递给父组件使用
```javascript
<div id="app">
  <!-- 引用父组件 -->
  <son @func="getMsg"></son>

  <!-- 组件模板定义 -->
  <script type="x-template" id="son">
    <div>
      <input type="button" value="向父组件传值" @click="sendMsg" />
    </div>
  </script>
</div>

<script>
  // 子组件的定义方式
  Vue.component('son', {
    template: '#son', // 组件模板Id
    methods: {
      sendMsg() { // 按钮的点击事件
        this.$emit('func', 'OK'); // 调用父组件传递过来的方法，同时把数据传递出去
      }
    }
  });

  // 创建 Vue 实例，得到 ViewModel
  var vm = new Vue({
    el: '#app',
    data: {},
    methods: {
      getMsg(val){ // 子组件中，通过 this.$emit() 实际调用的方法，在此进行定义
        alert(val);
      }
    }
  });
</script>
```
