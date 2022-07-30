---
title: JS基础知识点
date: 2022-07-30T22:46:00.000+00:00
lang: zh
type: interview
duration: 25min
---

### 前言
---
自己整理出来的一些前端工程师必备的面试题，面试中出场率很高，相信小伙伴在找工作或者跳槽中能够顺利找到自己想要的工作。

![avatar](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/1/11/16f9528d13891dc3~tplv-t2oaga2asx-image.image)

### Html

#### 1、DOCTYPE有什么作用？标准模式与混杂模式如何区分？它们有何意义？
DOCTYPE的作用是告诉浏览器使用哪种版本的HTML规范来渲染文档。当DOCTYPE不存在或者形式不正确时会导致HTML文档以混杂模式解析文档。标准模式以浏览器支持的最高标准运行，混杂模式中的页面会以一种比较宽松的向后兼容的方式显示。

H5不基于SGML因此不需要对DTD进行引用，但是需要通过DOCTYPE来规范浏览器行为。HTML4.0基于SGML，所以需要引用DTD才能告知浏览器文档所使用的文档类型。

#### 2、页面导入样式时，使用link和@import的区别？
1、Link属于HTML标签，除了能加载CSS样式外还可以加载RSS，而@import是CSS提供，只能用于加载CSS。

2、页面被加载时，使用link标签的加载的CSS样式会同时被加载，而＠import引用的CSS会等到页面被加载完再加载。

3、＠import是CSS２.１提供的，只有再IE５及以上才能被识别，而link是HTML标签，无兼容问题。

#### 3、H5有哪些新特性？
1、语义化更好的标签如section、article、header、nav、footer main aside

2、多媒体元素audio和video

3、绘图功能canvas和SVG两种绘图方法

4、地理位置信息、移动端事件处理

5、客户端存储localStorage 和sectionStorage

6、动画优化的requestAnimationFrame

7、WebWorker开辟一个独立同时运行的线程，减少渲染时间

8、WebSocket请求，类似于HTTP协议，用于跨域通信

#### 4、HTML5中的localStorage和sectionStorge 与cookie的异同？
相同：都是保存在浏览器客户端，并且都是同源的。

不同：
1、存储量：LocalStorage 和sessionStorage:存储数据可达5M而cookie存储数据很小一般为4k
Cookie会随着HTTP请求一同发给用户，即cookie在浏览器和服务器之间来回传递，而localStorage和sessionStorage不会把数据发送给服务器，仅在本地保存

2、数据的有效期不同：cookie在设置的cookie过期时间内一直有效。而localStorage始终有效，即使窗口或者浏览器关闭数据还是长久保存。SessionStorage仅在浏览器窗口关闭之前有效，一旦关闭就丢失了。

3、作用域不同：cookie在所有的同源窗口都是共享的。LocalStorage在所有的同源窗口都是共享的，sessionStorage不在不同的浏览器共享，即使同一页面。

#### 5、行级元素与块级元素的区别？
行级元素不会独占一行，无法设置其width和height，高度由其内容区撑开，行级元素内只能是行级元素而不能是块级元素。Padding可以设置，但是margin-top和margin-bottom设置无效。

块级元素独占一行，可以设置width和height，高度由内部元素撑开，块级元素内可以是块级元素也可以是行级元素。可以设置margin和padding。

#### 6、Px与 em和 rem的区别？
Px是绝对像素值，大部分元素可以设置，如果没有设置会继承父元素的像素值

Em是相对单位。它是以当前元素的父级元素的基值来设置的

rem是相对单位。它是根据HTML元素的像素基值为1rem，常用于解决PC端与移动端的自适应。

#### 7、简述src 与href的区别？
src用于替换当前元素。而href则是建立当前元素与外界资源的联系。

src指向外部资源，指向的内容将会下载并嵌入到当前标签的位置，在请求src资源时会将其下载并应用到文档内。例如iframe img script标签等。

当浏览器解析到该元素时，会暂停其他资源的下载和处理，直到将资源加载、编译、执行完毕才会去处理其他元素。这也是为什么要将script放于底部的原因。

href标签指向网络资源的所在位置，建立当前元素和外部资源的链接。并且下载该资源时浏览器会并行下载并且不会停止对当前文档的处理。所以推荐css实例link方式加载。

### CSS
#### 1、介绍一下标准的CSS盒模型？这与低版本的IE盒模型有什么区别？
标准盒模型：宽度=内容区宽度（content）+padding+border

IE怪异盒模型：宽度=内容宽度(width+padding+border)

#### 2、CSS如何设置这两种盒模型？
通过box-sizing这个属性来设置

box-sizing：content-box 为标准盒模型

box-sizing：border-box  为IE怪异盒模型

#### 3、JS如何获取盒模型对应的宽和高？
Dom.style.width/height &nbsp; &nbsp; &nbsp;  直接获取元素的宽高（只能获取内联样式）

Dom.currentStyle.width/height &nbsp; &nbsp; &nbsp;  IE独有的获取宽高样式

Window.getComputedStyle(dom).width/heihgt &nbsp; &nbsp; &nbsp;  兼容的获取宽高样式的方法

Dom.getBoundingClientRect().width/height &nbsp; &nbsp; &nbsp;           //计算元素相对于viewport的位置有left top right bottom x y width height8个值

#### 4、BFC的原理？（BFC的渲染规则）
1、在BFC元素的垂直边距上会发生重叠

2、BFC的区域不会与浮动元素的box重叠

3、BFC在页面上是一个独立的容器，外面的元素不会影响到里面的元素，里面的元素也不会影响外面的元素。

4、计算BFC高度时浮动元素也会参与计算。

5、BFC可以阻止元素被浮动元素覆盖。

#### 5、如何创建BFC？
float值不为none

overflow不为visible

position的值不是static或者relative

display为inline-block或者table-cell或者flex

#### 6、BFC的常见使用场景？
1、解决边距重叠问题

2、BFC不与float元素重叠

3、清除浮动（父级元素会计算浮动元素的高度）

#### 7、CSS三栏布局的实现？
１、使用float浮动元素要位于非浮动元素的上面

２、使用绝对定位的方式来实现

３、使用flex弹性盒模型来实现

４、使用table布局来实现　父元素设置为table布局子元素布局设置为table-cell即可

５、使用grid布局，使用grid-template-rows来设置高度使用grid-template-columns来设置列数

６、使用inline-block配合calc来实现三栏布局

7、双飞翼布局

8、圣杯布局

#### 8、你对line-height是如何理解的？
行高是指一行文字的高度，具体是说两行文字之间基线的距离。CSS中起作用的height和line-height，没有定义height属性，最终其表现作用一定是line-height

单行文本垂直居中：只需要设置height和line-height相等即可

多行文本垂直居中：不知道宽高的可以使用padding上下一致即可

1、父元素设置display：table，子元素设置display：tabel-cell，vertical-align:middle

2、父元素设置height和line-height相等，子元素设置为display：inline-block，vertical-align:middle，line-height：16px即可

3、父元素有高度设置相对定位，子元素将该元素设置绝对定位，使其垂直居中 top：50%； margin-top：-xxpx;

#### 9、怎么让chrome支持小于12px的文字？
P{font-size:10px; -webkit-transform: scale(0.8);}

#### 10、如何使得一个元素的高度随着它的宽度变化，且高度一直是宽度的一半？
1、元素设置width属性，然后使用JS动态设置其宽度（但是需要时刻监听元素的改变）

2、元素不设置高度，仅设置宽度然后padding:25% 0 此时需要设置父元素；

``注意：``: padding，margin值是相对于其父元素进行设置的。

#### 11、如何实现垂直水平居中？
大家可以参考这篇文章学习，写的比较详细，考虑也比较全面

[16种方法实现水平居中垂直居中](https://juejin.im/post/6844903474879004680)

#### 12、清除浮动的方法你知道几种？
1、父级元素增加空div通过clear：both

2、父级元素设置height

3、父级元素使用overflow：hidden／auto 触发BFC

4、让父级元素也浮动起来

5、将父级元素设置为display：table布局

#### 13、li元素元素直接的看不见的空白怎么消除？
1、将所以的li写在一行上

2、使用浮动

3、设置font-size： 0；

#### 14、display:inline-block 什么时候会显示间隙？
1、元素直接存在空格时，解决：消除空格

2、margin值存在时，解决：将margin值设置为负值

3、font-size不为0，解决：将font-size： 0；

#### 15、什么时候使用背景图什么时候使用img?
1、占位：img图片是占位的,它是html的一个标签，如果未设置大小则会按照图片大小进行展示

background-image是不占位的，它只是css的一个属性

2、否可操作
 background-image是只能看的，只能设置background-position, background-attachment, background-repeat, background-size等属性来操作图片

img是一个document对象，它是可以操作的。比如更换img src的路径可以达到更换图片的目的，也可以移动它的位置，从document中移除等等操作。所以如果是装饰性的图片就使用background-img，如果和文体内容很相关就使用img。

3、加载顺序不同
background-image 是在页面结构加载完成之后才加载的，而img是页面标签会在加载结构的时候加载，如果img图片很大那么加载时间很长后面的结构都会被阻塞，所以实际加载顺序上会加载img然后加载background-image

#### 16、flex布局
[一劳永逸的搞定 flex 布局](https://juejin.im/post/6844903474774147086)

#### 17、常用的预处理器语言mixin
预处理器语言大家可以自己根据喜好进行选择，通常常用的scss,sass,less, stylus等

下面是一些我开发过程中用到的scss的mixin，希望给大家参考
```
// position定位相关 
@mixin position($type: absolute,
    $left: null,
    $right: null,
    $top: null,
    $bottom: null) {
    position: $type;

    @if ($left !=null && $left !='') {
        left: $left;
    }

    @if ($right !=null && $right !='') {
        right: $right;
    }

    @if ($top !=null && $top !='') {
        top: $top;
    }

    @if ($bottom !=null && $bottom !='') {
        bottom: $bottom;
    }
}
// 单行隐藏
@mixin ellipsis($line) {
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: $line;
    overflow: hidden;
    text-overflow: ellipsis;
}
// 多行隐藏
@mixin ellipsis-one {
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
}


/*弹性盒子居中（传入null不设置该属性）*/
@mixin flex-center($direction:row, $justify:center,  $align:center, $flex-wrap: null) {
  display: flex;
  @if ($direction != null) {
    flex-direction: $direction;
  }
  @if ($justify != null) {
    justify-content: $justify;
  }
  @if ($align != null) {
    align-items: $align;
  }
  @if ($flex-wrap != null) {
    flex-wrap: $flex-wrap;
  }

// 行高
@mixin line-height($height:30px, $line-height:30px) {
  @if ($height != null) {
    height: $height;
  }
  @if ($line-height != null) {
    line-height: $line-height;
  }
  // 清除浮动
  @mixin clearfix() {
    &:before,
    &:after {
        content: "";
        display: table;
    }
    &:after {
        clear: both;
    }
}

```

### JS

#### 1、let、const和var的区别？

var声明变量可以重复声明，而let不可以重复声明

var是不受限于块级的，声明会提升，而let是受限于块级，仅在该块级内起作用

var会与window相映射（会挂一个属性），而let不与window相映射

var可以在声明的上面访问变量，而let有暂存死区，在声明的上面访问变量会报错

const声明之后必须赋值，否则会报错

const定义不可变的量，改变了就会报错

const和let一样不会与window相映射、支持块级作用域、在声明的上面访问变量会报错

#### 2、说说你使用递归时为什么会出现内存溢出的情况？
递归非常消耗内存，因为需要同时保存很多的调用帧，因为栈可存放的函数是有限制的，一旦存放了过多的函数且没有得到释放的话，就会出现爆栈的问题。

解决：可以使用尾递归解决

#### 3、谈谈你认为的event loop？
event loop是JavaScript的事件执行机制，我们都知道JS是单线程的，而且异步任务的执行时间是晚于同步任务的，这就是因为存在event loop。

因为JS中存在宏任务和微任务，这两个分别维护一个队列，都是采用先进先出的策略执行的，并且宏任务的优先级高于微任务。

常见的宏任务有：script，setTimeout、setInterval、I/O、UI 交互事件、postMessage、MessageChannel、setImmediate(Node.js 环境)。

常见的微任务有：Promise.then、 MutationObserver、 process.nextTick(Node.js 环境)。


![任务执行图](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/1/12/16f97635adfe3cdf~tplv-t2oaga2asx-image.image)

任务执行顺序如下：
1、从宏任务的头部取出一个任务执行，先执行宏任务；

2、执行过程中若遇到微任务则将其添加到微任务的队列中（等执行栈空闲在执行）；

3、宏任务执行完毕后，微任务的队列中是否存在任务，若存在，则挨个儿出去执行，直到执行完毕；

4、如有必要会进行GUI 渲染；

这就是event loop的执行顺序。

#### 4、Xss 和CSRF的原理与预防？

1、$\color{#FF0000}{XSS}$

XSS，即 Cross Site Script，中译是跨站脚本攻击；其原本缩写是 CSS，但为了和层叠样式表(Cascading Style Sheet)有所区分，因而在安全领域叫做 XSS。

XSS 攻击是指攻击者在网站上注入恶意的可执行代码，通过恶意脚本对客户端网页进行篡改，从而在用户浏览网页时，控制用户或者获取用户隐私数据的一种攻击方式。

XSS主要分为3种（非持久型，持久型，基于DOM型）

1、非持久型主要是通过攻击者在网站中注入恶意代码，如URL中注入script脚本或者注入能够获取用户信息的脚本，从而达到攻击的目的。

通常这种攻击是一次性的，恶意脚本并不会被写入数据库中，所以相对来说危害不是很大。

2、持久型用户输入的恶意代码会被存储在服务器端，当浏览器请求数据时，脚本从服务器上传回并执行。这种 XSS攻击具有很强的稳定性，而且攻击到的是很多用户。

比较常见的一个场景是攻击者在社区或论坛上写下一篇包含恶意 JavaScript 代码的文章或评论，文章或评论发表后，所有访问该文章或评论的用户，都会在他们的浏览器中执行这段恶意的 JavaScript 代码。

3、基于DOM的则是发生在客户端，攻击者恶意修改dom结构而造成的攻击

##### 防御：1、对用户输入的任何东西进行字符转义，**永远不要相信用户的输入**  &nbsp; &nbsp;2、使用httpOnly防止cookie被劫持

2、$\color{#FF0000}{CSRF}$

CSRF 攻击是攻击者借助受害者的Cookie骗取服务器的信任，可以在受害者毫不知情的情况下以受害者名义伪造请求发送给受攻击服务器，从而在并未授权的情况下执行在权限保护之下的操作。

***满足条件：用户已经登陆， 网站存在安全漏洞***

##### 防御：1、使用token进行验证，每次发送请求是都要验证token值是否正确，这样可以防御恶意攻击者的请求攻击。2、Referer验证，通过检查referee来确定请求是否是合法的源。 3、设置SameSite,可以对 Cookie 设置 SameSite 属性。该属性表示 Cookie不随着跨域请求发送，可以很大程度减少 CSRF 的攻击，但是该属性目前并不是所有浏览器都兼容

#### 5、前后端如何通信？
1、Ajax 同源下的通信方式		

2、websocket不受同源策略的限制 

2、CORS支持跨域通信也支持同源通信

#### 6、跨域通信的几种方法？
1、JSONP 

原理：利用script标签的异步加载。前后端约定callback函数名，后端返回一个函数名包含的数据对象。需要后端的配合。&nbsp;JSONP 使用简单且兼容性不错，但是只限于 get 请求。

2、document.domain

方式只能用于二级域名相同的情况下，比如 x.test.com 和 y.test.com 适用于该方式。&nbsp;&nbsp;&nbsp;只需要给页面添加 document.domain = 'test.com' 表示二级域名都相同就可以实现跨域

3、iframe和location.hash

原理：hash改变页面不刷新,可以通过改变hash值来执行回调操作从而实现数据的传递

4、postMessage

这种方式通常用于获取嵌入页面中的第三方页面数据。一个页面发送消息，另一个页面判断来源并接收消息，通过postMessage方法和window.onmessage方法实现

5、Websocket 

原理：不受同源策略的限制

6、CORS

原理：后端服务器开启 Access-Control-Allow-Origin 即可以启动CORS，执行某个域下可以通信。

7、服务器代理

原理：通过服务器做第三方代理，来传递请求和接受返回的数据并进行传递过去。

#### 7、性能优化的方案有哪些？
借一张图片来看，大家如果想看的话也可以去看看这本小册[前端性能优化原理与实践](https://juejin.im/book/6844733750048210957/section/6844733750031417352)
![alt](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/10/23/1669f5358f63c0f8~tplv-t2oaga2asx-image.image)


#### 8、HTTP协议的主要特点？
简单快速：每个资源URI是固定的，在HTTP协议中处理比较简单

灵活：HTTP中头部分数据类型，通过一个HTTP协议可以完成不同数据类型的传输

无连接：连接依次就会断掉不会保持连接

无状态：客户端与服务端是两种身份，HTTP帮忙建立连接，依次任务完成就断开，下次传输还是需要确立连接

#### 9、HTTP报文的组成部分？
请求报文：请求行、请求头、空行、请求体

响应报文：状态行、响应头、空行、响应体

#### 10、POST和GET请求的区别？
GET在浏览器回退时是无害的，而POST会再次提交

GET产生的URL地址可以被收藏，而POST不可以

GET请求会被浏览器主动缓存，而POST不会除非主动设置

GET只能进行url编码，而POST支持多种编码

GET请求参数会被完整的保留在浏览器历史记录中而POST的参数不会

GET请求在URL传送的参数的长度有限，而POST没有限制

GET只能接受ASCII字符，而POST没有限制

GET比POST更不安全，因为参数直接暴露在URL上，所以不安全

GET参数通过URL传递，而POST放在Request body中

#### 11、常见的HTTP状态码？
1XX：指示信息--表示信息已被接受，继续处理

2XX：成功 --表示信息已经成功返回

    200 OK客户端请求成功
    206 Partial Content：客户端发送了一个带RangeGET请求，服务器响应完成
3XX： 重定向 --表示资源地址已经变更

    301 永久重定向 所有请求的页面已经转移之新的URL
    302 所请求的页面已经临时转移至新的URL
    304请求已经发出，但是为满足要求而不需要进行请求即可完成
4XX：请求错误--表示客户端请求出错
    
    401 用户未登录或者为授权
    403 Forbidden 对请求的页面的访问被禁止
    404 Not Found 请求的资源不存在
5XX：服务器端出错 --表示服务器正在维修或者已坏

    500：服务器发生不可预期的错误原来缓冲的文档还可以继续使用
    503：请求未完成，服务器临时过载或当机，一段时间内可恢复正常
#### 12、http的缓存机制是怎么样的？
HTTP 缓存是我们日常开发中最为熟悉的一种缓存机制。它又分为强缓存和协商缓存。优先级较高的是强缓存，在命中强缓存失败的情况下，才会走协商缓存。

$\color{#0ff}{强缓存}$ 

返回状态码是200 from xxxx

1、expires，这个的实现机制是expires是一个时间戳，接下来如果我们试图再次向服务器请求资源，浏览器就会先对比本地时间和 expires 的时间戳，如果本地时间小于 expires 设定的过期时间，那么就直接去缓存中取这个资源。

那么问题就很明显了，这个非常依赖本地时间，如果用户修改了本地时间那么缓存机制也就失效了。

2、cache-control,在 Cache-Control 中，我们通常通过 max-age 来控制资源的有效期。max-age 不是一个时间戳，而是一个时间长度，如max-age=33433000;这样的话只有这个请求在这个时间段内都是有效的。并且cache-control优先级也更高，也向下兼容。


$\color{#0ff}{协商缓存}$

返回的状态码是304 Not Modified

1、Last-Modified与If-Modified-Since

工作：服务器接收到请求过来的If-Modified-Since时会对比这个时间与服务器文件修改的最后时间，如果发生变化则会返回一个完整的响应，否则会返回304，并且响应头中不会有Last-Modified字段。

但是也存在缺陷：

    1、我们如果对资源进行了编辑，但是内容并未修改，而修改时间却改变了，那么下一次请求时需要重新响应，但是返回的数据却并未改变。

    2、如果我们修改文件的时间很短（s以内），由于 If-Modified-Since 只能检查到以秒为最小计量单位的时间差，那么这个他的Last-modified并未修改，导致请求的资源反而出错。
2、Etag 是由服务器为每个资源生成的唯一的标识符，这个标识符是基于文件内容生成的，只要文件内容不同，它们对应的 Etag 就是不同的。

因此每次请求只需要对比Etag是否改变即可。Etag的优先级更高，并且两者同时存在是优先使用Etag。

    
#### 13、谈谈你对原型链的理解？
每一个对象中都存在__proto__属性，__proto__属性里面又存在constructor函数，函数内部又有prototype，并且这个值和__proto__属性一样。所以有这样的关系：$\color{#ff0000}{原型的 constructor 属性指向构造函数，构造函数又通过 prototype 属性指回原型}$

那么原型链是怎么回事呢？

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/1/12/16f97711a06d04ae~tplv-t2oaga2asx-image.image)

原型链就是多个对象通过__ __proto__ __ 的方式连接了起来。当当前元素中没有某一个函数或者方法时会沿着原型链一直往上查找，直到最顶部为止。

大家可以理解下这张图片
![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/1/12/16f97707325ae19e~tplv-t2oaga2asx-image.image)

#### 14、instanceOf的原理？
原理：本质上通过原型链来判断的


![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/1/12/16f9772af0bc8456~tplv-t2oaga2asx-image.image)

```
// instanceof 原理实现
function myInstanceof (left, right) {
    let prototype = right.prototype
    let left = left.__proto__
    while (true) {
        if (left === null || right === null) {
            return false
        }
        if (prototype === left) {
            return true
        }
        left = left.__proto__
    }
}
```

#### 15、new运算符工作原理？
new构造四部曲：
1、生成一个obj对象

2、链接原型 

3、绑定this 

4、返回对象
```
function create () {
    let obj = {}
    let Con = [].shift.call(arguments)
    obj.__proto__ = Con.prototype
    let result = Con.apply(obj, arguments)
    return result instanceof Object ? result : obj
}
```

    可以思考下这道题：new关键字的问题（return 1后会是什么情况）？
#### 16、继承的方法你知道几种？
1、借助构造函数实现继承

原理：将父类构造函数的this指向子类构造函数的实例上去

缺点：继承的子类上没有继承到父元素原型上的属性和方法，并没有实现真正的继承，只是实现了部分继承。

2、借助原型链实现继承（解决了继承到父元素到方法问题）

原理：子类构造函数的prototype链接到父类实例对象上实现原型链的继承

缺点：原型对象是共用的，子类构造函数构造出来的实例对象改变原型对象的属性时会导致另一个继承子类的实例对象的属性随之改变。原因是因为子类构造出来的子类执行的原型对象共用同一个地址。

3、组合方式 上面两种方式结合 （解决了公用地址问题）

缺点：子类构造函数的原型对象没有自己的constructor，所以会网上找到父类原型对象的构造函数，所以子类的constructor会指向父类的构造函数,并且父类的构造函数被调用了两次

```
function Parent (name) {
    this.name = name;
    this.color = ['red', 'blue'];
}

Parent.prototype.sayName = function () {
    console.log(this.name);
}

function Son (name, age) {
    Parent.call(this, name);
    this.age = age;
}
Son.prototype = new Parent();
Son.prototype.sayAge = function () {
    console.log(this.age);
}
let son = new Son('liming', 20);
let son2 = new Son('saner', 22);
console.log(son.name) // liming
console.log(son.sayAge()) // 20
console.log(Son.constructor === Parent.constructor) // true

```
4、组合继承优化 （解决以上两个问题）
```
function Parent (name) {
    this.name = name;
    this.color = [1, 2];
}

Parent.prototype.sayName = function () {
    console.log(this.name);
}

function Son (age) {
    Parent.call(this, name);
    this.age = age;
}
Son.prototype = Object.create(Parent.prototype); // 通过添加中间桥梁实现
Son.prototype.constructor = Son; // 将自己的构造函数指回自己
Son.prototype.sayAge = function () {
    console.log(this.age);
}
let son = new Son('liming', 20);
let son2 = new Son('saner', 22);
console.log(son.constructor); // function Son() {....}
```

5、圣杯继承方式

```
function inherit(target, origin) {
    function F() {};
    F.prototype = origin.prototype;
    target.prototype = new F();
    target.prototype.constructor = target;
    target.prototype.uber = origin.prototype;
}
```

#### 17、Call、apply的区别？
相同：都是改变this指向

不同点：传参列表不同，call是变量传入，apply是数组方式传入

#### 18、This指向问题？
1、对于函数直接调用的方式，不管在何处被执行，this一定指向window。

2、函数的执行时有调用者，那么谁调用this就指向谁

3、通过 new 的方式，this 永远被绑定在新创建的对象上，任何方式都改变不了 this 的指向。

4、通过call，apply改变this指向的时候，this指向传入的第一个参数值。

5、箭头函数执行时，this是包裹箭头函数最近的一个普通函数内的this

#### 19、闭包是什么原理？优缺点？
函数A里面包含了函数B，而函数B里面引用了函数A的变量，那么函数B被称为闭包。

或者：闭包就是能够读取其他函数内部变量的函数。

闭包存在的意义就是让我们可以间接访问函数内部的变量

$\color{#0ff}{优点：}$

一个是可以读取函数内部的变量，并且这个变量不会被回收

另一个是封装对象的私有属性和私有方法

$\color{#0ff}{缺点：}$ 

消耗内存，可能会造成内存溢出的情况。

闭包经典题：
```
for (var i = 1; i <= 5; i++) {
  setTimeout(function timer() {
    console.log(i)
  }, 1000)
}
```
以上这段代码的输出

    由于setTimeouts是个异步函数，所以会先把循环全部执行完毕,最后才会打印输出6个6
    
如何解决

    第一种方法  使用立即执行函数解决
```
for (var i = 1; i <= 5; i++) {
    (function(j) {
        setTimeout(function() {
            console.log(j);
        }, 1000)
    } (i));
}
```
    第二种方法  使用let的块级作用域解决
```
for (let i = 1; i <= 5; i++) {
    setTimeout(function () {
        console.log(i);
    }, 1000)
}
```
    第三种方法  使用setTimeout的第三个参数，这个参数会被当成 timer 函数的参数传入
```
for (var i = 1; i <= 5; i++) {
    setTimeout(function (i) {
        console.log(i);
    }, 1000, i)
}
```

#### 20、服务器如何与客户端实现长连接（如何识别客户端）？
HTTP协议采用‘请求-应答’模式，当采用普通模式，即非keep-alive模式时，每个请求/应答客户端和服务器端都要重新建立一个连接，完成之后立即断开连接（HTTP是无连接的协议）

持久连接时当使用keep-alive模式时，keep-alive功能使客户端到服务器端的连接持久有效，当出现对服务器的后继请求时，keep-alive公告避免了建立或者重新建立连接。

#### 21、事件的冒泡与捕获机制？

![事件流](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/1/12/16f97f0c368bd563~tplv-t2oaga2asx-image.image)
一个完整的事件流分三个阶段，第一阶段时捕获，第二阶段是目标阶段-即事件通过捕获到达目标阶段，第三阶段是冒泡阶段即从目标元素上传到window对象。

### Vue

#### 1、生命周期钩子函数（在特定时刻运行的函数）
每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子函数。

![生命周期图示](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/1/12/16f97fb8239f33a4~tplv-t2oaga2asx-image.image)

如上图所示
created中适合发送请求。

mounted阶段dom元素已经挂载，可以操作dom元素

updated函数可以根据更新后执行一系列操作

beforeDestroy可以用于移除事件、定时器等等，否则可能会引起内存泄露的问题。

destroyed进行组件的销毁操作，如果有子组件也会递归销毁，最后销毁父组件。

还有keep-alive的两个独有生命周期，分别为 activated 和 deactivated，用keep-alive包裹的组件在切换时不会销毁而且缓存到内存中并执行deactivated，命中缓存渲染时执行activated函数。

#### 2、v-show和v-if的区别？
v-show是切换display属性来达到显示与隐藏的效果的。这也就是说v-show刚开始一定会加载，有比较高的初始渲染开销，但是一旦渲染后就不需要销毁组件，所以更适合切换场景频率高的。

v-if是惰性的，如果刚开始为false的话那么就不会挂载组件，直到条件为true是才会去挂载渲染组件。并且在切换是会触发销毁/挂载组件，切换开销比较大，适合不经常切换的场景。当然这种惰性渲染也可以减少页面初始渲染开销。


#### 3、computed和watch有什么区别?
    computed常用于计算值的场景
    计算属性是基于它们的响应式依赖进行缓存的，所以当值没发生改变时时不会执行的
    还可以使用setter函数来处理一些逻辑
    
```
    computed: {
        fullName: {
            // getter
            get: function () {
                return this.firstName + ' ' + this.lastName
            },
            // setter
            set: function (newValue) {
            var names = newValue.split(' ')
            this.firstName = names[0]
            this.lastName = names[names.length - 1]
        }
    }
```
    
    watch用来观察和响应 Vue 实例上的数据变动，实时对数据监听并处理一些复杂的逻辑操作
    无缓存性，页面重新渲染时值不变化也会执行
常见的监听使用方式：
    
```
watch: {
    // 该回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深
    c: {
      handler: function (val, oldVal) { /* ... */ },
      deep: true
    },
    // 该回调将会在侦听开始之后被立即调用
    d: {
      handler: 'someMethod',
      immediate: true
    },
    //e: {
    //  f: {
    //    g: 5
    //  }
    //}
    e: [
      'handle1',
      function handle2 (val, oldVal) { /* ... */ },
      {
        handler: function handle3 (val, oldVal) { /* ... */ },
        /* ... */
      }
    ],
    // watch vm.e.f's value: {g: 5}
    'e.f': function (val, oldVal) { /* ... */ }
  }
}
```

#### 4、如何动态绑定class和style？
1、绑定class
使用对象来绑定：```<div v-bind:class="{ active: isActive }"></div>```

使用数据绑定多个：```<div v-bind:class="[activeClass, errorClass]"></div>```

2、绑定style

使用对象来绑定```<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>```

使用数组绑定多个```<div v-bind:style="[baseStyles, overridingStyles]"></div>```

#### 5、key的作用？
为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 key 属性。以便于在diff是可以快速查找到某个元素。

#### 6、keep-alive 组件有什么作用？
如果你在组件切换的时候，需要保存一些组件的状态防止多次渲染，就可以使用 keep-alive 组件包裹需要保存的组件。

用 keep-alive 包裹的组件在切换时不会进行销毁，而是缓存到内存中并执行 deactivated 钩子函数，命中缓存渲染后会执行 actived 钩子函数。

#### 7、组件通信的方式有多少种？
1、父子组件

2、非父子组件

$\color{#0ff}{父子组件}$

1、使用props向子组件传递数据，子组件通过emit触发事件向父组件传递数据

2、父组件通过ref调用子组件的方法

3、通过.sync语法实现数据的父子组件的通信

$\color{#0ff}{非父子组件}$

1、provide 选项允许我们指定我们想要提供给后代组件的数据/方法，使用provide注入，子组件中通过inject接受即可使用

2、Event Bus 解决，在Vue.prototype上面挂载bus总线，使用bus的emit和on事件完成

3、使用vuex来实现跨组件通信。

#### 8、组件中 data 为什么是个函数而根组件中却是对象？
组件复用时所有组件实例都会共享一个 data，如果 data 是对象的话，就会造成一个组件修改 data 以后会影响到其他所有组件，所以需要将 data 写成函数，每次用到就调用一次函数获得新的数据。而new Vue的实例，是不会被复用的，因此不存在对象引用问题。

#### 9、Vue双向绑定的实现？

原理：
```
unction observe(obj) {
  // 判断类型
  if (!obj || typeof obj !== 'object') {
    return
  }
  Object.keys(obj).forEach(key => {
    defineReactive(obj, key, obj[key])
  })
}

function defineReactive(obj, key, val) {
  observe(val)
  Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    // get函数
    get: function reactiveGetter() {
      console.log('get')
      return val
    },
    // set函数
    set: function reactiveSetter(newVal) {
      console.log('set')
      val = newVal
    }
  })
}
```

#### 10、mixin 和 mixins 区别？
mixin 用于全局混入，会影响到每个组件实例，通常插件都是这样做初始化的。

mixins应该是来分发 Vue 组件中的可复用功能。如果多个组件中有相同的业务逻辑，就可以将这些相同的逻辑剥离出来，通过 mixins 混入代码，但是要注意合并的一些规则等。

#### 11、nextTick函数有什么作用？

nextTick可以让我们在下次 DOM 更新循环结束之后执行延迟回调，用于获得更新后的 DOM。

当我们在mounted中操作ref时就可以在nextTick中执行。

```
// DOM 还未更新
this.nextTick(function () {
  // DOM 已经更新
  this.$refs.$el.xxx
})
```

#### 12、单页面与多页面的优缺点？


![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/1/12/16f9850c1bdb997b~tplv-t2oaga2asx-image.image)

#### 13、对于Vue的优化方案？
    1、区分使用v-if和v-show的使用
    2、区分computed和watch的使用
    3、v-for的使用时必须加上key值。
    4、事件，定时器的销毁操作
    5、图片资源的懒加载（vue插件）
    6、路由的懒加载
    7、第三方插件的按需引入如Echarts
    8、组件的异步加载
    9、服务端渲染
    10、使用alias来简化路径查找
    11、loader配置使用exclude来快速查找文件

#### 14、什么是VNode？
Virtual DOM 其实就是一棵以 JavaScript 对象（VNode 节点）作为基础的树，用对象属性来描述节点，实际上它只是一层对真实 DOM 的抽象。最终可以通过一系列操作使这棵树映射到真实环境上。

#### 15、Vue中的服务器代理如何设置？
vue-cli3.0以后都是在根目录下新建vue.config.js来配置对应需要修改的配置的。

代理设置：
```
devServer: {
    port: 8080,
    open: true,
    proxy: {
        '/api/login': {
            target: 'http://localhost:8081',
            changeOrigin: true,
            pathRewrite: {
                 '^/api': '/api/4356'
            }
        }
    }
}
```

### Git
#### 1、Git pull操作相当于什么？

git pull相当于git fetch + git merge

git fetch是将远程主机的最新内容拉到本地，用户在检查了以后决定是否合并到工作本机分支中。

而git pull 则是将远程主机的最新内容拉下来后直接合并，即：git pull = git fetch + git merge，这样可能会产生冲突，需要手动解决。

#### 2、git merge和git rebase的区别？
git merge是git会自动根据两个分支的共同祖先的这个 commit 和两个分支的最新提交 进行一个三方合并，然后将合并中修改的内容生成一个新的 commit。并且合并最新的commit是在合并的分支上。

如果要master合并test分支，那么就在master上使用git merge test即可

git rebase，衍合在当前分支上重演另一个分支的历史，提交历史是线性的。 本质上，这是线性化的自动的 cherry-pick，会将两个分支合并为一个分支。

如果要将test分支合并到master上，那么要在test上使用git rebase master

个人觉得写的不错的文章[git merge 与 git rebase的区别](https://blog.csdn.net/zzhongcy/article/details/86476160)

#### 3、git commit --amend的用法？
git commit -m '描述'提交之后，发现-m的说明文字写的有问题，想要重新写一次，也就是想撤销上次的提交动作，重新提交一次，则可以使用这个操作。

#### 4、git reset回滚操作?
reset命令把当前分支指向另一个位置，并且有选择的变动工作目录和索引。也用来在从历史仓库中复制文件到索引，而不动工作目录。

如果不给选项，那么当前分支指向到那个提交。如果用--hard选项，那么工作目录也更新，如果用--soft选项，那么都不变。


![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/1/12/16f98785692ef997~tplv-t2oaga2asx-image.image)

#### 5、查看git的提交历史？
git log 命令可以显示所有提交过的版本信息。

git reflog 可以查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作）

#### 6、git提交流程？
1、当本地文件修改后使用git add file将文件放入暂存区

2、git commit 给暂存区域生成快照并提交到仓库中。

3、git push将本地仓库的代码同步的远程仓库中去。

4、git reset -- files 用来撤销最后一次git add files，你也可以用git reset 撤销所有暂存区域文件。--soft不撤销工作区代码，--hard撤销工作区代码。

5、git checkout -- files 把文件从暂存区域复制到工作目录，用来丢弃本地修改。

### Linux

#### 1、查杀进程？
    1、lsof -i tcp:端口号  查看当前进程的信息
    
    kill pid 杀掉某个pid的进程服务
    
    2、ps -ef | grep 8099
    
    kill -9 PID  强行杀掉某个pid的进程服务

    3、ps -ef | grep server.js

    查找server.js的进程号

    4、开启一个守护进程

    pm2 start app.js --env production --name test

#### 2、文件重命名操作？
mv可以用来修改单文件改名操作 mv file1 file2 将file1更改为file2

mv还具备文件移动功能

    mv  b  sm/    将文件（夹）b 移动到当前目录下的sm目录下
    mv (-f如果目标存在直接强行覆盖 -b移动时为目标创建一个备份 -i目标存在会交互提醒是否要覆盖) 源文件  目标文件
    mv  b  sm/c    将文件（夹）b移动到当前目录下的sm目录下并重命名为c

ename 批量更改文件名 rename test zhangsan test? 将test\*的文件改为zhangsan*

#### 3、文件压缩解压缩？
tar -czvf test.tar ./*  打包当前文件下的文件成为test.tar文件

tar -xzvf test.tar    将test.tar内的打包文件解压缩至原来的地方

#### 4、ls -l  /proc/70681 查看当前的PID服务在哪个文件夹下面

#### 5、ps -a 使用PS指令查询进程

#### 6、ps (-e选择所以进程 -f列出完整列表 -a显示由同一个TTY -x选择进程且不含控制的TTY)

#### 7、nohup npm run dev & 以忽略挂起信号方式允许前端服务器

#### 8、启动后台服务：nohup node app.js & &nbsp;&nbsp; 后台运行----开启后台服务进程，并且关机服务也不会停

#### 9、启动前台服务：nohup node app.js &nbsp;&nbsp;--开启前台服务，关机则停止


### Hr面
这些问题也是我比较常见问到的。嗯，答案我也没有标准的，只能将问题给大家参考。
#### 1、为什么选择我们公司？

#### 2、你觉得你比别人的优势在哪？

#### 3、你职业规划是怎样的呢 ？

#### 4、你平时的怎么学习的？

#### 5、你为啥要学前端？

#### 6、你对前端工程师这个职位是怎么样理解的？


### 个人提问

#### 1、目前前端部门常用的技术栈是什么？

#### 2、公司是否会有技术分享交流活动？

#### 3、公司技术团队的架构和人员组成？

#### 4、公司的业务方向是怎么样的呢？

#### 5、你认为你们公司的优势是什么？


### 最后
***
各位觉得文章还行的可以点个赞激励我继续前行，另外有任何问题也可以评论区交流。有错误也欢迎大家指正。



