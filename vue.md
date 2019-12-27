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

   

