---
title: Tsup原理解析
date: 2022-09-26T21:40:00.000+00:00
lang: zh
duration: 25min
description: 介绍打包工具Tsup的原理
---

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
   import { defineConfig } from 'tsup'
   
   export default defineConfig({
     entry: ['src/index.ts'],
     splitting: false,
     sourcemap: true,
     clean: true,
   })
   ```

3. 直接通过 script 脚本运行

   为什么推荐你用脚本呢？因为可以重复使用只需要执行一条`npm`命令就行了

   当然你也可以直接使用 cli 命令，因为`tsup`也支持了 cli 命令，cli 有一个快速生成工具[cac](https://github.com/cacjs/cac)，如果你有兴趣也可以看看这个工具，本次我们不讲解这个工具。
   ```typescript
   "script": {
        "build": "tsup",
        "dev": "tsup --watch"
    }
   ```
在 dev 的情况下你可以进行打包并监听文件的改变进行打包，这样就可以快速看到效果了

## 3. 原理分析
1. 解析options
在`cli-main`里面使用`cac`注册了 cli，那么我们可以看到，重点看到`build`方法，这个就是打包的入口文件
```typescript
// index.ts下的build方法
const config =
    _options.config === false
      ? {}
      : await loadTsupConfig(
          process.cwd(),
          _options.config === true ? undefined : _options.config
        )
```
在build函数里面我们可以看到loadTsupConfig这个方法，这里会解析用户的tsup的配置文件

当然会判断用户的options。config如果有就直接使用用户的如果没有就loadTsupConfig

这里可以看到对于用户的配置性就是我们需要考虑的事情了，很多时候并不仅仅是给用户开放了配置文件就行，还需要考虑到用户是否需要自定义配置文件

```typescript
// load.ts
if (configPath) {
    if (configPath.endsWith('.json')) {
      // load package.json文件
      let data = await loadJson(configPath)
      if (configPath.endsWith('package.json')) {
        data = data.tsup
      }
      if (data) {
        return { path: configPath, data }
      }
      return {}
    }

    const config = await bundleRequire({
      filepath: configPath,
    })
    return {
      path: configPath,
      data: config.mod.tsup || config.mod.default || config.mod,
    }
  }
```
configPath就是从那些配置文件进行获取，这里的优先级也就是上面说的那些优先级，如果同时设置两个，那么会按照优先级拿到对应的configPath，然后解析拿到对应的配置文件

```typescript
const configData =
    typeof config.data === 'function'
      ? await config.data(_options)
      : config.data
```
这里配置文件可能是个function，支持用户直接调用，传入的则是cli的options，这样就可以让用户拿到对应的options进行处理

这里是不是想到了`Vite`的配置文件，`Vite`的配置文件也是支持function的，这样就可以让用户拿到对应的options进行处理，扩展性更高
2.创建logger
```typescript
const logger = createLogger(item?.name)
``` 
logger在与用户交互行较强的工具上都是必须的，因为为了给用户更强的交互性，方便让用户知道目前进行到了哪一步、哪一步出错了、哪一步成功了、成功的用了多少时间、尺寸多大等等信息，更加方便用户的使用

库里面使用了[colorette](https://github.com/jorgebucaran/colorette)这个模块进行log输出的，大家有兴趣也可以看看这个模块或者找找有没有更好的
3.打包dts
```typescript
const worker = new Worker(path.join(__dirname, './rollup.js'))
worker.postMessage({
 configName: item?.name,
 options: {
   ...options, // functions cannot be cloned
   banner: undefined,
   footer: undefined,
   esbuildPlugins: undefined,
   esbuildOptions: undefined,
   plugins: undefined,
   treeshake: undefined,
   onSuccess: undefined,
   outExtension: undefined,
 }
})
```
dts也就是生成typescript的文件类型，用户在使用模块时能够快速的获取到类型提升开发效率
这里使用了rollup进行打包，`rollup-plugin-dts`这个插件进行打包，这里使用了worker进行dts的打包，这样就不会阻塞主线程，提升打包速度

```typescript
inputConfig: {
   input: dtsOptions.entry,
   onwarn(warning, handler) {
     if (
       warning.code === 'UNRESOLVED_IMPORT' ||
       warning.code === 'CIRCULAR_DEPENDENCY' ||
       warning.code === 'EMPTY_BUNDLE'
     ) {
       return
     }
     return handler(warning)
   },
   plugins: [
     // 清除插件
     tsupCleanPlugin,
     // 对.ts .d.ts进行解析，其他文件进行过滤
     tsResolveOptions && tsResolvePlugin(tsResolveOptions),
     hashbangPlugin(),
     jsonPlugin(),
     // 哪些文件需要忽略比如.json .jsx等
     ignoreFiles,
     // dtsPlugin
     dtsPlugin.default({
       compilerOptions: {
         ...compilerOptions,
         baseUrl: compilerOptions.baseUrl || '.',
         // Ensure ".d.ts" modules are generated
         declaration: true,
         // Skip ".js" generation
         noEmit: false,
         emitDeclarationOnly: true,
         // Skip code generation when error occurs
         noEmitOnError: true,
         // Avoid extra work
         checkJs: false,
         declarationMap: false,
         skipLibCheck: true,
         preserveSymlinks: false,
         // Ensure we can parse the latest code
         target: ts.ScriptTarget.ESNext,
       },
     }),
   ].filter(Boolean),
   external: [...deps, ...(options.external || [])],
},
outputConfig: {
   dir: options.outDir || 'dist',
   format: 'esm',
   exports: 'named',
   banner: dtsOptions.banner,
   footer: dtsOptions.footer,
}
```
调用rollup打包之前会对用户的tsconfig.json进行解析和用户的配合进行合并，然后交由dtsPlugin去进行类型打包输出

同时`rollup`也可以进行watch，根据文件的改变实时进行类型打包，并把结果通过postMessage发送给主线程
```typescript
const startRollup = async (options: NormalizedOptions) => {
  const config = await getRollupConfig(options)
  if (options.watch) {
    watchRollup(config)
  } else {
    try {
      await runRollup(config)
      parentPort?.postMessage('success')
    } catch (error) {
      parentPort?.postMessage('error')
    }
    parentPort?.close()
  }
}
```
4.打包ts文件
ts文件主要是通过esbuild进行打包的，比较依赖于esbuild的功能，当然也继承了esbuild的快
```typescript
...options.format.map(async (format, index) => {
   const pluginContainer = new PluginContainer([
     shebang(),
     ...(options.plugins || []),
     treeShakingPlugin({
       treeshake: options.treeshake,
       name: options.globalName,
     }),
     cjsSplitting(),
     es5(),
     sizeReporter(),
   ])
   await pluginContainer.buildStarted()
   await runEsbuild(options, {
     pluginContainer,
     format,
     css: index === 0 || options.injectStyle ? css : undefined,
     logger,
     buildDependencies,
   }).catch((error) => {
     previousBuildDependencies.forEach((v) =>
       buildDependencies.add(v)
     )
     throw error
   })
   })
```
runEsbuild函数主要就是负责进行esbuild打包输出，讲`pluginContainer`传入，可以使用插件对各个生命周期函数进行处理，比如打包前调用`pluginContainer.buildStarted`进行输出或者打印信息
4.1. pluginContainer
   ```typescript
   export class PluginContainer {
     plugins: Plugin[]
     context?: PluginContext
     constructor(plugins: Plugin[]) {
       this.plugins = plugins
     }

     setContext(context: PluginContext) {
       this.context = context
     }
     // ....
     async buildStarted() {
       for (const plugin of this.plugins) {
         if (plugin.buildStart) {
           await plugin.buildStart.call(this.getContext())
         }
       }
     }
   }
   ```
   pluginContainer作用可以是给用户暴露各个插件的生命周期，用户可以开发插件，实现插件的高度扩展性

   比如sizeReporter插件，就是在打包结束后，输出打包的文件大小
   ```typescript
   export const sizeReporter = (): Plugin => {
     return {
       name: 'size-reporter',

       buildEnd({ writtenFiles }) {
         reportSize(
           this.logger,
           this.format,
           writtenFiles.reduce((res, file) => {
             return {
               ...res,
               [file.name]: file.size,
             }
           }, {})
         )
       },
     }
   }
   ```
4.2 runEsbuild方法
runEsbuild的功能主要就是对文件进行打包，输出，同时在打包输出后也会调用pluginContainer.buildFinished函数，可以在打包执行后做一些操作
```typescript
const pkg = await loadPkg(process.cwd())
const deps = await getDeps(process.cwd())
const external = [
 // Exclude dependencies, e.g. `lodash`, `lodash/get`
 ...deps.map((dep) => new RegExp(`^${dep}($|\\/|\\\\)`)),
 ...(options.external || []),
]
const outDir = options.outDir

const outExtension = getOutputExtensionMap(options, format, pkg.type)
const env: { [k: string]: string } = {
 ...options.env,
}

if (options.replaceNodeEnv) {
 env.NODE_ENV =
   options.minify || options.minifyWhitespace ? 'production' : 'development'
}
```
首先先对options数据进行处理，比如获取依赖，获取输出目录，获取文件后缀，获取环境变量等。

因为是基于esbuild打包，所以配置都是围绕着esbuild的配置进行兼容处理的。

```typescript
const esbuildPlugins: Array<EsbuildPlugin | false | undefined> = [
 format === 'cjs' && nodeProtocolPlugin(),
 {
   name: 'modify-options',
   setup(build) {
     pluginContainer.modifyEsbuildOptions(build.initialOptions)
     if (options.esbuildOptions) {
      // 用户可以传入esbuildOptions做一些callback操作
       options.esbuildOptions(build.initialOptions, { format })
     }
   },
 },
 // esbuild's `external` option doesn't support RegExp
 // So here we use a custom plugin to implement it
 // external插件，可以将依赖排除在外
 format !== 'iife' &&
   externalPlugin({
     external,
     noExternal: options.noExternal,
     skipNodeModulesBundle: options.skipNodeModulesBundle,
     tsconfigResolvePaths: options.tsconfigResolvePaths,
   }),
// 处理tsconfigDecoratorMetadata，使用swc触发 decorator metadata
 options.tsconfigDecoratorMetadata && swcPlugin({ logger }),
 // 原生模块的处理
 nativeNodeModulesPlugin(),
 // css打包的插件
 postcssPlugin({ css, inject: options.injectStyle }),
 // svelte的处理插件
 sveltePlugin({ css }),
 // 用户自定义的插件，最后执行
 ...(options.esbuildPlugins || []),
]
```
插件就不讲太多了，只简单讲了下其作用，因为主要是带大家了解内部的打包原理

然后就是调用esbuild的build方法进行打包输出
```typescript
result = await esbuild({
   entryPoints: options.entry,
   format:
     (format === 'cjs' && splitting) || options.treeshake ? 'esm' : format,
   bundle: typeof options.bundle === 'undefined' ? true : options.bundle,
   platform,
   globalName: options.globalName,
   jsxFactory: options.jsxFactory,
   jsxFragment: options.jsxFragment,
   sourcemap: options.sourcemap ? 'external' : false,
   target: options.target,
   banner,
   footer,
   tsconfig: options.tsconfig,
   // ....
   plugins: esbuildPlugins,
   // ....
   logLevel: 'error',
   minify: options.minify,
})
```
打包完成之后，在调用pluginContainer.buildFinished方法，进行buildFinished生命周期的回调
```typescript
// Manually write files
if (result && result.outputFiles) {
 await pluginContainer.buildFinished({
   outputFiles: result.outputFiles,
   metafile: result.metafile,
 })

 const timeInMs = Date.now() - startTime
 logger.success(format, `⚡️ Build success in ${Math.floor(timeInMs)}ms`)
}
```
如果设置了metafile，那么也会把对应的metafile写入到outdir目录下面的`metafile-${format}.json`文件中

之前讲到了dts会有watch模式监听文件变化进行打包

这里的esbuild也会监听文件变化，不同的是，`esbuild`打包是通过`chokidar`进行文件的监听的。
```typescript
const { watch } = await import('chokidar')
// 获取到监听的文件路径
const watchPaths =
 typeof options.watch === 'boolean'
   ? '.'
   : Array.isArray(options.watch)
   ? options.watch.filter(
       (path): path is string => typeof path === 'string'
     )
   : options.watch
// 根据watchPaths以及ignored生成watcher
const watcher = watch(watchPaths, {
    ignoreInitial: true,
    ignorePermissionErrors: true,
    ignored,
})
// 监听文件变化，进行打包
// 其中watch的回调打包进行了debounce优化
watcher.on('all', (type, file) => {
    file = slash(file)
    // By default we only rebuild when imported files change
    // If you specify custom `watch`, a string or multiple strings
    // We rebuild when those files change
    if (options.watch === true && !buildDependencies.has(file)) {
      return
    }
    logger.info('CLI', `Change detected: ${type} ${file}`)
    debouncedBuildAll()
})
```
最后将两个任务都放到`Promise.all`中执行，进行并行打包
```typescript
await Promise.all([dtsTask(), mainTasks()])
```

## 总结
好了，到这里就讲完了esbuild的打包原理，虽然是相对比较浅的进行了讲解，但是如果你正在使用`tsup`或者你在学习`esbuild`的话都是有帮助的，当然如果你想了解插件系统怎么实现，也可以参考这个，希望对你有所帮助。

