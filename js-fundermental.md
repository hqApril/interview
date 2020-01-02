1. javascript原始类型

    * string
    * number
    * null
    * undefined
    * boolean
    * symbol(es6新增)

2. 什么是对象类型

    在javascript中，除了原始类型，剩下的都是对象类型；原始类型存储的都是具体的值，而对象类型存储的引用地址。

3. 关于typeof

    typeof 对除了null以外的原始类型，可以准确判断具体的类型

    ```javascript
    typeof '1'           // string
    typeof 1		   	// number
    typeof true			// boolean
    typeof undefined     // undefined
    typeof null          // object 
    typeof Symbol()      // symbol
    ```

    对于对象类型，除了函数会返回function以外，其它对象类型都是返回object

    ```javascript
    typeof function () {} // function
    typeof {}			 // object
    ```

4. 关于instanceof

    instanceof用于检测构造函数的prototype属性是否出现在某个实例对象的原型链上

    ```javascript
    function Car() {}
    
    let car = new Car();
    
    console.log(car instanceof Car);      // true
    console.log(car instanceof Object);   // true
    ```

5. 关于类型转换

    javascript中，类型转换只有三种

    * 转换成数字
    * 转换成布尔值
    * 转换成字符串

    ```javascript
    console.log([] == ![]);     // true
    Number('1a');               // NaN
    Number('a1');			   // NaN
    ```

    转boolean

    除了`undefined` `null` `false` `0` `-0` `NaN` `''` 转换成false之外，其它所有值都转成true，包括所有对象

    对象转原始类型
    
    该转换会调用内置的[ToPrimitive]函数，逻辑为
    
    1. 如果已经是原始类型，则直接返回
    2. 调用valueOf()，如果转换成了原始类型，则返回
    3. 调用toString(), 如果转换成了原始类型，则返回
    4. 也可以重写Symbol.toPrimitive()方法，优先级别最高
    5. 如果都没有返回原始类型，则会报错
    
    ```javascript
    let obj = {
        value: 0,
        valueOf() {
            return 1;
        },
        toString() {
            return '2';
        },
        [Symbol.toPrimitive]() {
            return 3;
        }
    }
    
    console.log(obj + 1);    // 4
    ```
    
    ```javascript
    // Q: 如何定义变量a，使 a == 1 && a == 2 && a == 3 为true？
    let a = {
        value: 1,
        valueOf() {
            return this.value++;
        }
    }
    
    if (a == 1 && a == 2 && a == 3) {
        console.log('true');  // true
    }
    ```
    
6. 关于 == 和 ===

    == 比较预算符会先转换操作数，然后再进行比较。

    * 如果有一个操作数是布尔值，则会先将该值转换为数字 -- false为0，true为1
    * 如果有一个操作数是字符串，另一个是数字，则会先将字符串转换为数字
    * 类型相同不会进行比较

    === 比较运算符不会转换操作数，严格相等，左右两边不止需要值相等，也要类型相等

    ```javascript
    null == undefined       // true
    null === null           // true
    undefined === undefined // true
    {} == {}                // false
    {} === {}               // false
    ```

7. 关于this

    this只有如下几种情况，按优先级由低到高

    1. 独立函数调用，例如`foo()` ，指向全局对象window，在严格模式下，指向undefined

        ```javascript
        function foo() {
            console.log(this === window); // true
        }
        
        foo();
        
        function bar() {
            'use strict';
            console.log(this === window); // false
            console.log(this === undefined); // true
        }
        
        bar();
        ```

    2. 对象调用，例如`obj.foo()`，此时this指向obj对象

    3. `call()` `apply()` `bind()` 改变上下文的方法，this指向这些方法的第一个参数，当第一个参数为null时，this指向全局对象window，严格模式下，指向null；当第一个参数不传时，this指向全局变量window，严格模式下，指向undefined；

        ```javascript
        function foo() {
            console.log(this);
        }
        
        foo.call(null); // window
        foo.call(); // window
        
        function bar() {
            'use strict';
            console.log(this);
        }
        
        bar.call(null); // null
        bar.call(); // undefined
        ```

    4. 箭头函数没有this，箭头函数里面的this只取决于包裹箭头函数的第一个普通函数的this；即箭头函数的this指向取决于该函数创建的环境；

    5. new实例化一个构造函数，this永远指向该构造函数返回的实例上，优先级最高；

        ![this](https://wangtunan.github.io/blog/assets/img/3.4fd85c39.png)

8. 关于闭包

    闭包的几种表现形式

    1. 返回一个函数
    2. 作为函数传递
    3. 回调函数
    4. IIFE 立即执行函数

9. 关于原型、原型链

    每个函数都有个prototype属性，prototype也是只有函数才有的属性

    每个对象（除了null）在创建的时候就会关联一个对象，这个对象就是我们所说的原型，每一个对象都会从原型继承属性（应该说，该对象查找不到属性的时候，会向上沿原型链到原型上查找属性）；每个对象都有一个\_\_proto__属性，指向对应的构造函数的prototype

    ```javascript
    function Foo() {}
    
    const foo = new Foo();
    
    foo.__proto__ === Foo.prototype; // true
    // constructor 指向它的构造函数
    foo.constructor === Foo; // true
    foo.__proto__ === foo.constructor.prototype; // true
    ```

    另外几种创建对象时，原型的指向

    ```javascript
    // 字面量
    const a = {};
    
    a.__proto__ === Object.prototype; // true;
    
    // Object.create
    const b = Object.create(a);
    b.__proto__ === a; // true;
    ```

    由于\_\_proto\_\_是任何对象都有的属性，而js里万物皆对象，所以最终会形成一条由\_\_proto\_\_连起来的链条,递归访问\_\_proto\_\_最终一定会到头，并且值是null；

    js引擎查找对象的属性时，先查找对象本身是否存在该属性，如果不存在，则会沿原型链查找（不会查找自己的prototype）;

10. 关于继承

    继承的几种方式

    1. 原型链实现继承
    2. 借用构造函数实现继承
    3. 组合继承
    4. 寄生组合继承
    5. 类继承

    原型链实现继承

    通过重写子类的原型，并将它指向父类。这种方式实现的继承，创建出来的实例，既是子类的实例，又是父类的实例。它的缺陷有

    1. 不能向父类构造函数传参
    2. 父类上的引用类型属性会被所有实例共享，其中一个实例改变时，会影响其它实例

    ```javascript
    function Animal() {
        this.colors = ['red', 'blue'];
    }
    
    function Dog(name) {
        this.name = name;
    }
    
    Dog.prototype = new Animal();
    
    var dog1 = new Dog('wangwang');
    var dog2 = new Dog('miaomiao');
    
    dog2.colors.push('yellow');
    
    console.log(dog1.colors); // ["red", "blue", "yellow"]
    console.log(dog2.colors); // ["red", "blue", "yellow"]
    
    console.log(dog1 instanceof Dog); // true
    console.log(dog1 instanceof Animal); // true
    ```

    借用构造函数实现继承

    通过在子类中使用call方法，实现借用父类构造函数并向父类构造函数传参的目的，无法继承父类原型对象上的属性和方法，只能借用构造函数

    ```javascript
    function Animal(name) {
        this.name = name;
        this.colors = ['red', 'blue'];
    }
    
    Animal.prototype.eat = function () {
        console.log(this.name + ' is eating!');
    }
    
    function Dog(name) {
        Animal.call(this, name);
    }
    
    var dog1 = new Dog('wangwang');
    var dog2 = new Dog('miaomiao');
    
    dog2.colors.push('yellow');
    
    console.log(dog1.colors); // ["red", "blue"]
    console.log(dog2.colors); // ["red", "blue", "yellow"]
    
    console.log(dog1 instanceof Dog); // true
    console.log(dog2 instanceof Animal); // false
    
    console.log(dog1.eat()); // error
    ```

    组合继承

    结合上面两种方法，保留两种继承方式的优点，它的缺点：父类构造函数被调用很多次

    ```javascript
    function Animal(name) {
        this.name = name;
        this.colors = ['red', 'blue'];
    }
    
    Animal.prototype.eat = function () {
        console.log(this.name + ' is eating!');
    }
    
    function Dog(name) {
        Animal.call(this, name);
    }
    
    Dog.prototype = new Animal();
    
    var dog1 = new Dog('wangwang');
    var dog2 = new Dog('miaomiao');
    
    dog1.colors.push('yellow');
    
    console.log(dog1.name); // wangwang
    console.log(dog2.colors); // ["red", "blue"]
    dog2.eat(); // miaomiao is eating!
    ```

    寄生组合继承

    在组合继承的基础上，采用`Object.create()`来改造实现

    ```javascript
    function Animal(name) {
        this.name = name;
        this.colors = ['red', 'blue'];
    }
    
    Animal.prototype.eat = function () {
        console.log(this.name + ' is eating!');
    }
    
    function Dog(name) {
        Animal.call(this, name);
    }
    
    Dog.prototype = Object.create(Animal.prototype);
    Dog.prototype.constructor = Dog;
    
    var dog1 = new Dog('wangwang');
    var dog2 = new Dog('miaomiao');
    
    dog1.colors.push('yellow');
    
    console.log(dog1.name); // wangwang
    console.log(dog2.colors); // ["red", "blue"]
    dog2.eat(); // miaomiao is eating!
    ```

    Class类实现继承

    ```javascript
    // es6
    class Animal {
        constructor(name) {
            this.name = name;
            this.colors = ['red', 'blue'];
        }
        
        eat() {
            console.log(this.name + ' is eating');
        }
    }
    
    class Dog extends Animal {
        constructor(name) {
            super(name);
        }
    }
    
    var dog1 = new Dog('wangwang');
    var dog2 = new Dog('miaomiao');
    
    dog1.colors.push('yellow');
    
    console.log(dog1.name); // wangwang
    console.log(dog2.colors); // ["red", "blue"]
    dog2.eat(); // miaomiao is eating!
    ```

11. var、let以及const的区别

     > 1. var 声明的变量会提升到作用域的顶部，而 let 和 const 不会进行变量提升
     > 2. var 声明的全局变量会被挂载到 window 对象下，作为 window 的属性，而 let 和 const 不会
     > 3. 可以使用 var 重复声明一个变量，而 使用 let 和 const 这么做会报错
     > 4. var 声明的变量作用域范围是函数作用域，而 let 和 const 声明的变量作用域是块级作用域
     > 5. const 声明的常量，一旦声明则不能再次赋值，再次赋值会报错

     ```javascript
     // 作用域提升
     console.log(a); // undefined
     console.log(b); // error
     consol.log(PI); // error
     
     var a = 'a';
     let b = 'b';
     const PI = 3.1415926;
     ```

     ```javascript
     // 挂载到全局变量
     var a = 'a';
     let b = 'b';
     const PI = 3.1415926;
     
     console.log(window.a); // a
     console.log(window.b); // undefined
     console.log(window.PI); // undefined
     ```

     > es6 基础先扔着吧，没啥想写的

     

12. 并发与并行的区别

     并发是宏观概念，有一起执行的两个任务，在一段时间内，通过任务的切换，完成这两个任务，这种情况称之为并发；

     并行是微观概念，假设cpu有两个核心，有一起执行的两个任务，分别调用一颗cpu来执行并完成任务，这种情况叫做并行（两个任务是真同时执行的）；

13. 进程与线程的区别

     进程是程序执行时的一个实例，是系统进行资源分配的基本单位。不同的进程拥有不同的虚拟地址空间，而同一进程内的不同线程共享同一地址空间；

     线程是程序执行流的最小单元，是进程中的一个实体，是被系统独立调度和分派的基本单位；线程与资源分配无关，不拥有自己的系统资源，线程属于某一个进程；

     通常一个进程中可以包含多个线程，它们共享和利用线程所拥有的资源。但是一个线程只能属于一个进程；一个线程中的进程相对其它线程不可见；
    
14. 关于Proxy（es6 新增代理）

     proxy可以为对象增加代理功能，实现对数据set、get、has等操作进行监听；

15. 数组map、filter和reduce

     ```javascript
     // map 遍历整个数组，对数组元素做修改，不修改原数组
     const arr = [1, 2, 3];
     const newArr = arr.map((val, key, arr) => val * 2);
     
     console.log(arr); // [1, 2, 3]
     console.log(newArr); // [2, 3, 6]
     ```

     ```javascript
     // filter 遍历整个数组，根据返回值筛选元素生成新数组，不修改原数组
     const arr = [0, 1, 2, 3];
     const newArr = arr.filter((val, key, arr) => val % 2);
     
     console.log(arr); // [0, 1, 2, 3]
     console.log(newArr); // [1, 3]
     ```

     ```javascript
     // reduce 接受一个函数，将原数组中的元素最终转换为一个值，第一个参数是回调函数，第二个参数是初始值
     const arr = [0, 1, 2, 3];
     let sum = arr.reduce((account, current) => {
         return account + current;
     });
     
     console.log(sum); // 6
     ```

16. 什么是事件委托

     由于给页面上过多的dom节点挂载事件处理程序会导致页面性能的下降，所以可以用事件委托的方式达到性能优化的目的，其原理是利用事件冒泡的特性，把事件处理程序挂载到层级尽可能高的dom节点上，从而处理相应的事件；

17. 什么是AOP

     AOP 就是 Aspect Oriented Programming 的 缩写，也就是 面向切面编程；是函数式编程的衍生范式；主要作用是把一些跟核心业务无关的功能与模块抽离出来，例如日志统计，安全控制，异常处理等；把这些功能抽离出来之后，再通过动态植入的方式掺入业务模块中；达到降低代码耦合度的需求；

18. 关于事件绑定

     事件绑定有两种方式，分别是主流浏览器的addEventListener和ie的attachEvent

     ```javascript
     var el = document.getElementById('root');
     var func = function () {};
     
     // 第三个参数，默认false，false为事件冒泡，true为事件捕获
     el.addEventListener('click', func, false);
     
     el.removeEventListener('click', func);
     ```

     ```javascript
     var el = document.getElementById('root');
     var func = function () {};
     
     // attachEvent只支持事件冒泡
     el.attachEvent('onclick', func);
     el.detachEvent('onclick', func);
     ```

19. 阻止事件冒泡

     `stopPropagation()`和`stopImmediapropagation()`都可以阻止事件的冒泡，不过`stopPropagation`还可以阻止目标执行别的注册事件

     ```javascript
     // 阻止事件冒泡
     // 不阻止事件冒泡的时候，window的click会触发
     // 使用stopPropagation时，window的click不会触发
     // 使用stopImmediaPropagation时，dom的捕获事件不会触发，window的click不会触发
     var el = document.getElementById('el');
     
     el.addEventListener('click', function (e) {
         console.log('冒泡时触发');
         
     	// e.stopPropagation();
         // e.stopImmediaPropagation();
     });
     
     el.addEventListener('click', function (e) {
         console.log('捕获时触发');
     }, true);
     
     window.addEventListener('click', function (e) {
         console.log('向上冒泡时触发');
     });
     ```

20. 什么是同源策略

     同源策略是指，一个源的客户端脚本在没用明确授权的情况下，不能够访问另一个源的客户端脚本。两个url之间，只要协议、域名、端口号有一个不同，就会出现跨域；

     不同源受到的限制

     * 无法读取cookie
     * 无法获取dom
     * 无法发送ajax请求

     实现跨域的几种方式

     * jsonp
     * CORS
     * document.domain
     * postMessage

21. 关于cookie

     ```javascript
     // 设置cookie
     function setCookie(cname, cvalue, exdays) {
     	var d = new Date();
         d.setTime(d.getTime() + exdays * 24 * 60 * 60 * 1000);
         var expires = "expires=" + d.toUTCString();
         
         document.cookie = cname + '=' + cvalue + '; ' + expires;
     }
     ```

     ```javascript
     // 获取cookie
     function getCookie(name) {
         var arr = document.cookie.match(new RegExp('(^| )' + name + '=([^;]*)(;|$)'));
         
         if (arr) {
             return unescape(arr[2]);
         }
         
         return null;
     }
     ```

22. 关于`localStorage`和`sessionStorage`

     localStorage和sessionStorage都是用来存储客户端临时信息的对象，他们均只能存储字符串类型的对象，且均是以kv的形式存储的；

     localStorage的生命周期是永久的，这意味着除非用户主动清除localStorage信息，否则这些信息将会永远存在；

     sessionStorage的生命周期是在当前窗口或标签存在的这段时间，一旦窗口或标签被关闭，那么所有的sessionStorage储存的数据也将会被清空；

     不同的浏览器无法共享localStorage和sessionStorage，相同浏览器的同源页面可以共享相同的localStorage，但是不同页面或标签无法共享sessionStorage；这里的页面及标签指的是顶级窗口，如果一个标签页包含多个iframe标签，那么他们属于同源页面，那么他们之间可以共享sessionStorage；