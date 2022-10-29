---
title: 正则表达式快速入门
date: 2022-10-28T20:00:00.000+00:00
lang: zh
duration: 30min
description: 讲解了正则表达式的相关匹配原理，基础知识，以及匹配模式
---

## **1.前言**

我们为什么要使用正则表达式？

正则表达式在编程中是一个很重要的工具，它可以用来匹配字符串，也可以用来替换字符串，还可以用来分割字符串。正则表达式的语法很复杂，但是只要掌握了基本的语法，就可以应用到很多的场景中。

但是在前端开发中，正则表达式的应用场景并不多，因为String对象提供了很多的方法可以处理字符串，所以平时对于正则表达式的使用偏薄弱，学习之后容易忘记，今天就简单理解下正则表达式的知识。
从以下几个方面进行讲解：

![文章主题](https://img-blog.csdnimg.cn/2812fe9ac82c407cabbf51da56ba3091.png)


## **2.正则表达式的两种匹配模式**

### **1.匹配位置**

什么是位置？位置就是相邻字符串之间的位置，当然也包括开头或者结尾。

![拆解字符串](https://img-blog.csdnimg.cn/e2b7bf48909444d089d1150fd3bccd31.png)

匹配位置的意思就是说对这个字符串的位置进行匹配，比如说匹配字符串的开头、结尾、或者任意某个位置。***下面的示例如果不理解多参考这张图来配合理解***

-   `^`: 匹配(单行/多行)字符串的**开头**

`/^A/` 并不会匹配 "an A" 中的 'A'，但是会匹配 "An E" 中的 'A'

-   `$`: 匹配(单行/多行)字符串的**结尾**

`/t$/` 并不会匹配 "eater" 中的 't'，但是会匹配 "eat" 中的 't'。

```
const result = 'hello world'.replace(/^|$/g, '*');

// *hello world*
```

上述代码执行可以看到开头和结尾的位置被*替换了

-   `\b`: 匹配**单词的边界**

一个词的边界就是一个词不被另外一个“字”字符跟随的位置或者前面跟其他“字”字符的位置，简单说就是`\w`与`\W`之间的位置，当然包括`\w`与`^`和`$`的位置，比如上图中的h前面就是单词边界

```
const result = 'hello world'.replace(/\b/g, '*');

// *hello* *world*
```

-   `\B`: 匹配**非单词边界**

非单词边界就好理解了，除了单词边界外的都是非单词边界，比如上图中的h与e之间就是非单词边界

```
const result = 'hello world'.replace(/\B/g, '*');

// h*e*l*l*o w*o*r*l*d
```

-   `(?=p)`: 匹配**p前面**的位置

[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-lookahead)上称为先行断言，即匹配p前面的位置

```
const result = 'hello world'.replace(/(?=e)/g, '*');

// h*ello world
```

-   `(?!p)`: 匹配**后面不是p**的位置

[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-lookahead)上称为正向否定查找，即匹配后面不为p的位置

```
const result = 'hello'.replace(/(?!e)/g, '*');

// *he*l*l*o*

const result = 'hello world'.replace(/(?!e)/g, '*');

// *he*l*l*o* *w*o*r*l*d*
```

-   `(?<=p)`: 匹配**p后面**的位置

[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-lookahead)上称为后行断言，即匹配p后面的位置

```
const result = 'hello world'.replace(/(?<=e)/g, '*');

// he*llo world
```

-   `(?<!p)`: 匹配**前面不是p**的位置

[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-lookahead)上称为反向否定查找，即匹配前面不为p的位置

```
const result = 'hello world'.replace(/(?<!e)/g, '*');

// *h*el*l*o* *w*o*r*l*d*
```

以上四个记忆起来比较困难，我们可以理解包含`?=`就是断言(肯定)查找而包含`?!`就是否定查找，然后就是有`<`就是找后面的位置（后面位置通过<指向前面的括号内容）

### **2.匹配字符**

匹配字符比较好理解，就得对hello里面的字符进行匹配，比如/hello/则是对字符的全量匹配

其实字符匹配有两种匹配模式**横向匹配**和**纵向匹配**

1.  #### 横向匹配

横向匹配包括特殊字符里的各种量词

`{m,n}`: 匹配前面的字符至少 m次，最多 n 次，**注意,前后不需要空格**

`{n}`: 匹配了前面一个字符刚好出现了 n 次

`{n,}`: 匹配前一个字符至少出现了 n 次

`?`: 匹配前面一个表达式 0 次或者 1 次。等价于 `{0,1}`

`+`: 匹配前面一个表达式 1 次或者多次。等价于 `{1,}`

`*`: 匹配前一个表达式 0 次或多次。等价于 `{0,}`

```
const string = '12345678';

const reg = /\d{2,5}/;

const result = string.match(reg);

// '12345'
```

上述{2,5}会直接匹配5个呢？因为它是贪婪的，也就是说会尽可能的多匹配，有5个我就不匹配2个

那怎么设置成惰性匹配模式呢？

很简单，量词后面加个?，问一问你是不是很贪心？是不是够了？比如/\d{2,5}?/

**紧跟在任何量词** ***、 +、? 或 {}** **的后面**，将会使量词变为**惰性**

2.  #### 纵向匹配

纵向匹配可以理解为具体到某个字符时它不是确定的匹配字符而是有多种组合的可能

```
const string = '12356 12456 125889';

const reg = /\d+[345]/g;

const result = string.match(reg);

// [ '1235', '1245', '125' ]
```

这里的[345]是纵向上的匹配，比如组合可以是`\d+3` `\d+4` `\d+5`三种匹配模式

3.  #### 多选分支

横向匹配和纵向匹配都是一个字模式的匹配，而多选分支则是多个子模式任选其一

`(p1|p2|p3)`，其中`p1`、`p2`和`p3`是子模式，用`|`（管道符）分隔，表示其中任何之一。

```
const string = '1234';

const reg = /(\d+3)|(\d+4)/g;

const result = string.match(reg);

// [ '123' ]
```

**分支匹配是一个惰性模式匹配**，类似于js的语法，前面的匹配上了后面就不匹配了

如果记不住那有空就去[learn-regex](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)上练练手。

## 3.什么情况使用括号

1.  分组匹配，进行一组模式的匹配

```
const string = 'tststs';

const reg = /(ts)+/;

const result = string.match(reg);

// match[0]: tststs
```

2.  分支选择，进行分支可选匹配

```
const string = 'tsx';

const reg = /(ts|js)x/;

const result = string.match(reg);

// match[0]: tsx
```

3.  引用分组，常用于拿到分组数据进行操作

```
const string = 'key=value';

const reg = /(\w+)=(\w+)/;

const result = string.replace(reg, ( $ , $1, $2) => {

  return $2 + '=' + $1;

})

console.log(result); // value=key
```

4.  反向引用，可以在正则表达式中引用前面的分组进行相同匹配

```
const string = 'key=value key=value';

const reg = /(\w+)=(\w+)\s\1=\2/;

const result = reg.test(string);

console.log(RegExp.$1) // key

console.log(RegExp.$2) // value
```

5.  非捕获分组，匹配 'x' 但是不记住匹配组/(?:x)/，不记住匹配组就无法使用反向引用，也不会在API里引用

```
// 匹配分组

const string = 'tststs';

const reg = /(ts)+/

const result = string.match(reg);

// result: [ 'tststs', 'ts', index: 0, input: 'tststs', groups: undefined ]

// 非匹配分组

const string = 'tststs';

const reg = /(?:ts)+/

const result = string.match(reg);

// result: [ 'tststs', index: 0, input: 'tststs', groups: undefined ]
```

4.  ## 如何读写正则表达式

<!---->

1.  ### 读懂正则表达式

要读懂正则表达式写的是什么，那就要理解正则表达式的结构和操作符

结构有哪些呢？

```
字面量，匹配一个具体字符，包括不用转义的和需要转义的。比如a匹配字符"a"，又比如\n匹配换行符，又比如.匹配小数点。

字符组，匹配一个字符，可以是多种可能之一，比如[0-9]，表示匹配一个数字。也有\d的简写形式。另外还有反义字符组，表示可以是除了特定字符之外任何一个字符，比如[^0-9]，表示一个非数字字符，也有\D的简写形式。

量词，表示一个字符连续出现，比如a{1,3}表示“a”字符连续出现3次。另外还有常见的简写形式，比如a+表示“a”字符连续出现至少一次。

锚点，匹配一个位置，而不是字符。比如^匹配字符串的开头，又比如\b匹配单词边界，又比如(?=\d)表示数字前面的位置。

分组，用括号表示一个整体，比如(ab)+，表示"ab"两个字符连续出现多次，也可以使用非捕获分组(?:ab)+。

分支，多个子表达式多选一，比如abc|bcd，表达式匹配"abc"或者"bcd"字符子串。

反向引用，比如\2，表示引用第2个分组。
```

操作符有哪些？

```
1.转义符 \

2.括号和方括号 (...)、(?:...)、(?=...)、(?!...)、[...]

3.量词限定符 {m}、{m,n}、{m,}、?、*、+

4.位置和序列 ^ 、$、 \元字符、 一般字符

5. 管道符（竖杠）|

// 以上优先级从上往下排列
```

看一个例子🌰（网上找的https://segmentfault.com/a/1190000009324194）

```
验证国内电话号码

0555-6581752、021-86128488

正则：/(^0[0-9]{3}-[1-9][0-9]{6}$)|(^0[0-9]{2}-[1-9][0-9]{7}$)/
```

-   因为竖杠“|”,的优先级最低，所以正则分成了两部分`^0[0-9]{3}-[1-9][0-9]{6}$`和`^0[0-9]{2}-[1-9][0-9]{7}$`，并且每部分都是括号为一组。
-   `^0[0-9]{3}-[1-9][0-9]{6}$` 首先按照优先级方括号[0-9]、[1-9]优先级更高，其次是量次{3}和{6}是前面的方括号[0-9]的数量，最后才是^、$和普通字符0
-   `^0[0-9]{2}-[1-9][0-9]{7}$`首先按照优先级方括号[0-9]、[1-9]优先级更高，其次是量次{3}和{7}是前面的方括号[0-9]的数量，最后才是^、$和普通字符0

[可视化](https://jex.im/regulex/#!flags=&re=(%5E0%5B0-9%5D%7B3%7D-%5B1-9%5D%5B0-9%5D%7B6%7D%24)%7C(%5E0%5B0-9%5D%7B2%7D-%5B1-9%5D%5B0-9%5D%7B7%7D%24))如下

![ 正则可视化](https://img-blog.csdnimg.cn/5bfc6eee24014682afd43c948ba18e1f.png)


2.  ### 如何写正则表达式

##### > IP地址匹配(**目标分组法**），适用于一个正则可以拆解为多组的情况

```
IP地址的长度为32位(共有2^32个IP地址)，分为4段，每段8位 用十进制数字表示，每段数字范围为0～255，段与段之间用句点隔开。

// 0.0.0.0 ～ 255.255.255.255
```

1.  分析正则由几部分组成，将其分组

可以看出这里是有四部分构成，并且每部分的构成相同，范围都在 0 ~ 255

```
(((...).){3}(...)）

或者

((...)（.(...)){3}）
```

2.  分析每部分的分组的规律，先把每一组的正则公式写出来，当然每个人拆解的规律不同，可能写出的正则也不同

```
// 首先可以拆解为两种情况

// 0-199

[0-1]?\d{1,2}

// 200-255

2[0-4]\d | 25[0-5]

// 完整的分组

/(2(5[0-5]|[0-4]\d))|[0-1]?\d{1,2}/
```

上面这组还可以另外一种方法分组

```
// 一位数 9 09 009 0{0,2}\d

// 两位数 09 19 [0-1]\d

// 三位数 100-199 1\d{2}

// 三位数 200-249 2[0-4]\d

// 三位数 250-255 25[0-5]
```

3.  将每组的正则公式按照要求组合成一个完整的正则表达式

```
// 前面三部分相同

(((2(5[0-5]|[0-4]\d))|[0-1]?\d{1,2}).){3}

// 最后一组不需要.

(((2(5[0-5]|[0-4]\d))|[0-1]?\d{1,2}).){3}((2(5[0-5]|[0-4]\d))|[0-1]?\d{1,2})

// 只匹配ip地址，需要考虑首尾

^(((2(5[0-5]|[0-4]\d))|[0-1]?\d{1,2}).){3}((2(5[0-5]|[0-4]\d))|[0-1]?\d{1,2})$
```

4.  测试正则表达式是否完整

```
// 一位 6.6.6.6

// 两位 10.10.10.12

// 两位以0开头 01.02.03.05

// 三位 250.245.255.251

// 三位带0 009.007.008.010

// 含0 127.0.0.1

// 首位 9.123.223.355
```

也可以到[可视化网站](https://jex.im/regulex/#!flags=&re=%5E(a%7Cb)*%3F%24)去看一下自己的逻辑是否正确，还有一个[ihateregex](https://ihateregex.io/expr/ip/)也会有相应的可视化（还有一些常见的正则表达式可用🐮）

![正则可视化](https://img-blog.csdnimg.cn/7496ab1542484621b26822ea3f3f0758.png)


##### > **数字的千位分隔符表示法（目标递进法），适用于一个目标，通过层层递进写出**

比如把"12345678"，变成"12,345,678"

1.  先把尾部,写出来

```
// (?=\d{3}$) 后面跟着是3个数字并且是处于结尾的

const str = '1234567890';

const reg = /(?=\d{3}$)/g

const result = str.replace(reg, ',');

console.log(result); // 1234567,890
```

2.  把其他位置的,弄出来

```
const str = '1234567890';

const reg = /(?=(\d{3})+$)/g

const result = str.replace(reg, ',');

console.log(result); // 1,234,567,890

// 123456789 -> ,123,456,789 存在问题，不能为首位
```

3.  首位不能为,

```
// 使用正向否定查找 x(?!y) ^代表的就是首位

const str = '123456789';

const reg = /(?!^)(?=(\d{3})+$)/g

const result = str.replace(reg, ',');

console.log(result); // 123,456,789
```

4.  支持其他形式 '123456789 987654321'

```
// 只需要把首位^和$使用单词边界\b替换即可

const str = '123456789 987654321';

const reg = /(?!\b)(?=(\d{3})+\b)/g

const result = str.replace(reg, ',');

console.log(result); // 123,456,789 987,654,321
```

5.  看看是否可以简化

```
// (?!\b)的意思就是匹配的这个位置后面不能是\b（不能是\b那么这个位置只能是\B）

// 所以(?!\b)=\B

const str = '123456789 987654321';

const reg = /\B(?=(\d{3})+\b)/g

const result = str.replace(reg, ',');

console.log(result); // 123,456,789 987,654,321
```

6.  测试正则表达式是否完整，继续使用其他字符串进行验证

![正则验证图](https://img-blog.csdnimg.cn/d11bf26399934b7b9edfce7b84657308.png)


##### > 常见正则表达式

1.以下是手机号的号段，大家可以思考下写出对应的正则表达式

中国电信号段133、153、173、177、180、181、189、191、193、199

中国联通号段130、131、132、155、156、166、175、176、185、186、166

中国移动号段134、135、136、137、138、139、147、150、151、152、157、158、159、172、178、182、183、184、187、188、198

```
const reg = /^1[3456789]\d{9}$/

const str = '18025031832'

const result = reg.test(str)

console.log(result)

// true
```

2.  从query上获取对应的key/value信息

```
const query = 'name=ego&type=entity&id=ego'

const reg = /([^&=]+)=([^&=]+)/g;

const result = query.match(reg);

console.log(result)

// [ 'name=ego', 'type=entity', 'id=ego' ]
```

## 5.正则表达式使用注意点

### 1.字面量与RegExp对象的区别

字面量：由斜杠 (/) 包围而不是引号包围比如/ab+c/

构造函数: 由引号而不是斜杠包围比如 new RegExp('ab+${c}', 'i')

区别：

字面量：脚本加载后，正则表达式字面量就会被编译，当你在循环中使用字面量构造一个正则表达式时，正则表达式**不会**在每一次迭代中都被重新编译，有点类似于静态编译

构造函数：在脚本**运行**过程中，用构造函数创建的正则表达式会被编译，比较适合动态的正则表达式，有点类似动态编译

如果是正则表达式是静态的则推荐使用字面量格式，动态变化的则使用构造函数形式

### 2.正则表达式的四种常见操作

1.  验证

验证就是看目标字符串里是否有满足匹配的子串

-   `test`

```
const reg = /\w+\d/;

const str = "abc123";

console.log(reg.test(str)); // true
```

-   `exec`

```
const reg = /[a-z]+\d/;

const str = "abc123string12345";

console.log(reg.exec(str)); // true
```

-   `match`

```
const reg = /[a-z]+\d/;

const str = "abc123";

const result = str.match(reg);

console.log(result);

// [ 'abc1', index: 0, input: 'abc123', groups: undefined ]
```

-   `search`

```
const reg = /[a-z]+\d/;

const str = "abc123";

const result = str.search(reg);

console.log(result); // 0

// -1代表未匹配
```

2.  切分

切分常用于切割目标字符串

```
const str = "2022-10-24";

const reg = /-/;

const result = str.split(reg);

console.log(result);

// [ '2022', '10', '24' ]
```

3.  提取

提取某段字符串里面的某部分数据

`match`

```
const reg = /([a-z]+)\d+([A-Z]+)/;

const str = "abc123ABC555";

const result = str.match(reg);

console.log(result);

// [

//   'abc123ABC',

//   'abc',

//   'ABC',

//   index: 0,

//   input: 'abc123ABC555',

//   groups: undefined

// ]
```

`exec`

```
const reg = /([a-z]+)\d+([A-Z]+)/;

const str = "abc123ABC555";

const result = reg.exec(str);

console.log(result);

// [

//   'abc123ABC',

//   'abc',

//   'ABC',

//   index: 0,

//   input: 'abc123ABC555',

//   groups: undefined

// ]
```

4.  替换

替换字符串里面的某段字符

`replace`

```
const reg = /(\w+)=(\w+)/;

const str = "key=value&key1=value1";

const result = str.replace(reg, ($, $1, $2) => {

  return `${$2};${$1}`;

});

console.log(result);

// value;key&key1=value1
```

`replaceAll`

```
const reg = /(\w+)=(\w+)/g;

const str = "key=value&key1=value1";

const result = str.replaceAll(reg, ($, $1, $2) => {

  return `${$2};${$1}`;

});

console.log(result);

// value;key&value1;key1

// replaceAll使用正则表达式时一定需要g标识符
```

6.  ## 简单了解正则匹配原理

正则匹配的核心原理-**回溯**

先来看一个简单的正则匹配

```
const calcRegExpTime = (input, reg) => {

    const start = Date.now();

    const res = reg.test(input);

    const timeStamp = Date.now() - start;

    console.log('speed: ', res, timeStamp);

}



const str = '1234567891212345678912345678s'

const reg1 = /^(\d+)*$/

calcRegExpTime(str, reg1)
```

大家认为上面这个正则匹配要花多少时间？

![匹配时间](https://img-blog.csdnimg.cn/2f1d6e2b9c2d4f9593a1c7821792322a.png)


在node里面执行花了11m，所以你以为的简单正则可能是一个性能很差的正则表达式

可以看一个简单的小视频，[地址传送](https://regex101.com)，这里就是简单的正则回溯

![正则演示](https://img-blog.csdnimg.cn/f7a609d214294301b451014c6e6b90c8.png)


由此可见，由于使用了b{1,3}横向匹配，所以中间有两次回溯，从b匹配3次到b匹配1次的回溯

而刚刚上面那个回溯的次数更是惊人（*这次的str比上面的短，所以时间上只花了5.1ms*）

![正则匹配次数](https://img-blog.csdnimg.cn/65dce52c5a0e4f43b5b9dc64899214ae.png)


解决问题的方法就是减少回溯

1.  ##### 少用贪婪模式，多用惰性模式

惰性匹配会尽可能少地重复匹配字符。如果匹配成功，它会继续匹配剩余的字符串。

比如/ab{1,3}?bbc/去匹配'abbbc'的话只需要6步就能完成，它会首先选择最小的匹配范围，即匹配 1 个 b 字符，因此就避免了回溯问题。

2.  ##### 使用具体的元字符、字符类（\d、\w、\s等） ，少用”.”字符，越模糊匹配越耗费的时间越长
2.  ##### 减少分支的数量，缩小范围

用`ab(cd|ef) 代替 (abcd|abef)`

因为匹配时会自动去尝试匹配 ab，如果没有找到，就不会再尝试任何选项。

4.  ##### 减少不需要获取的分组，即尽可能使用非捕获分组

非捕获分组不但能够节省捕获的时间，而且会减少回溯使用的状态的数量。由于捕获需要使用内存，所以也减少了内存的占用
