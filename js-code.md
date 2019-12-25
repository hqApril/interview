> 为了保证兼容性，尽量用es5的方式写函数
>
> 不影响的部分就无所谓了，没啥意义
>
> 比如在不考虑作用域的地方用`let` `const`

1. 实现浅拷贝

    ```javascript
    // 这里默认source是对象，就不做验证了
    // es5
    function shadowCopy(source) {
        var obj = {};
        
        for (var key in source) {
            if (source.hasOwnProperty(key)) {
                obj[key] = source[key];
            }
        }
        
        return obj;
    }
    ```

    ```javascript
    // es6
    function shadowCopy(source) {
        return {...source}; // 扩展运算符
        return Object.assign({}, source); // Object.assign();
    }
    ```

2. 实现深拷贝

    ```javascript
    // 利用 JSON 的序列化和反序列化，可以实现简易的深拷贝
    // 缺点
    // 1. 属性值为undefined的属性会被忽略
    // 2. 属性名为Symbol的属性会被忽略
    // 3. 函数无法序列化，会被忽略
    // 4. 循环引用无法解决，会直接报错
    function deepClone(source) {
        return JSON.parse(JSON.stringify(source));
    }
    
    // P.S
    const obj = {
        name: 'apr',
        age: undefined,
        sex: null,
        [Symbol()]: 100,
        sayHello: function () {
            console.log('hello');
        }
    };
    
    console.log(deepClone(obj)); // { name: "apr", sex: null }
    ```

    ```javascript
    // 正经实现一个深拷贝函数
    // 感觉不是很完善的样子，面试应该够用了吧
    // 有心情去看看lodash的cloneDeep的完整实现
    function deepClone(source) {
        var copy;
        
        // 过滤掉五种原始类型和function
        if (!source || typeof source !== 'object') {
            return source;
        }
        
        // 拷贝Date类型
        if (source instanceof Date) {
            copy = new Date();
            copy.setTime(source.getTime());
            
            return copy;
        }
        
        // 拷贝Array类型
        if (source instanceof Array) {
            copy = [];
            
            for (var i = 0; i < source.length; i++) {
                copy.push(deepClone(source[i]));
            }
            
            return copy;
        }
        
        // 拷贝Object类型
        if (source instanceof Object) {
            copy = {};
            
            for (var key in source) {
                if (source.hasOwnProperty(key)) {
                    copy[key] = deepClone(source[key]);
                }
            }
            
            return copy;
        }
        
        throw new Error('Unable to copy source!');
    }
    ```

3. 实现getQueryString

    ```javascript
    // url 为 window.location.href
    function getQueryString(name) {
        var query = window.location.search.substring(1);
        
        query.split('&').forEach(function(param) {
           var arr = param.split('=');
            
           if (arr[0] === name) {
               return arr[1];
           }
        });
    }
    ```

4. 实现setInterval

    ```javascript
    function mySetInterval(callback, interval) {
        var timer = null;
        var now = Date.now;
        var startTime = now();
        var endTime = startTime();
        var loop = functon () {
            timer = requestAnimationFrame(loop);
            endTime = now();
            
            if (endTime - startTime >= interval) {
                callback && callback(timer);
            }
        }
        
        timer = requestAnimationFrame(loop);
        
        return timer;
    }
    ```

5. 实现call方法

    ```javascript
    // 详细可以加一些参数类型验证，这里不加了
    Function.prototype.myCall = function(context) {
        if (typeof this !== 'function') {
            throw new TypeError('error');
        }
        
        context = context || window;
        context.fn = this;
        var args = Array.prototype.slice.call(arguments, 1);
        var result = context.fn(...args); // 这里就可以绑定this到context中了
        delete context.fn;
        
        return result;
    }
    ```

6. 实现apply方法

    ```javascript
    Function.prototype.myApply = function(context) {
        if (typeof this !== 'function') {
            throw new TypeError('error');
        }
        
        context = context || window;
        context.fn = this;
        
        var args = arguments[1];
        
        var result = context.fn(...args);
        delete context.fn;
        
        return result;
    }
    ```

7. 实现bind方法

    ```javascript
    Function.prototype.myBind = function(context) {
        if (typeof this !== 'function') {
            throw new TypeError('error');
        }
        
        var self = this;
        var args = Array.prototype.slice.call(arguments, 1);
        
        return function Func() {
            if (this instanceof Func) {
                return new self(...args, ...arguments);
            }
            
            return self.apply(context, args.concat(...arguments));
        }
    }
    ```

8. 实现防抖函数

    ```javascript
    // 防抖就是，在单位时间内，函数只能被触发一次，如果多次触发，则重新计算单位时间，继续往后延迟
    function debounce(fn, delay) {
        var timer = null;
        
        return function () {
            clearTimeout(timer);
            
            var args = arguments;
            
            timer = setTimeout(function () {
                timer = null;
                
                fn.apply(this, args);
            }, delay);
        }
    }
    ```

9. 实现节流函数

    ```javascript
    // 节流就是在单位时间内，函数只能被触发一次，但是多次触发，不重新计算时间，直到函数运行为止
    function throttle(fn, interval, first) {
        var timer = null;
        
        return function () {
            if (first) {
                first = false;
                
                fn.apply(this, arguments);
                
                return;
            }
            
            if (timer) {
                return;
            }
            
            var self = this;
            var args = arguments;
            
            timer = setTimeout(function () {
                timer = null;
                
                fn.apply(self, args);
            }, interval);
        }
    }
    ```

10. 实现instanceof

    ```javascript
    function myInstanceof(left, right) {
    	let prototype = right.prototype;
        
        while (left) {
            if (left.__proto__ === prototype) {
                return true;
            }
            
            left = left.__proto__;
        }
        
        return false; 
    }
    ```

11. 自定义对象的迭代器，从而可以使用 for of 以及 扩展运算符等

     ```javascript
     const obj = {
         name: 'apr',
         sex: 'male',
         age: 18,
     }
     
     Object.defineProperty(obj, Symbol.iterator, {
         writable: false,
         enumerable: false,
         configurable: true,
         value: function () {
             let self = this;
             
             const keys = Object.keys(self);
             let index = 0;
             
             return {
                 next: function () {
                     return {
                         value: self[keys[index++]],
                         done: index > keys.length
                     };
                 }
             };
         }
     });
     
     console.log([...obj]); // ["apr", "male", 18]
     
     for (val of obj) {
         console.log(val); // "apr" "male" 18
     }
     ```

12. 实现图片懒加载

     ```javascript
     var myImage = (function () {
     	var imgNode = document.createElement('img');
         document.body.appendChild(imgNode);
         var img = new Image();
         
         img.onload = function () {
             imgNode.src = img.src;
         }
         
         return {
             setSrc: function (src) {
                 imgNode.src = './img/loading.gif';
                 img.src = src;
             }
         }
     })();
     ```

13. 实现事件委托，循环创建10个li标签，点击li标签是，打印其对应的索引

     ```html
     <ul id="list"></ul>
     ```

     ```javascript
     var list = document.getElementById('list');
     
     for (let i = 0; i < 10; i++) {
         var li = document.createElement('li');
         li.setAttribute('data-index', i);
         li.innerText = i;
         list.appendChild(li);
     }
     
     list.onclick = function (e) {
         e = e || window.event;
         
         var target = e.target || e.srcElement;
         
         if (target.nodeName.toLowerCase() === 'li') {
             console.log(target.dataset.index);
         }
     }
     ```

14. 实现原生ajax请求

     ```javascript
     // get 请求
     var xhr = new XMLHttpRequest();
     
     xhr.open('get', 'http://www.test.com?name=apr&age=18');
     xhr.send();
     
     xhr.onreadystatechange = function () {
         if (xhr.readyState === 4 && xhr.status === 200) {
             console.log('success');
         }
     }
     ```

     ```javascript
     // post 请求
     var xhr = new XMLHttpRequest();
     
     xhr.open('post', 'http://www.test.com');
     xhr.send('name=apr&age=18');
     
     xhr.onreadystatechange = function () {
         if (xhr.readyState === 4 && xhr.status === 200) {
             console.log('success');
         }
     }
     ```

15. 实现AOP

     ```javascript
     Function.prototype.before = function (beforeFn) {
         var self = this;
         
         return function () {
             beforeFn.apply(this, arguments);
             
             return self.apply(this, arguments);
         }
     }
     
     Function.prototype.after = function (afterFn) {
         var self = this;
         
         return function () {
             var ret = self.apply(this, arguments);
             
             afterFn.apply(this, arguments);
             
             return ret;
         }
     }
     
     var func = function () {
         console.log(2);
     }
     
     func = func.before(function () {
        console.log(1); 
     }).after(function () {
         console.log(3);
     });
     
     func();
     ```

16. 实现柯里化函数

     ```javascript
     var currying = function (fn) {
         var args = [];
         
         return function () {
             if (arguments.length === 0) {
                 return fn.apply(this, args);
             } else {
                 Array.prototype.push.apply(args, arguments);
                 
                 return arguments.callee;
             }
         }
     }
     
     var cost = currying(function () {
         var args = Array.prototype.slice.call(arguments);
         let sum = 0;
         
         for (let i = 0; i < args.length; i++) {
             sum += args[i];
         }
         
         return sum;
     });
     
     console.log(cost(1)(2)(3)(4)()); // 10
     ```

     ```javascript
     // 实战题
     // 实现一个add方法，是计算结果能够满足如下预期
     // add(1)(2)(3) = 6;
     // add(1, 2, 3)(4) = 10;
     // add(1)(2)(3)(4)(5) = 15;
     
     function add() {
         var args = Array.prototype.slice.call(arguments);
         
         var adder = function () {
             Array.prototype.push.apply(args, arguments);
             
             return adder;
         }
         
         adder.toString = function () {
           	let sum = 0;
             
             for (let i = 0; i < args.length; i++) {
                 sum += args[i];
             }
             
             return sum;
         };
         
         return _adder;
     }
     
     console.log(add(1)(2)(3)); // 6
     console.log(add(1, 2, 3)(4)); // 10
     console.log(add(1)(2)(3)(4)(5)); // 15
     ```

17. 实现分时函数

     ```javascript
     // 把共创建1000个节点，改成每隔200ms创建10个节点，反之短时间内创建大量节点
     var timeChunk = function (arr, fn, count, interval) {
         var timer = null;
         var data = null;
         
         var start = function () {
             for (var i = 0; i < Math.min(count || 1. arr.length); i++) {
                 data = arr.shift();
                 
                 fn(data);
             }
         };
         
         return function () {
           timer = setInterval(function () {
               if (arr.length === 0) {
                   clearInterval(timer);
                   
                   timer = null;
                   
                   return;
               }
               
               start();
           }, interval || 200);  
         };
     }
     
     var arr = [];
     
     for (var i = 0; i < 1000; i++) {
         arr.push(i);
     }
     
     var renderDomList = timeChunk(arr, function (data) {
         var div = document.createElement('div');
         div.innerHTML = data;
         document.body.appendChild(div);
     }, 10, 200);
     
     renderDomList();
     ```

18. 手动实现jsonp

     ```javascript
     var jsonp = function (url, data, callback) {
         var cbName = 'callback_' + new Date().getTime();
         var queryString = url.indexOf('?') < 0 ? '?' : '&';
         
         for (var key of data) {
             queryString += k + '=' + data[key] + '&';
         }
         
         queryString += 'callback=' + cbName;
         
         var script = document.createElement('script');
         
         script.src = url + queryString;
         
         window[cbName] = function (data) {
             callback(data);
             
             document.body.removeChild(script);
         }
         
         document.body.appendChild(script);
     };
     
     jsonp('http://www.baidu.com', {}, function (data) {
         console.log(data);
     });
     ```

     
