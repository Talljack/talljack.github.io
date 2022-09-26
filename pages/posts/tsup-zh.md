## 1. 前言

今天要写的是打包工具`Tsup` ，它可以快速打包 typescript 库，无需任何配置，并且基于`esbuild`进行打包，同时也可以快速生成`ts`类型，它还支持`Cli`脚手架运行，方便又高效

随着`esbuild`的兴起，越来越多的打包工具开始使用`esbuild`做为打包底层工具，其中`Vite`最具代表性，它就是采用`esbuild`来支持 `.ts`、`jsx`、`.tsx` 代码的转化，当然 Vite 目前主要用于项目打包中，而 `Tsup`则主要用于`typescript`库的打包，它支持`watch`模式，开发过程中更改代码可以快速看到效果，极大地提高了开发效率。

相信很多小伙伴已经惊奇这个能力是怎么做到的，那么我通过`Tsup`这个库来分析下它是如何进行打包的，同时你也可以学习到`esbuild`的`plugin`的使用、`Rollup`的`plugin`的使用、如何封装库里面的插件系统即`pluginContainer`以及`node`相关的一些库的功能

## 2. 使用

1. 安装

   这里就使用`pnpm`了，`pnpm`的安装速度比较快以及在依赖管理方面进行了优化，具体可见文章[Pnpm: 最先进的包管理工具](https://zhuanlan.zhihu.com/p/404784010)。

   ```ts
   pnpm add tsup -D
   ```

2. 配置文件

   配置比较简单，看一下官方文档基本上就可以直接上手使用

   目前支持了如下几种配置文件类型

   - tsup.config.ts
   - tsup.config.js
   - tsup.config.cjs
   - tsup.config.json
   - 以及 package.json 中配置

   ```ts
   import { defineConfig } from "tsup";

   export default defineConfig({
     entry: ["src/index.ts"],
     splitting: false,
     sourcemap: true,
     clean: true,
   });
   ```

3. 直接通过 script 脚本运行

   为什么推荐你用脚本呢？因为可以重复使用只需要执行一条`npm`命令就行了

   当然你也可以直接使用 cli 命令，

4.
