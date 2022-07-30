---
title: Vite文件加载原理
date: 2022-07-30T23:00:00.000+00:00
lang: zh
duration: 25min
description: 粗略的介绍Vite文件加载原理
---

### 1.加载入口实现

es6模块都是通过type设置为module来实现的，浏览器加载 ES6 模块，也使用`<script>`标签，但是要加入`type="module"`属性，可参考[浏览器加载规则](https://es6.ruanyifeng.com/#docs/module-loader#%E5%8A%A0%E8%BD%BD%E8%A7%84%E5%88%99)，所以在项目中需要设置index.html里面的文件为`type="module"` 即可完成浏览器加载。

```ts
<script src="/src/main.ts" type="module"></script>
```

### 2.文件的加载

首先在把 `type= "module"`放到`<script>`标签中来声明这是一个模块，这样通过`src`或`import`导入的文件将会发起http请求；vite会拦截这些请求，并将请求文件进行特别处理。

我们通过起一个koa服务器来解析数据，当访问/时即把index.html返回即可

```ts
if (url === '/') {
  const home = fs.readFileSync('./index.html', 'utf-8')
  ctx.type = 'text/html'
  ctx.body = home
}
```

返回之后会去加载main.ts模块然后发起一个http请求到服务端

### 3.解析ts/js文件

服务端拿到ts文件后会读取文件信息，发现下面这段代码

```ts
import { createApp } from 'vue'
```

之间返回读取的file后就会发现有如下的报错信息，没办法解析vue模块，所以需要对vue模块进行模块替换

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/66a441eb574846fa955abb38fb399d0d~tplv-k3u1fbpfcp-zoom-1.image)

vite中会存在裸模块替换，当试图请求node_modules文件夹的文件时，会进行`裸模块替换`(路径转换为相对路径)，浏览器中只识别`相对路径`和`绝对路径`。

> **import Vue from 'vue'** 会转换成 **import Vue from '/@modules/vue'**

接下里将`/@modules`解析为真正的文件地址，并返回给浏览器。其它打包工具，如`webpack`打包时帮我们做了这件事情。通过 `import` 导入的文件 `webpack` 会去 `node_modules/包名/package.json` 文件内找 `moduel` 属性，如下例子。

```ts
{ 
  "license": "MIT",
  "main": "index.js",
  "module": "dist/vue.runtime.esm-bundler.js",
  "name": "vue",
  "repository": {
    "type": "git",
    "url": "git+https://github.com/vuejs/vue-next.git"
  },
  "types": "dist/vue.d.ts",
  "unpkg": "dist/vue.global.js",
  "version": "3.2.20"
}
```

只要将这个`dist/vue.runtime.esm-bundler.js`这个地址文件返回就好了。所以需要进行模块替换解析，下面使用了rewriteImport函数实现，vite是用的[es-module-lexer](https://github.com/guybedford/es-module-lexer)来解析成ast拿到import的地址

```ts
 else if (url.endsWith(".ts")) {
    // 2. 返回ts
    const filePath = path.join(__dirname,url) // 获取绝对路劲
    const file = fs.readFileSync(filePath, 'utf-8')
    ctx.type = "application/javascript"
    ctx.body = rewriteImport(file)
  }
```

```
function rewriteImport(content) {
  return content.replace(/ from ['"](.*)['"]/g, (s1, s2) => {
    // s1, 匹配部分， s2: 匹配分组内容
    if (s2.startsWith("./") || s2.startsWith("/") || s2.startsWith("../")) {
      // 相对路劲直接返回
      return s1;
    } else {
      return ` from "/@modules/${s2}"`
    }
  })
}
```

替换模块之后发现返回的内容，并且并没有报错信息

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/67c42b26572f41caaedd3f9d34ce608d~tplv-k3u1fbpfcp-zoom-1.image)

同时也请求了一个http://localhost:3003/@modules/vue请求，接下来我们对这个请求进行处理

### 4.解析第三方模块

按照上面的分析我们只需将请求路劲以`/@modules/`开头的请求返回 `node_modules/包名/package.json`里面的`module`属性值文件即可，因为在node_modules里面有对应的模块存在，只需要将其解析出来就行，同时需要rewriteImport，因为模块可能也加载了其他模块

```
 else if (url.startsWith("/@modules/")) {
    const filePrefix = path.resolve(__dirname, 'node_modules', url.replace('/@modules/', ''))
    // /Users/yugangcao/Downloads/mini-vite/node_modules/vue
    const module = require(filePrefix + '/package.json').module
    // dist/vue.runtime.esm-bundler.js
    const file = fs.readFileSync(filePrefix + '/' + module, 'utf-8')
    // 读取文件地址 /Users/yugangcao/Downloads/mini-vite/node_modules/vue/dist/vue.runtime.esm-bundler.js
    ctx.type = "text/javascript"
    ctx.body = rewriteImport(file)
  }
```

### 5.解析.vue文件

main.ts里面会发出http://localhost:3003/src/App.vue 请求，加载App.vue文件，在`vite`中，单文件处理是使用的是`@vue/compiler-sfc`模块进行编译处理的,因此咱们也借助该模块完成Vue文件的处理。 它解析的结果大概如下：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7440bcc0090b4bb69d6366f9e9007149~tplv-k3u1fbpfcp-zoom-1.image)

同时在vue中我们使用`@vue/compiler-dom` 来编译 `template`，由于我们返回的`vue`是`runtime`版本的，是没有编译器的，我们应该将编译好的template返回回去，下面我们返回一个渲染（render）函数即可 ，[体验地址](https://vue-next-template-explorer.netlify.app/#eyJzcmMiOiI8ZGl2PjIzMjM8L2Rpdj4iLCJvcHRpb25zIjp7Im9wdGltaXplSW1wb3J0cyI6ZmFsc2V9fQ==)

> rewriteImport(compile(templateContent, { mode: "module" }).code

```
export function render(_ctx, _cache) {
  const _component_HelloWorld = _resolveComponent("HelloWorld")

  return (_openBlock(), _createElementBlock(_Fragment, null, [
    _createElementVNode("img", {
      alt: "Vue logo",
      src: "./assets/logo.png"
    }),
    _createVNode(_component_HelloWorld, { msg: "Hello Vue 3 + TypeScript + Vite" })
  ], 64 /* STABLE_FRAGMENT */))
}
```

同时把解析的scrip内容进行rewriteImport后返回去加载其他模块，如果有style则需要加载一个style文件，解析代码如下

```ts
else if (url.endsWith(".vue")) {
    // 解析单文件组件相当于vue-loader做的事情
    // 转换script部分：将默认导出的组件对象转换为常量
    const p = path.resolve(__dirname, url.slice(1));
    const ret = parse(fs.readFileSync(p, "utf-8"));
    console.log('vue', ret)
    const scriptContent = ret.descriptor.script?.content ?? ret.descriptor.scriptSetup?.content;
    const script = scriptContent.replace(
      "export default ",
      "const __script = "
    );
    const templateContent = ret.descriptor.template.content;

    // 转换template为模板请求
    // 将转换获得的渲染函数设置到__script上
    // 最后重新导出__script
    ctx.type = "text/javascript";
    ctx.body = `
${rewriteImport(compile(templateContent, { mode: "module" }).code)}
${rewriteImport(script)}
// 如果有 style 就发送请求获取 style 的部分
${ret.descriptor.styles.length ? `import "${url}?type=style"` : ''}
__script.render = render
export default __script
    `;
  }
```

### 6.解析style

从.vue文件里返回的信息里会发出一个http://localhost:3003/src/App.vue?type=style 请求，Vite对style的处理比较特殊，处于`热更新`模块中，由于我们没有实现热更新，咱们这儿就模拟实现一下，将style content返回，在客户端实现该方法。

```
else if (url.endsWith('?type=style')) {
    const p = path.resolve(__dirname, url.split("?")[0].slice(1));
    const ret = parse(fs.readFileSync(p, "utf-8"));
    const styleBlock = ret.descriptor.styles[0];
    // console.log('style', styleBlock)
    ctx.type = "application/javascript";
    ctx.body = `
      const css = ${JSON.stringify(styleBlock.content)};
      updateStyle(css);
      export default css;
    `;
  }
```

在`index.html`中添加`updateStyle`代码：

```
// ...
<body>
    <div id="app"></div>
    <script>
     function updateStyle(content) {
       const isExist = typeof CSSStyleSheet !== undefined
       if(isExist) {
         console.log('CSSStyleSheet is exist')
          // 方法1，使用可构造样式表
        let cssStyleSheet = new CSSStyleSheet()
        cssStyleSheet.replaceSync(content)
        document.adoptedStyleSheets = [
          ...document.adoptedStyleSheets,
          cssStyleSheet
        ]
       } else {
         // 方法2
        let style = document.createElement('style')
        style.setAttribute('type', 'text/css')
        style.innerHTML = content        document.head.appendChild(style)
       }
      }
    </script>
    <script src="/src/main.ts" type="module"></script>
</body>
// ...
```

### 7.解析静态文件

以png图片为例，当拦截到.png时，直接将读取到的文件信息返回即可

```
else if (url.endsWith(".png") | url.endsWith('.svg')) {
    const imgPath = path.join(__dirname,'src', url) // 获取绝对路劲
    ctx.body = fs.readFileSync(imgPath);
  }
```

### 8.解析.css文件

判断.css文件然后读取文件，加上插入代码后返回渲染即可

```
else if(url.endsWith('.css')){
    const p = path.resolve(__dirname,url.slice(1))
    const file = fs.readFileSync(p,'utf-8')
    const content = `const css = "${file.replace(/\n/g,'')}"
      updateStyle(css);
      export default css
    `
    ctx.type = 'application/javascript'
    ctx.body = content
  }
```

### 9.解析错误问题

当解析node_modules中代码中会出现一个小错，就是vue源码里有用process.ENV判断环境的，我们浏览器client里设置以下即可，

错误信息如下

```
Uncaught ReferenceError: process is not defined
    at shared:442
```

我们在**rewriteImport** 中配置**将process.env.NODE_ENV替换为development** 即可，当然也可以在index.html中添加这个环境变量也可以

最终页面的渲染效果图:

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1836c2d3994b4fb3aa56ce6d85da1095~tplv-k3u1fbpfcp-zoom-1.image)
