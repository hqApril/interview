1. 什么是`RESTful`

   REST就是Representational State transfer 表述性状态转移，就是对资源的表述性状态转移；

   REST是一种设计架构的原则与规范，符合REST原则的架构即可称为RESTful

   REST架构的核心原则：统一接口

   主要原则

   * 网络上的所有事物都被抽象为资源
* 每个资源都具有唯一的资源标识符
   * 同一个资源具有多种表现形式
* 对资源的操作不会改变资源标识符
   * 所有的操作都是无状态的

2. 从输入url到页面展示的过程
    1. dns解析
    2. tcp三次握手
    3. 发送请求，分析url，设置请求报文
    4. 服务器返回html文件
    5. 浏览器渲染
        * HTML parser --> DOM Tree 构建dom树
        * CSS parser --> Style Tree 构建样式树
        * attachment --> Render Tree 结合dom树和style树，生成渲染树
        * layout布局，GPU绘制页面
    
3. 什么是XSS跨站脚本攻击

    xss跨站脚本攻击简单来说，就是攻击者想尽一切办法把可执行的代码嵌入到页面中，以达到非法窃取某些数据或者破坏的目的，根据场景不同，XSS攻击可分为几种类型：

    > **反射型**
    >
    > 反射型也称为非持久性XSS攻击，是指发生请求时，XSS代码会出现在请求的url中，作为参数提交到服务器，响应结果中包含XSS代码，最后通过浏览器进行解析并执行，一个反射型XSS可能如下所示：
    >
    > ```javascript
    > function ajax(url) {
    >     return new Promise((resolve, reject) => {
    >         setTimeout(() => {
    >             resolve(url.split('?')[1]);
    >         }, 1500);
    >     });
    > }
    > 
    > var url = 'https://www.baidu.com/getUserInfo?name=<script>alert(document.cookie)</script>';
    > 
    > ajax(url).then((data) => {
    >    document.body.inerAdjacentHTML('beforeEnd', data.split('=')[1]); 
    > });
    > ```
    >
    > **存储型**
    >
    > 存储型也称持久型XSS，它的主要攻击方式是将代码发送到服务器，最常见的存储型XSS攻击就是评论或者浏览攻击，一个存储型XSS可能如下图：
    >
    > ![存储型XSS](https://wangtunan.github.io/blog/assets/img/12.5c4e51ca.png)

    XSS防御

    >1. 将用户输入的内容，进行必要的标签转义，包括`<`、`>`、`/`等；
    >2. 在服务器端设置`cookie`属性为`httpOnly`防止客户端通过document.cookie读取；
    >3. 过滤一些危险字符串，例如`onerror`、`href`、`script`、`src`等；

4. CSP安全策略

    CSP安全策略本质上来说就是建立白名单机制，告诉浏览器哪些外部资源可以加载和执行，我们只需要配置，拦截主要交给浏览器；

    通常有两种方法设置CSP：

    >1. 通过设置`HTTP Header`的`Content-Security-Policy`
    >2. 通过`meta`标签来设置，例如：`<meta http-equiv="Content-Security-Policy" content="策略">`

5. CSRF跨域请求伪造

    CSRF攻击原理上是攻击者伪造一个后端请求地址，诱导用户进行点击，如果用户在已登录的情况下点击了这个危险链接，则后端服务器会认为用户在正常访问，攻击者可以从请求中拿到一些信息，进而进行攻击；

    CSRF防御

    >1. `get`请求不对数据进行修改
    >2. 不让第三方网站访问用户的cookie，可以通过cookie的SameSite属性
    >3. 阻止第三方网站请求
    >4. 进行请求时，附加`refer`验证和`token`验证；抵御CSRF攻击的关键在于，在请求中放入攻击者所不能伪造的信息，并且该信息总不存在于cookie之中；鉴于此，系统开发人员可以在http请求中以参数的形式加入一个随机产生的token，并在服务端进行token校验，如果请求中没有token或者token不正确，则认为该请求是CSRF攻击；

6. 点击劫持

    点击劫持是一种视觉欺骗手段，攻击者将需要攻击的网站通过iframe嵌套的方式嵌入自己的网页中，并将iframe设置为透明，在页面中透出一个按钮诱导用户点击；

    防御手段

    >设置http响应头`X-FRAME-OPTIONS`，它可以设置`DENY`、`SAMEORIGIN`、`ALLOW-FROM`分别表示不允许iframe展示、只允许同源iframe展示、指定域名iframe展示；

7. 中间人攻击

    中间人攻击时攻击方同时与服务端和客户端建立起了连接，并让对方认为连接是安全的，但是实际上整个通信过程都被攻击者控制了；攻击者不仅能获取到双方的通信信息，还能修改通信信息；一般来说使用`https`协议可以有效防止中间人攻击，但并不是说`https`就可以高枕无忧，因为攻击者可以通过某种方式从`https`降级到`http`进行访问；

    ![中间人攻击](https://wangtunan.github.io/blog/assets/img/13.d8ae3b54.jpg)