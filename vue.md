1. 什么是MVVM

   MVVM是Model-View-ViewModel的缩写；

   **Model**代表数据模型，也可以在Model中定义数据修改以及操作的业务逻辑；

   **View**代表UI组件，它负责将数据模型转化成UI展现出来；

   **ViewModel**监听模型数据的改变以及控制视图行为、处理用户的交互，可以理解为一个同步Model和View的对象，负责连接View和Model；

   在MVVM框架下，View和Model没有直接的联系，而是通过ViewModel的形式进行交互，Model和ViewModel之间的交互是双向的，因此View数据的变化会同步到Model上，而Model数据的变化也会立即反应到View上；

   **ViewModel**通过双向数据绑定把View和Model连接起来，而View和Model之间的同步工作是完全自动的，无需人为的干涉，因此开发者只需要关注业务逻辑，不需要手动操作DOM，也不需要关心数据状态的同步问题，让复杂的数据状态维护完全由MVVM来统一管理；

2. vue的生命周期
   1. **beforeCreate** --- 实例初始化之后，此时的数据观察和事件机制都没有形成，不能获取dom节点 --- 对应钩子函数**beforeCreate**
   
   2. **created** --- vue 实例已经创建，仍然不能获取dom元素 --- 对应钩子函数**created**
   
   3. **beforeMount** --- 这一阶段，我们虽然仍然得不到具体的dom元素，但是vue挂载的根节点已经创建，下面vue对dom的操作将围绕这个这个根元素进行；beforeMount是过渡性的，一般一个项目只能用到一两次 --- 对应钩子函数**beforeMount**
   
   4. **mounted** --- 一般我们的异步操作都写在这里，在这个阶段，数据和dom都已经被渲染出来了 --- 对应钩子函数**mounted**
   
   5. **beforeUpdate** --- 这一阶段，vue遵循数据驱动dom的原则；beforeUpdate函数在数据更新后虽然没立即更新视图，但是dom中的数据会改变，这是vue双向绑定的作用，vue最开始初始化的时候不会被调用 --- 对应钩子函数**beforeUpdate**
   
   6. **updated** --- 这一阶段，dom会和更改过的内容同步 ，vue最开始初始化的时候不会被调用--- 对应钩子函数**updated**
   
   7. **beforeDestory** --- 当我们调用distory方法时，会销毁当前vue实例，在销毁之前，会触发beforeDestory钩子 --- 对应钩子函数**beforeDestory**
   
   8. **destroyed** --- 销毁之后，会触发该钩子函数 --- 对应钩子函数**destroyed**
   
      ![vue生命周期示意图](https://cn.vuejs.org/images/lifecycle.png)
   
3. 什么是vue的生命周期

   生命周期就是vue实例在创建到销毁一系列过程，从开始创建、初始化数据、编译模板、挂载dom -> dom渲染、数据更新 -> dom渲染、实例销毁，这一系列的过程；

4. 生命周期的作用是什么

   vue实例的生命周期中，添加了多个事件钩子，让我们更好去控制整个vue实例化过程，方便我们编写自己的逻辑代码；

5. vue生命周期一共有几个阶段

   一共有八个阶段 创建前/创建后、载入前/载入后、更新前/更新后、销毁前/销毁后；

6. 第一次加载会触发哪几个钩子

   beforeCreate、created、beforeMount、mounted（dom在这个钩子时已经渲染完毕）

7. 实现简单的数据双向绑定

   ```html
   <div id="root">
       <input type="text" id="txt" />
       
       <p id="monitor"></p>     
       </p>
   </div>
   ```

   ```javascript
   var obj = {};
   
   Object.defineProperty(obj, 'txt', {
       get: function () {
           return obj;
       },
       set: function (newValue) {
           document.getElementById('txt').value = newValue;
           document.getElementById('monitor').innerHTML = newValue;
       }
   });
   
   document.getElementById('txt').oninput = function (e) {
       obj.txt = e.target.value;
   }
   ```

8. vue的响应式原理

   当一个vue实例创建时，vue会遍历data选项的属性，用Object.defineProperty将他们转化为getter/setter并在内部追踪相关的依赖，在属性被访问和修改时通知变化；每个组件实例都有相应的watcher程序实例，他会在组件渲染的过程中把属性记录为依赖，之后当依赖项的setter被调用时，会通知watcher重新计算，从而致使它关联的组件得以更新；

9. v-html 和 v-text 区别

   * v-html: 将数据输出到元素内部，如果输出的数据有html代码，会被正常渲染（innerHTML）
   * v-text: 将元素输出到元素内部，如果输出的数据有html代码，会作为普通文本输出（innerText）
   * {{}}: 作用同v-text，其问题是，在vue实例创建过程中，有可能会出现值闪烁的问题，可以通过v-cloak解决

10. $route和$router区别

    $route是“路由信息对象”，包括path、params、hash、query、fullPath、matched、name等路由信息参数，而$router是“路由实例”对象，包括了路由的跳转方法、钩子函数等；
    
11. computed、watch、methods区别

    computed是计算属性，用来声明式地描述一个值依赖了其他值；当你在模板中把数据绑定到一个计算属性上时，vue会在其依赖的任何值改变时去更新dom；这个功能可以让你的代码更加声明式，数据驱动且易于维护；

    watch是监听属性，监听你定义的变量，当你定义的变量的值变化时，调用对应的方法；方法里的形参对应的是参数的新值和旧值；

    计算属性不能计算data中已经定义过的变量；

    methods是函数集合，不会使用缓存；

12. `<keep-alive></keep-alive>`的作用是什么

    `<keep-alive></keep-alive>`包裹动态组件时，会缓存不活动的组件实例，主要用于保留组件状态或避免重新渲染；

13. 什么是`$nextTick`

    `$nextTick`是在下次dom更新循环结束之后执行延迟回调，在修改数据之后使用`$nextTick`，则可以再回调中获取更新之后的dom

14. 子组件调用父组件的方法

    * 一种方法是子组件中直接通过`this.$parent.event`来调用父组件的方法
    * 一种是通过`$emit来向父组件发送一个事件，父组件直接监听这个事件就行了
    
15. vue的路由实现：hash模式和history模式

    > **hash模式**
    >
    > 在url后加上符号#，#以及#后面的字符称之为hash，用window.location.hash读取；
    >
    > hash虽然在url中，但不被包括在http请求中，用来知道浏览器动作，对服务器安全无用，hash不会重载页面；
    >
    > **history模式**
    >
    > history采用了html5的新特性，且提供了两个新的方法：pushState()、replaceState()；可以对浏览器历史记录栈进行修改，以及popState事件的监听到状态变更；
    >
    > history模式下，前端的url必须和实际向后端发起请求的url一致；

16. vue.js的两大核心

    `数据驱动`和`组件系统`

17. vue更新数组时触发视图更新的方法

    push、pop、shift、unshift、splice、sort、reverse
