
##JS 部分
1. 如何获得一个变量的正确类型？
  ```js
  Object.prototype.toString.call(xxx)
  ```

2. 对象转换为基本类型
  * 会自动类型转换：弱类型
  * 不会自动类型转换：强类型
  ```js
  //可调用 valueOf() / toString() 方法
  // valueOf() 与 toString() 是可以重写的；valueOf()的优先级高于toString()
  ```

3. 四则运算符
  ```js
  1 + '2' = "12";
  2 * '2' = 4; 4 - '2' = 2
  'a' + + 'b' = 'aNaN' //因为 +'b' = NaN; 加号会做类型强制转换
  ```

4. typeof
  * 对于基本类型，除null外，typeof 的结果为其类型
  * 对于引用类型，除函数外，typeof 的结果为'object'

5. 操作符
  ```js
  [] == ![]   => true  // ![] == false 
  ```

6. 原型
  * 每个函数都有 prototype 属性，该属性指向原型
  * 每个对象都有 __proto__ 属性，该属性指向其构造函数的原型
  * __proto__ 将对象连接起来组成了原型链

7. new 
  * 在调用 new 的过程中会发生以下四件事情：
    1. 新生成一个对象
    2. 链接到原型
    3. 绑定 this
    4. 返回新对象
    ```js
    //实现一个 new
    function create() {
      //创建一个空对象
      let obj = new Object();
      // 获得构造函数
      let Con = [].shift.call(arguments)  //相当于 arguments.shift()
      //链接到原型
      obj.__proto__ = Con.prototype
      //绑定 this, 执行构造函数
      let result = Con.apply(obj, arguments);
      //确保 new 出来的是一个新对象
      return typeof result == 'objct' ? result : obj
    }
    ```
    ```js
    //实现一个 instanceof 
    function instanceof (left, right) {
      let prototype = right.prototype
      let left = left.__proto__
      while(true) {
        if (left == null) {
          return false
        }
        if (left == prototype) {
          return true
        }
        left = left.__proto__
      }
    }

    ``` 

8. this
  * this 的四种指向:
    1. 被当作函数调用，指向window
    2. 被当作方法调用，指向当前所属对象
    3. 被 new 调用，指向构造函数的实例对象
    4. 被 call / apply / bind 调用，指向第一个参数

  * 箭头函数没有 this , 箭头函数的 this 取决于他的外面的第一个不是箭头函数的函数的 this

9. 作用域链：包含自身变量对象和上级变量对象的列表 (当查找变量的时候，会先在当前上下文的变量对象中查找，如果没找到，就会从父级执行上下文的变量对象中查找，一直找到全局上下文的变量对象)

10. 函数和变量提升
  * 在函数和变量提升的过程中，相同的函数会覆盖上一个函数，并且函数优先于变量提升

11. 闭包
  * 函数 A 返回了一个函数 B ,并且函数 B 中使用了函数 A 的变量，函数 B 就被称为闭包
  ```js
  for (var i = 1; i <= 5; i++) {
    setTimeout(function timer() {
      console.log(i)
    }, i * 1000)
  }  //结果为：每隔一秒，打印出一个6

  //解决办法: 使用闭包
  for (var i = 0; i <= 5; i++) {
    (function(j) {
      setTimeout(function timer() {
        console.log(j)
      }, j * 1000)
    })(i)
  }
  ```

12. 模块化
  * 模块化的好处：
    * 解决命名冲突
    * 提高复用性
    * 提高代码可维护性
  * CommondJS: require / module.exports / exports.name = xxx ; 应用于服务器端，同步导入
    + 导出时是值拷贝，导出值的改变不影响导入值
  * ES6: import xxx from ... / export /export default; 应用于客户端，异步导入
    + 当导出使用的是 export default xxx 时，导入时 import 后 不使用大括号
    + 实时绑定，导入导出的值都指向同一个内存地址

13. 防抖与节流
  * 防抖与节流的作用都是防止函数多次调用
  * 防抖：
    ```js
    //简易版
    function debounce(fn, wait) {
      let timer;
      return function(...args) {
        if (timer) {
          clearTimeout(timer);
        }
        timer = setTimeout(() => {
          fn.apply(this, args)
        }, wait);
      }
    }
    ```
    ```js
    //第一次立即执行版
    function debounce2(fn, wait) {
      let timer;
      return function(...args) {
        if (!timer) {
          timer = setTimeout(() => {timer = null}, wait);
          fn.apply(this, args);
        } else {
          clearTimeout(timer);
          timer = setTimeout(() => {
            fn.apply(this, args);
          }, wait)
        }
      }
    }
    ```

    * 节流
    ```js
      function throttle(fn, wait) {
        let timeout = false;
        return function(...args) {
          if (!timeout) {
            timeout = true;
            setTimeout(() => {
              timeout = false;
              fn.apply(this, args);
            }, wait)
          }
        }
      }
    ```

14. call 与 apply 与 bind
  * call 的实现
  ```js
    Function.prototype.myCall = function(context) {
      if (typeof this !== 'function') {
        throw new TypeError('error');
      }
      var context = context || window;
      context.fn = this;
      var arg = [...arguments].slice(1);
      var result = context.fn(...arg);
      delete context.fn;
      return result;
    }
  ```
  * apply 的实现
    ```js
    Function.prototype.myApply = function(context) {
      if (typeof this !== 'function') {
        throw new TypeError('error');
      }
      var context = context || window;
      context.fn = this;
      if (arguments[1]) {
        var result = context.fn(...arguments[1]);
      } else {
        var resutl = context.fn();
      }
      delete context.fn;
      return result;
    }
    ```
    * bind 的实现
    ```js
    Function.prototype.myBind = function(context) {
      if (typeof this !== 'function') {
        throw new TypeError('error');
      }
      var self = this;
      //注意：arguments 是一个类数组对象，不具备数组标准方法
      var args = [...arguments].slice(1);
      return function F() {
        //判断 F 是直接调用还是通过 new 调用
        if (this instanceof F) {
          return new self(...args, ...arguments)
        }
        return self.apply(context, args.concat(...arguments))
      }
    }
    ```

15. 实现柯里化函数
  ```js
  function curry(fn, args) {
    var length = fn.length;
    var args = args || [];
    return function() {
      var _args = args.concat([...arguments].slice());
      if (_args.length < length) {
        return curry.call(this, fn, _args);
      } else {
        return fn.apply(this, _args);
      }
    }
  }
  ```

16. Generator 函数实现
  ```js
  function generator(cb) {
    var obj = {
      next: 0,
      stop: function() {}
    }
    return {
      next: function() {
        if (cb(obj) == undefined) {
          return {
            value: undefined,
            done: true
          }
        } else {
          return {
            value: cd(obj),
            done: false
          }
        }
      }
    }
  }
  ```

17. flattenMap 实现
  ```js
  function flattenDeep(arr) {
    var res = [];
    for (var val of arr) {
      if (Array.isArray(val)) {
        res.push(...flattenDeep(val));
      } else {
        res.push(val);
      }
    }
    return res;
  }
  ```
18. sleep 函数实现
  ```js
    function sleep(wait) {
      return new Promise((resolve, reject) => {
        setTimeout(resolve, wait)
      })
    }
  ```

19. Promise 简易实现
  ```js
  function MyPromise(excutor) {
      const that = this;
      that.status = "pending"; //定义状态改变前的初始状态
      that.value = undefined;	//定义状态为resolved的时候的状态
      that.reason = undefined; //定义状态为rejected的时候的状态

    function resolve(value) {
      if (that.status === 'pending') {
        that.status = 'resolved';
        that.value = value;
      }
    }

    function reject(reason) {
      if (that.status === 'pending') {
        that.status = 'rejected';
        that.reason = reason;
      }
    }

    try {
      excutor(resolve, reject);
    } catch(e) {
      reject(e)
    }
  }

  MyPromise.prototype.then = function(onResolve, onReject) {
    let self = this;
    if (self.status === 'resolved') {
      setTimeout(() => {
        onResolve(self.value);
      }, 0)
    }
    if (self.status === 'rejected') {
      setTimeout(() => {
        onReject(self.reason);
      }, 0)
    }
  }
  ```

20. 垃圾回收机制
  * 内存泄漏(内存溢出): 一些代码始终没有被回收，导致内存占满溢出
  * 垃圾回收的两种方法: 标记清除、引用计数

21. 执行栈：存储函数调用的栈结构

## 浏览器部分

1. 事件机制
  * 事件触发三阶段：捕获阶段、目标阶段、冒泡阶段
  * addEventListener() 函数的第三个参数可以是布尔值，true 表示是捕获事件，false 表示是冒泡事件。默认为 false.
  * 可调用 event.stopPropagation() 阻止事件传播
  
  * 事件代理：在祖先元素上注册事件监听，事件的父元素处理事件的子元素；优点：节省内存；不需要给子节点注释事件

2. 跨域
  * 原因：浏览器出于安全考虑，有同源策略，防止CSRF攻击 (利用用户的登录状态发起恶意请求)
  * 形式：协议、域名或者端口有一个不同就是跨域
  * 解决跨域问题的常用方法：
    * JSONP: 利用 `<script>` 标签没有跨域限制的漏洞；只限于 get 请求
      ```js
      //jsonp 实现
      function jsonp(url, callback) {
        var script = document.createElement('script');
        var jsonpCbName = "jsonpcallback"+ Data.now();
        window[jsonpCbName] = callback;
        var newUrl;
        var index = url.indexOf('?');
        if (index >= 0) {
          newUrl = url + '&callback=' + jsonpCbName;  
        } else {
          nweUrl = url + "?callback=" + jsonpCbName;
        }
        script.onload = function(e) {
          document.body.removeChild(script);
          delete window[jsonpCbName];
        }
        script.onerror = function(e) {
          document.body.removeChild(script);
          delete window[jsonpCbName];
        }
        script.url = newUrl
        document.body.appendChild(script)
      }
      ```
    * CORS: 在服务器端设置往响应头中加入 `Access-Control-Allow-Origin` ，就可以开启CORS，表示哪些域名可以访问资源
    * 第三方服务器代理：跨域限制只在浏览器端存在，在服务器端不存在

3. Event Loop (事件循环)
  * JS 是非阻塞单线程语言；(如果是多线程，处理DOM就可能发生问题，比如一个线程增加节点，一个线程删除节点)
  * (**JS在执行的过程中会产生执行环境，这些执行环境会被顺序的加入到执行栈中。如果遇到异步的代码，会被挂起并加入到 Task（有多种task)队列中。一旦执行栈为空， Event Loop 就会从 Task 队列中拿出需要执行的代码并放入到执行栈中执行，若依本质上来说 JS 中的异步还是同步行为**)
  * 浏览器事件循环机制：区分宏任务和微任务
  * Node 事件循环机制：
    1. timer 阶段：setTimeout / setInterval
    2. poll 阶段：I/O
    3. check 阶段：setImmediate
    4. close 阶段：执行close事件的回调

4. 存储
  * session 与 cookie
    * 参照(https://blog.csdn.net/duan1078774504/article/details/51912868)
      * 浏览器初次请求时，服务器生成一个 Session(内存存一个Map) 和 一个 SessionID 用来标识这个 Session(回话)，并通过响应(在响应头中设置set-Cookie)将 SessionID 发送到浏览器
      * 浏览器保存Cookie，并在以后的请求中携带cookie字段(值为SessionID)
      * 服务器从请求中提取出 SessionID, 读SessionID对应的Map, 执行逻辑
    * cookie: 在客户端保持状态的方案； session: 在服务器端保持状态的方案；
  * cookie / localStorage / sessionStorage
  * service worker: 做缓存文件，提高首屏速度

5. 渲染机制
  * 解析 HTML 生成 DOM 树；根据 CSS 生成CSSOM树；合并成渲染树；根据渲染树布局；绘制渲染树
  * 图层：一般来说，可以将普通文档流看成一个图层。特定的属性可以生成一个新的图层。不同的图层渲染互不影响。
  * 可以生成新图层的属性：translate3d / translateZ / iframe / position: fixed
  * 减少重绘和回流 (重绘与回流实际上与 Event Loop 有关)
    * 使用 transform / opacity 等动画效果不会引起回流重绘
    * 使用 visibility 替换 display: none;
    * 避免使用 table 布局


##安全部分
1. XSS (跨站脚本攻击): 用户将恶意代码注入到网页中, 通过修改 HTML 或者执行 JS 代码来攻击网站
  * 防御：转义输入输出的内容

2. CSRF (跨站请求伪造): 利用用户的登录状态发起恶意请求
  * 防御:
    * 验证 Referer: 通过验证 Referer 来判断该请求是否为第三方网站发起的
    * 添加 Token: 服务器下发一个随机 Token ，每次发起请求时将 Token 携带上，服务器验证 Token 是否有效 

3. CSP (内容安全策略): 用于检测并削弱某些特定类型的攻击；本质上是建立白名单，规定了浏览器只能执行特定来源的代码。


##性能部分

####网络相关
1. DNS 预解析
  * `<link rel="dns-prefetch" href="//example.com" />` ；通过预解析的方式来预先获得域名所对应的IP

2. 缓存
  * 强缓存：在响应头中加入 Expires(缓存有效时间) / Cache-Control (缓存会在多长时间后过期)
  * 协商缓存：Last-Modified (本地文件最后修改日期)
  * 缓存策略： 
    * 不需要缓存的资源：Cache-Control: no-store
    * 频繁变动的资源：Cache-Control: no-cache 并配合 ETag 使用，表示该资源已被缓存，但是每次都会发送请求询问资源是否更新

3. 使用 HTTP / 2.0
  * HTTP 2.0 可以多路复用, HTTP 头部压缩

4. 预加载

#### 渲染过程优化
1. 懒加载：将不关键的资源延后加载；即只加载自定义区域(通常是可视区域)内需要加载的东西；
    对于图片来说，先设置图片标签的 src 属性为一张占位图，将真实的图片资源放到一个自定义属性中，当进入自定义区域时，就将自定义属性替换为 src 属性。
2. 避免使用CSS表达式

#### 文件优化
1. CSS文件放在 head 中
2. JS 文件放在底部
3. 文件压缩
4. CDN: 静态资源尽量使用CDN加载

```html
<!-- 如何渲染几万条数据并不卡住页面? -->
<!-- 考察点：不能一次性将几万条都渲染出来，而应该一次渲染部分DOM, 那么就可以通过 requestAnimationFrame 来每 16ms 刷新一次 -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <ul>
      控件
    </ul>
    <script>
      setTimeout(() => {
        // 插入十万条数据
        const total = 100000
        // 一次插入 20 条，如果觉得性能不好就减少
        const once = 20
        // 渲染数据总共需要几次
        const loopCount = total / once
        let countOfRender = 0
        let ul = document.querySelector('ul')
        function add() {
          // 优化性能，插入不会造成回流
          const fragment = document.createDocumentFragment()
          for (let i = 0; i < once; i++) {
            const li = document.createElement('li')
            li.innerText = Math.floor(Math.random() * total)
            fragment.appendChild(li)
          }
          ul.appendChild(fragment)
          countOfRender += 1
          loop()
        }
        function loop() {
          if (countOfRender < loopCount) {
            window.requestAnimationFrame(add)
          }
        }
        loop()
      }, 0)
    </script>
  </body>
</html>
```

## 框架部分
1. MVVM
  * Model: 数据模型  View: 界面  ViewModel: 作为桥梁负责沟通 View 和 Model
2. 路由
  * hash: 带有 # 号 / history: 不带 # 号
3. 服务端渲染：服务器端解析HTML生成 DOM 树，转成字符串，输出给浏览器

