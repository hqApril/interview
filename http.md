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