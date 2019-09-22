## 1.相对比较基础篇

### 1.布局

- 假设高度已知，写出 3 栏布局。其中左栏和右栏宽度已知(300px)，中间自适应

  > `A B C` 答出来及格, `D E` 答出来优秀
  > 延伸：高度不确定的情况下呢？`浮动 定位 网格布局就不能使用了，但是 flex 和 table 可以实现高度自适应`

  - A. 浮动解决方案
    > 优点 兼容性好
    > 缺点 要处理好清除浮动的问题
    > 延伸 处理浮动的几种方法?`1.给前面一个父元素设置高度 2.结尾处加空div标签 clear:both 3.父级div定义 overflow:hidden`
  - B. 绝对定位解决方案
    > 优点 方便
    > 缺点 脱离文档流导致它下面的也脱离文档流 可用性差
    > 延伸: z-index: -1; 表示什么意思?`让该层的层级低于block块状水平盒子的层级`
  - C. flex 解决方案
    > 为了解决浮动和定位布局的不足而出现的 移动端使用的较多
    > 延伸： 让一个元素水平 垂直居中 使用 flex 如何实现? `align-items:center;justify-content:conter;`
  - D. 表格布局解决方案

    ```css
    <style>
    .box {
      width: 100%;
      height: 100px;
      display: table;
    }
    .box > div {
      display: table-cell;
    }
    .left {
      width: 300px;
    }
    .center {
      /* 中间盒子不设置宽度即可 */
    }
    .right {
      width: 300px;
    }
    </style>
    ```

  - E. 网格布局解决方案

    > css 新属性， 支持栅格化的标准。不用再模拟网格
    > 网格布局可以做很多负责的业务 但是代码量很少 是一种新的技术

    ```css
    <style>
    .box {
      display:grid;
      width:100%;
      grid-template-rows : 100px; /* 网格行高度 */
      grid-template-columns : 300px auto 300px; /* 中间盒子不设置宽度即可 类似margin */
    }
    .left {}
    .center {}
    .right {}
    </style>
    ```

### 2.盒模型

- 1. css 盒模型有几种？它们之间的区别？它们分别如何设置？

  - 几种
    > `标准模型+IE模型`
  - 区别
    > 宽高的计算方式不同
    > `标准模型`的宽高指的是 content 的宽高 不计算 padding、border、margin
    > `IE 模型`的宽高是计算 boder、padding 的
  - 如何设置
    > 标准模型: `box-sizing:content-box <浏览器默认>`
    > IE 模型: `box-sizing:border-box`

- 2. js 如何设置获取盒模型对应的宽和高? `答出来3个以上优秀`
     > 1. `dom.style.width/height` (通过 dom 元素获取宽高) <缺陷：只能取内联样式的宽高>
     > 2. `dom.currentStyle.width/height` (拿到 dom 渲染以后的宽高) <缺陷：只有 IE 支持>
     > 3. `window.getComputedStyle(dom).width/height` (兼容性更好一些) <兼容 firefox 和 chrom 通用性更好一些>
     > 4. `dom.getBoundingClientRect().width/heigh`t (根据浏览器左上角拿到 dom 元素的定 位和宽高： top、left、width、height)

- 3. 什么是 `BFC` (边距重叠解决方案)？ `答出来优秀`
  - 基本概念
    > 块级格式化上下文
    > W3C 对 BFC 的定义如下：
    > `浮动元素和绝对定位元素，非块级盒子的块级容器（例如 inline-blocks, table-cells, 和 table-captions），以及overflow值不为“visiable”的块级盒子，都会为 他们的内容创建新的BFC（块级格式上下文）。`
  - `BFC` 原理
    > 1. 在 `BFC` 这个元素的垂直方向的边距会发生重叠
    > 2. `BFC` 的区域不会与浮动元素区域重叠
    > 3. `BFC` 在页面上是一个独立的容器，外面的元素不会影响里面的元素 反之亦然
    > 4. 计算 `BFC` 高度的时候 浮动元素也会参与计算
  - 如何创建 `BFC`
    > 1. float 值不为 none `<浮动>`
    > 2. position 值不为 static 或 relative `<定位>`
    > 3. display 属性为 table 相关 `<table>`
    > 4. overflow 值不为 visible`<overflow>`

### 3.DOM 事件相关

- 1. 事件级别有几级？分别是哪几种？

  - 3 级 它们分别是： `答出来优秀`

    > `DOM0级`: `element.onclick=function(){}`

    > `DOM2级`: `element.addEventListener('click',function(){},false)` false 指定冒泡还是捕获

    > `DOM3级`: `element.addEventListener('keyup',function(){},false)` 3 级增加了事件类型， 鼠标 事件 键盘事件等等

    > 延伸: 为什么没有 DOM1 级? `DOM1 不涉及事件相关`

- 2. 简述 DOM 事件模型？ 简述事件流？ 和 DOM 事件捕获的具体流程?

  - DOM 事件模型指的是捕获和冒泡

    > 捕获：从上到下
    > 冒泡：从下到上

  - 事件流指的是用户触发事件的流程 分为 3 个阶段 分别是

    > 1.  捕获
    > 2.  目标阶段
    > 3.  冒泡

  - DOM 事件捕获的具体流程 `答出来优秀`
    > `1.window → 2.document → 3.html<document.documentElement> → 4.body<document.body> → ... → 5.目标元素`

- 3. Event 对象的相关问题？ `全答出来优秀`

  - 1. 阻止默认事件 `event.preventDefault()` <通知浏览器不要执行与事件关联的默认动作，如果 type 属性是 "submit",通过调用该方法，可以阻止提交表单>
  - 2. 阻止冒泡行为 `event.stopPropagation()` <解决：点击子元素 父元素会响应事件的问题>
  - 3. 事件响应优先级 `event.stopLmmediatePropagation()` <按钮绑定了两个 click 事件 A 事件中添加这个就会阻止 B 事件的执行>
  - 4. 当前所绑定的事件 `event.currentTarget` <注册事件监听器对象>
  - 5. 当前被点击的元素 `event.target` <获取目标元素>

- 4. 如何自定义事件? `回答出来技术面通过`

  - 1. Event() `构造函数, 创建一个新的事件对象 Event`

    ```js
    var eve = new Event("custome"); //声明一个自定义事件
    dom.addEventListener("custome", function() {
      // 监听该自定义事件
      console.log("custome!");
    });
    dom.dispatchEvent(eve); // 触发事件
    ```

  - 2. CustomEvent() `构造函数, 创建一个新的事件对象并传送自定义数据`

    ```js
    var eve = new CustomEvent("custome", {
      //声明一个自定义事件
      detail: { title: "This is title!" }
    });
    dom.addEventListener("custome", function() {
      // 监听该自定义事件
      console.log("CustomEvent的数据为：", event.detail.title);
    });
    dom.dispatchEvent(eve); // 触发事件
    ```

### 4.HTTP 协议

- 1. 简述 HTTP 协议的特点？ `答出来2以上及格`
     > 1. 无连接: 连接一次就会断掉 不会保持连接
     > 2. 无状态：服务端无法确定每次连接人的身份状态
     > 3. 简单快速：uri 是固定的 想要某个资源 只需要输入 uri 就行了
     > 4. 灵活：通过一个 http 协议的 ContentType 可以完成不同数据类型的存储

- 2. 简述 HTTP 报文的组成部分? 请分别对其进行简述

  - 分为`请求报文`和`响应报文`
    - 1. 请求报文
         > 请求行 ：由 `请求方法` `页面地址`以及 `HTTP 协议版本` 3 个字段组成 `GET /data/info.html HTTP/1.1`
         > 请求头 ：key/value 值
         > 空行：作用是通过一个空行，告诉服务器请求头部到此为止，空行下一个就是请求体。
         > 请求体：请求数据
    - 2. 响应报文
         > 响应行 ：由 `请求方法` `页面地址`以及 `HTTP 协议版本` 3 个字段组成 `GET /data/info.html HTTP/1.1`
         > 响应头 ：key/value 值
         > 空行：作用是通过一个空行，告诉服务器响应头部到此为止，空行下一个就是响应体。
         > 响应体：返回数据

- 3. POST 和 GET 的区别有哪些？`答出3个以上及格`
     > 1. GET 在浏览器回退时是无害的，而 POST 会再次提交请求 `*`
     > 2. GET 产生的 URL 地址可以被收藏，而 POST 不可以
     > 3. GET 请求会被浏览器主动缓存，而 POST 不会，除非手动设置 `*`
     > 4. GET 请求只能进行 url 编码 而 POST 支持多种编码方式
     > 5. GET 请求参数会被完整保留在浏览器历史记录里，而 POST 中的参数不会被保留 `*`
     > 6. GET 请求在 URL 中传送的参数是有长度限制的，而 POST 没有限制 `url 参数是拼接的 是有浏览器默认限制的` `*`
     > 7. 对参数的数据类型，GET 只接受 ASCLL 字符，而 POST 没有限制
     > 8. GET 比 POST 更不安全，因为参数直接暴露在 URL 上，所以不能用来传递敏感信息
     > 9. GET 参数通过 URL 传递，POST 放在 Request body 中 `*`

- 4.  举例说出常见的状态码？

  - 常见的状态码

    > 200 OK: `客户端请求成功`
    > 206 Partial Content: `客户发送了一个带有 Range<范围>头的 GET 请求，服务器完成了它 <请求视频或者音频文件很大的时候 服务器就会给你返回部分[Range]的文件>`
    > 301 Moved Permanently: `所请求的页面已经转移至新的 url <永久重定向>`
    > 302 Found: `所请求的页面已经临时转移至新的 url <临时重定向>`
    > 304 Not Modified: `客户端有缓冲的文档并发出了一个条件性的请求，服务器告诉客户端，原来缓冲的文档还可以继续使用`
    > 400 Bad Request: `客户端请求语法错误，不能被服务器所理解`
    > 401 Unauthorized: `请求未经授权，这个状态码必须和 WWW-Authenticate 抱头域一起使用`
    > 403 Forbidden: `对被请求页面的访问被禁止`
    > 404 Not Found: `请求资源不存在`
    > 500 Internal Server Error: `服务器发生不可预期的错误原来缓冲的文档还可以继续使用`
    > 503 Server Unavailable: `请求未完成，服务器临时过载或宕机，一段时间后可能恢复正常`

  - 一般性规律
    > 1xx:`指示信息 - 表示请求已接受，继续处理`
    > 2xx:`成功 - 表示请求已被成功接收`
    > 3xx:`重定向 - 要完成请求必须进行更进一步的操作`
    > 4xx:`客户端错误 - 请求有语法错误或请求无法实现`
    > 5xx:`服务器错误 - 服务器未能实现合法的请求`

### 5.面向对象

- 1.  请简述类的声明与继承？

      - 1. 类的声明 <有两种方式>

        ```js
        /*A：传统构造函数声明*/
        function Animal() {
          this.name = "name";
        }
        /*B：ES6中的class声明*/
        class Animal2 {
          constructor() {
            this.name = "name";
          }
        }
        ```

        > 延伸：类的实例有几种方式？ `1种。 类的声明方式不同 但是实例化方式相同 都是通过new`

      - 2. 类的继承 （ES5）

        - 1.  借助构造函数实现继承

              ```js
              function Parent1() {
                this.name = "parent1";
              }
              Parent1.prototype.say = function() {
                console.log("sayyyyyyyyy!");
              };
              function Child1() {
                // <将父级构造函数的 this 指向子级构造函数的实例上>
                Parent1.call(this); // \* call 改变上下文 (this)
                this.type = "child1";
              }
              console.log(new Parent1(), new Child1());
              ```

              > 原理：`子类继承父类 通过 call/apply 改变父类构造函数的 this 指向`
              > 缺点：`部分继承 只继承了父类的属性 并没有继承父类的原型对象上< 原型链上>的方法`

          - 2.  第二种：借助原型链实现继承

                ```js
                function Parent2() {
                  this.name = "Parent2";
                  this.arr = [1, 2, 3];
                }
                Parent2.prototype.dance = function() {
                  console.log("dancing!!!");
                };

                function Child2() {
                  this.type = "Child2";
                }
                Child2.prototype = new Parent2(); //new Child2().__proto__ === new Parent2()
                /* 如果要在Child2实例上找dance方法的话 流程是这样：
                    1. 在当前实例 new Child2() 上查找dance方法
                    2. 发现没有 就会去 new Child2().__proto__ 上面去找dance方法
                    3. new Child2(). __proto__ === new Parent2().__proto__
                    4. 去new Parent2()上查找dance方法 发现没有
                    5. 去new Parent2().__proto__ 查找dance方法
                    6. ok找到。*/
                ```

                > 原理：通过继承`__proto__`实现原型链继承
                > 缺点：如果实例两个对象 改变其中一个对象的属性/方法 另一个对象也会跟着改变 <因为改变的是原型链>

### 6.通信相关

- 1.  什么是同源策略及限制？

      - 1. 同源策略
           > 同源策略限制从一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这是一个用于隔离潜在的恶意文件的关键的安全机制
           > 延伸 1: 什么是源？ `源： 协议 域名 端口`
           > 延伸 2: 什么时候算跨域？ `3个中有1个不一样就不是同一个源 就是跨域了`

      - 2. 限制 `不是一个源的文档不能去操作另一个源的文档`

           - 1. Cookie、LocalStorage 和 indexDB 无法获取

           - 2. DOM 无法获得

           - 3. AJAX 请求不能发送 `ajax只适合同源的通信 跨域了就不能用`
                - 延伸: 前后端通信只能使用 ajax 吗? 除此之外还有什么？
                  > Ajax `仅是同源下的通信方式`
                  > WebSocket `不受同源策略的限制`
                  > CORS `支持跨域通信 也支持同源通信`

- 2. 跨域通信的有几种方式？

  - 1. JSONP `script标签的异步加载来实现`
       > 延伸 除了`<script />`的 src 不受同源策略限制之外，还有什么标签也同源策略限制？`1.<link href='' /> 2.<img src='' />`

  - 2. Hash `hash 改变页面不刷新`

  - 3. postMessage `H5 新增`

  - 4. WebSocket `不受同源策略限制`
       > 延伸 WebSocket 协议和 Http 协议的区别？`答出来加分`
       > Http 协议： `每次都需要客户端定时轮询向服务器请求 ，然后服务器再向客户端发送数据`
       > WebSocket 协议： `允许服务端主动向客户端推送数据，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。`

  - 5. CORS `可以理解为:支持跨域通信的 ajax`
       > 延伸 什么是 CORS? `跨源资源共享 是W3C出的一个标准，其思想是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功，还是应该失败。因此，要想实现CORS进行跨域，需要服务器进行一些设置，同时前端也需要做一些配置和分析。`

#### ✨✨✨✨

- 1. 在 React 的父组件中 如何在父组件触发子组件中定义的函数 并在父组件中获取该函数的返回值？

> 1. 父组件传递标示和回调函数，子组件中通过 ComponentWillReceiveProps 钩子函数来监听变化时，标示变化时， 触发子组件中的该函数，并通过 props 回调函数的参数 将其值返回

> 2. 通过高阶函数将其父组件进行包裹，对其父组件进行功能增强，把子组件中的相关函数 通过 props 传递给父组件

> 延伸： 高阶组件是什么？和普通组件有什么区别？高阶函数又是什么？小程序支持高阶组件吗？怎么解决？
