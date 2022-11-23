---
title: 分享一些vs code常用的插件
date: 2022-11-23T22:20:00.000+00:00
lang: zh
duration: 25min
description: 分享一些vs code常用的插件
---


## `前言`

最近在家办公，写代码发现没有那么流畅，一看是某些插件没有安装，搞得写代码的效率降低，所以这里有些比较实用的插件推荐给大家

## `开发实用插件`

### `Settings Sync`

利用 `Settings Sync` 💎将 `VS Code` 的设置保存在github上面，在换了电脑后可以快速同步vscode的配置（插件、代码片段、主题、file icon、热键等）

![在这里插入图片描述](https://img-blog.csdnimg.cn/1e075a355fed420589268eae03023d40.png)


先去GitHub创建一个git gist id，然后设置到vscode的设置里面就行，在github主页点击右上角头像下拉有一个 `Your gists`，里面没有的话则需要注册一个

![在这里插入图片描述](https://img-blog.csdnimg.cn/547ec6536e724c43be320bccdc087f2b.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/72e7dd2fc4c44aafa5a28c2fe907207b.png)

然后利用快捷键进行上传和下载

✅ 1. 上传快捷键Upload Key : **Shift + Alt + U** （Mac是 **Shift + Option + U**）

✅ 2. 下载快捷键Download Key : **Shift + Alt + D** （Mac是 **Shift + Option + D**）

### `GitHub Copilot`



AI编写代码，非常智能，很多常见的代码可以一键完成✅。

![在这里插入图片描述](https://img-blog.csdnimg.cn/fd54dbdbb3ee4f73b24be323d01c35e4.png)


如果是学生或者github的热门项目的贡献者的话是可以免费使用的，如果不是则需要付费使用

效果图如下，通过round名字可以快速生成你想要的代码片段，如果不是则舍弃即可

![在这里插入图片描述](https://img-blog.csdnimg.cn/4e346605f5ae4ce2b07a9184d40bb6d9.png)


### `Import Cost`

显示导入包的大小，能够在开发过程中就清晰的知道引入包的尺寸

![在这里插入图片描述](https://img-blog.csdnimg.cn/ea279abc08fb405d882f57222c1b1e23.png)


### `npm-import-package-version`

显示导入包的版本，能够在开发过程中就清晰的知道引入包的版本信息

![在这里插入图片描述](https://img-blog.csdnimg.cn/5ff6e942be2148fead15c4d52c04b0e1.png)


### `Indent Rainbow`

在编写代码中提供缩进更容易阅读和编写代码

可以方便的看到每个括号的起始位置，便于开发

![在这里插入图片描述](https://img-blog.csdnimg.cn/b7d66af7f5ab4afc8f423495f0272ba9.png)


### `WekaTime`

这个插件能够从你的编程活动中自动生成的指标、数据和时间跟踪

最基础的就是能够快速看到每天编程的时间，这能让你多休息，多注意身体，少内卷🙃

![](https://img-blog.csdnimg.cn/c876c7de80084635ad71bb3b7060afa3.png)

其他数据则可以去[wakatime官网查看](https://wakatime.com/)

![在这里插入图片描述](https://img-blog.csdnimg.cn/a0667b3b95b34bb8827822326ca59bf2.png)


### `Trailing Spaces`

这个插件能够高亮标识出你末尾多余的space，并帮你快速删除它们

![在这里插入图片描述](https://img-blog.csdnimg.cn/336a44197f1c40c490286bdb9d522720.png)


### `TypeScript Importer`

开发过程中能够快速找到你声明的类型并自动导入你所需要类型

对于TS的开发者，这能过提高不少开发效率

![img](https://img-blog.csdnimg.cn/img_convert/1ad2ed58384697ae9781c9af88fb5222.gif)

### `Path Intellisense`

路径自动寻找，可以在引入文件时根据提示能过快速智能的找到下一级路径
![IDE](https://img-blog.csdnimg.cn/img_convert/75829e8e80f39db37f6009ca5b3b1de2.gif)


### `Npm Intellisense`

导入包的时候能够智能的寻找到你可能需要引入的包

![在这里插入图片描述](https://img-blog.csdnimg.cn/b93067200c2642dcaa7c8470a962a2cb.png)


### `Path Autocomplete`

开发过程中为你导入的包提供路径补全，当你不知道这个文件夹下面有哪些文件夹是也可以给你提示

![在这里插入图片描述](https://img-blog.csdnimg.cn/eecc3d22a93a44f897de3dae223df8d0.gif#pic_center)


### `Image preview`

这个插件可以在开发区左侧和hover的时候能够快速看到展示的图片

在css文件中，当然hover到路径上也是能够显示的

![在这里插入图片描述](https://img-blog.csdnimg.cn/5ed209cf340f4b229f26634962ddedd4.png)


在typescript文件中

![在这里插入图片描述](https://img-blog.csdnimg.cn/4c87e015f00d4731b8e42d1c2dd610b4.png)


### `GitLens`

![在这里插入图片描述](https://img-blog.csdnimg.cn/f473e91d594842f4a0eec9247a4c2ec7.png)

`GitLens` 安装后，可以在编辑器中，查看git日志，尤其是谁提交的代码，便于讨论（甩锅）

安装后可以直接在代码里看到谁在什么时候提交了代码，很直观很方便

![在这里插入图片描述](https://img-blog.csdnimg.cn/389f292275a54adc864c2e14aa721d83.png)


### `Git History`

通过 `Git History` 展示 git 提交历史记录

![在这里插入图片描述](https://img-blog.csdnimg.cn/f2671b661c47471bbc4cf9660b55d863.png)

可以通过view log看到对应文件的history

![在这里插入图片描述](https://img-blog.csdnimg.cn/07ba0c6a18544f2e944137f46c1938e5.png)

可以通过`Previous`看到文件commit的修改历史

![在这里插入图片描述](https://img-blog.csdnimg.cn/722e3c5fa8d94e74b837c39d831d476d.png)


### `File Nesting Updater`

自动更新文件夹中的配置文件的嵌套配置，看下图你会更加清晰

![在这里插入图片描述](https://img-blog.csdnimg.cn/5c52d4a3a53f4cd79973e44e4fe659f5.png)


### `Error Lens`

对开发过程中的错误、警告⚠️和其他语言诊断进行高亮显示

非常有利于开发过程中进行快速debug

![在这里插入图片描述](https://img-blog.csdnimg.cn/1b820c27737247b48b5f57fed5e75c31.png)


### `CodeMetrics`

帮助你计算你写的代码的复杂度，比如function函数的复杂度

你写完代码就能看到函数的复杂度，它会提示你函数是否需要抽离模块出来

当然目前只能在 Typescript、Jacascript和Lua文件里使用

![在这里插入图片描述](https://img-blog.csdnimg.cn/cc3e0a6352ec4fee907d3669fe12981f.png)


### `Dash`

`Dash`是一个API 文档浏览器和代码片段管理器，目前只能在macOS里使用

![在这里插入图片描述](https://img-blog.csdnimg.cn/89fd35c14b6042d2830ad3ec07d3e9f5.png)

当然它不仅仅支持前端相关的文档，还支持其他语言以及框架的文档

![在这里插入图片描述](https://img-blog.csdnimg.cn/483b52d057be47a289d372305e42b9bb.png)


### `Better Comments`

通过使用警报、信息、TODO 等进行注释来改进您的代码注释！

![在这里插入图片描述](https://img-blog.csdnimg.cn/2fa19eae424c436ca6c3897241931bb9.png)

对应不同的注释信息会有不同的高亮显示，能过让你一眼就找到你的注释信息

![在这里插入图片描述](https://img-blog.csdnimg.cn/f6601f44e4634cbeab664fbe9e36bc2f.png)


### `TODO Highlight`

`TODO Highlight`突出显示代码中的 `TODO`、`FIXME` 和其他注释，和`Better Comments`功能相似，高亮显示略微不同

![在这里插入图片描述](https://img-blog.csdnimg.cn/a5491f887d494aeab51e5ab8f57f6d93.png)


### `Todo Tree`

`Todo Tree`这个插件可以在`activity bar`中有对应的图标，图标里面有项目的树状图，树视图中显示 `TODO`、`FIXME` 等评论标签，对于一些未来需要做的事情比较容易找到

![在这里插入图片描述](https://img-blog.csdnimg.cn/7c19a08fe2d04ff5a14778dc7bbe60ca.png)


### `Code Spell Checker`

`Code Spell Checker`是一个源代码拼写检查器，能过快速帮你检查出你的单词拼写错误并给出相应的建议

![在这里插入图片描述](https://img-blog.csdnimg.cn/cf1c1a1dafce4a14a95ba2a252a467c3.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/9c032b4e468146d49d42c6fa5af528a8.png)


### `Color Hightlight`

`Color Hightlight`会在编辑器中高亮颜色，能够一眼就看出你使用的颜色信息

![在这里插入图片描述](https://img-blog.csdnimg.cn/fa89888d32ed4eef8e926bbd6e7adc8b.png)

未使用`Color Hightlight`插件的效果如上图

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-nae39Rqu-1669209860800)(https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cedcf78f435a4d938e4bf14a4323e745~tplv-k3u1fbpfcp-watermark.image?)]
使用`Color Hightlight`插件后的效果如上图
![在这里插入图片描述](https://img-blog.csdnimg.cn/6b1a605f45dd45d695d2a3118df3278b.png)


### `CodeSnap`

`CodeSnap`插件可以 为您的代码截取漂亮的屏幕截图📷

![在这里插入图片描述](https://img-blog.csdnimg.cn/1d1470fca8dc4d61991103c76892a7b2.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/79201124ebed4a78a4cac9c2ed1e2c2a.png)


### `Parameter Hint`

`Parameter Hint`这个插件可以提供自动参数提示，比如函数会提示你参数的类型以及name字段等

![在这里插入图片描述](https://img-blog.csdnimg.cn/ec59001039884f7a897a73989c9d71f3.png)


### `Eslint`

`Eslint`插件用来规范代码风格，检测代码是否符合指定的规范

![在这里插入图片描述](https://img-blog.csdnimg.cn/54ec499ec57e493981230c24f682c651.png)


### `Prettier - Code formatter`

`Prettier`是代码格式化工具，快速按照指定的风格进行格式化

![在这里插入图片描述](https://img-blog.csdnimg.cn/78bd55937c7a472fb22b910b1ab0df85.png)


如果与`eslint`使用有冲突可以看下[eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier)

### `Project Manager`

对于项目管理比较有帮助，它能够帮助你快速切换到其他项目中去

![在这里插入图片描述](https://img-blog.csdnimg.cn/a651cf780dab4982844b9cfab19c3d89.png)


### `TypeScript Hero`

`TypeScript Hero`根据惯例对导入进行排序和组织，并删除未使用的导入，安装后默认开启

![在这里插入图片描述](https://img-blog.csdnimg.cn/b11c71f6959849a48aea2a7109d6dfd5.png)


### `TypeScript Extension Pack`

`TypeScript Extension Pack`插件是关于Typescript的扩展包（包含一些受欢迎的插件），它包含了[TypeScript Hero](https://marketplace.visualstudio.com/items?itemName=rbbit.typescript-hero)、[json2ts](https://marketplace.visualstudio.com/items?itemName=GregorBiswanger.json2ts)、[Move TS](https://marketplace.visualstudio.com/items?itemName=stringham.move-ts)、[Path Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense)、[TypeScript Importer](https://marketplace.visualstudio.com/items?itemName=pmneo.tsimporter)、[Prettier - JavaScript formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)等插件


![在这里插入图片描述](https://img-blog.csdnimg.cn/6cb846d19dbc4c95b63de1be3b73f82b.png)


### `TypeScript Vue Plugin (Volar)`

`Volar`插件是Vue3官方指定的开发插件，Vuer都懂


![在这里插入图片描述](https://img-blog.csdnimg.cn/f5d67dddece2411589eebd5f71d42afc.png)


其次建议大家开启`Volar Takeover` 模式，`Volar`能够使用一个 TS 语言服务实例同时为 Vue 和 TS 文件提供支持
具体问题和相关文档看[Vue的Volar文档](https://cn.vuejs.org/guide/typescript/overview.html#volar-takeover-mode)

### `Vue 3 Snippets`

要想快速开发vue项目，怎么能少得了`Vue Snippets`呢，这个插件可以帮助你快速开发，Vuer大赞

![在这里插入图片描述](https://img-blog.csdnimg.cn/453008655cf64084bdafd83c3fe74ba9.png)


同时对Vue3有更加具体的需要的话可以看看我的插件`Vue 3 Snippets for Vscode`，都是关于Vue3的并且支持很多模板语法和`Vue Router`和`Vuex`的语法，后续也会更新其他的比如`Pinia`以及`Nuxt`

![在这里插入图片描述](https://img-blog.csdnimg.cn/546ac14299584e38a573995c5b7fd468.png)


## `主题相关插件`

市面上主题插件就比较多了，我个人一直在用`Dracula Official`，个人觉得还是比较好用

### `Dracula Official`


![在这里插入图片描述](https://img-blog.csdnimg.cn/d747ac239ee04d868236dd9cdbda232c.png)


### `SynthWave '84`


![在这里插入图片描述](https://img-blog.csdnimg.cn/88028f9132da47d88aef96a986da2aad.png)


### `Radical`

![在这里插入图片描述](https://img-blog.csdnimg.cn/751fc333324c4a8f8951064ab36574c9.png)


### `GitHub Theme`

![在这里插入图片描述](https://img-blog.csdnimg.cn/b9a2bc90a1e34bdba8ac2afdd538dd30.png)


### `Gruvbox Material`

![在这里插入图片描述](https://img-blog.csdnimg.cn/7188bb139571439d97889283734f30f4.png)


## `文件图标插件`

我个人用的是`Material Icon Theme`，感觉都差不多，主要是能标识对应的文件和文件夹就行

### `Material Icon Theme`

![在这里插入图片描述](https://img-blog.csdnimg.cn/e0557879cbbc4a1090a02eb671fd934a.png)


### `vscode-icons`


![在这里插入图片描述](https://img-blog.csdnimg.cn/8a2b240b29034c619487c5f6b7601228.png)


### `VSCode Great Icons`


![在这里插入图片描述](https://img-blog.csdnimg.cn/be70638a6e1144f981a5afb545980f4f.png)


`其他`

### `A-super-translate`

对英语掌控不好的人比较适用

 -   翻译识别代码中注释部分，不干扰阅读。支持不同语言，单行、多行注释、
 -   支持用户字符串与变量翻译,支持驼峰拆分
 
![在这里插入图片描述](https://img-blog.csdnimg.cn/a02caab730094bf189647832d3f33096.png)


### `Draw.io Integration`

Draw.io的官方插件，支持在`VSCode`中画图，支持多人协作编辑图表

![在这里插入图片描述](https://img-blog.csdnimg.cn/325ff547062942d3ab1f951c2f877784.gif#pic_center)


### `LeetCode`

`LeetCode` 官方刷题插件，`vscode`刷题你值得拥有

![demo](https://img-blog.csdnimg.cn/img_convert/096f3693bb0e21847a1d96c306a1363b.gif)

### `Learn Vim`

`vscode`里学习 `Vim` ，让你快速成为键盘手

![在这里插入图片描述](https://img-blog.csdnimg.cn/e2b80f34606a4d2a9d16d6dee8163918.png)


### `Emoji`

从命令面板中找到并插入表情，喜欢用表情的同学可以试试

![emoji](https://img-blog.csdnimg.cn/img_convert/98f3488046bf6c3d1806dc469c4fed12.gif)

## `最后的话`
以上这些插件，如果对你有用的话，不妨点赞收藏关注一下，谢谢 🙏

大家有更好的插件，也欢迎补充
