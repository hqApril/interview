1. html5新特性

    * html5现在已经不是SGML的子集了，主要是关于图像、位置、存储、多任务等功能的增加；
    * 拖拽释放api
    * 语义化更好的内容标签 header、footer、nav、article、aside、section等等
    * 音视频api audio、video
    * 画布canvas
    * 地理api Geolocation
    * 本地离线存储 localStorage、sessionStorage、indexDB等
    * 表单控件calendar、date、time、email、url、search等
    * 新的技术webworker、websocket

2. html5移除的标签

    basefont、big、center、font、s、strike、tt、u

    frame，frameset、noframes

3. 如何区分html和html5

    可以通过头部<!DOCTYPE html>

4. meta标签的name属性

    name属性主要用于描述网页，与之对应的属性值为content，content中的内容主要是便于搜索引擎机器人查找信息和分类信息用的；

    * name为keywords：keywords用来告诉搜索引擎你网页的关键字是什么；
    * name为description：description用来告诉搜索引擎网页的内容是什么；
    * name为robots：robots用来告诉搜索机器人哪些页面需要爬取，哪些页面不需要；

5. a标签中 active hover link visited 正确设置顺序是什么

    1. a:link
    2. a:visited
    3. a:hover
    4. a:active

6. 手机端，图片长时间点击会选中图片，怎么处理

    ```javascript
    img.onselect = function () {
    	return false;
    };
    ```

7. canvas在标签上设置width和height 和在style中设置宽高有什么区别

    在标签上设置width和height是画布的实际宽度和高度，而在style中设置的宽高是canvas在页面上被渲染的高度和宽度；如果canvas的width和height没有被指定或值不正确，就会被设置成默认值（300x150）；

8. 关于渐进增强和优雅降级

    > **渐进增强 progressive enhancement**
    >
    > 针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验；
    >
    > **优雅降级 graceful degradation**
    >
    > 一开始就构建完整的功能，然后再针对低版本浏览器进行兼容；
    >
    > 区别
    >
    > 优雅降级是从复杂的现状开始，并试图减少用户体验的供给；而渐进增强是从一个非常简单的能够起作用的版本开始，不断扩充，以适用未来环境的需要；
    >
    > 优雅降级认为应该针对那些最高级、最完善的浏览器来设计网站；而那些被认为过时的或有功能缺失的浏览器下的测试工作安排在开发周期的最后，并把测试对象限定为主流浏览器的前一个版本；
    >
    > 在这种设计范例下，旧版的浏览器被认为仅能提供”简陋却无妨“的浏览体验；可以做一些小的调整来适应某个特定浏览器；但由于它们并非我们所关注的焦点，因此除了修复较大的错误之外，其它的差异将被直接忽略；
    >
    > 渐进增强则认为应关注内容本身；
    >
    > 内容是我们建立网站的诱因；有的网站展示它，有的则收集它，有的寻求，有的操作，还有的网站甚至会包含以上的种种，但相同的观点是它们全都涉及到内容；这使得渐进增强成为了以各种更为合理的设计范例；

9. 常见浏览器及其内核

    * IE，trident
    * chrome，以前webkit，现在是blink，blink也是webkit衍生来的
    * safari，webkit内核
    * firefox，gecko
    * opera，presto，后面改成webkit，现在用blink
