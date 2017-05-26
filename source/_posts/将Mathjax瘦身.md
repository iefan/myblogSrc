---
title: 将Mathjax瘦身
date: 2016-07-25 18:15:30
tags: [Mathjax, Grunt, Node.js]
category: 程序
---
### 缘由
由于题库要打包运行，Mathjax 实在太大，所以需要进行瘦身。
<!-- more -->

### 做法
经过查询，在[Mathjax 瘦身记](https://segmentfault.com/a/1190000003822609)这篇文章中找到相关的方法，下面记录如下：

* 下载[MathJax-grunt-cleaner](https://github.com/mathjax/MathJax-grunt-cleaner)
* 将其中的 `Gruntfile.js` 和 `package.json` 复制进下载好的 Mathjax 文件根目录下。
* 在 Node.js 环境下安装 Grunt:
```
npm install -g grunt-cli
```
* 在 Mathjax 根目录下运行
```
npm install
```
则npm包管理器即可根据 package.json下载安装相关折包。
* 修改 `Gruntfile.js` 文件中的`grunt.registerTask("template"`部分，可以参考：
```
// **Notes** on the template. When instructions say "Pick one", this means commenting out one item (so that it"s not cleaned).
//
//      Early choices.
"clean:unpacked",
//"clean:packed", // pick one -- packed for production, unpacked for development.
//"clean:allConfigs", // if you do not need any combined configuration files.
//      Fonts. Pick at least one! Check notes above on configurations.
"clean:fontAsana",
"clean:fontGyrePagella",
"clean:fontGyreTermes",
"clean:fontLatinModern",
"clean:fontNeoEuler",
"clean:fontStix",
"clean:fontStixWeb",
//"clean:fontTeX",
//      Font formats. Pick at least one (unless you use SVG output; then clean all).
//"clean:dropFonts", // when using SVG output
"clean:eot",
//"clean:otf",
"clean:png",
"clean:svg",
"clean:woff",
//      Input. Pick at least one.
"clean:asciimathInput",
"clean:mathmlInput",
//"clean:texInput",
//       Output
//"clean:htmlCssOutput",
"clean:mathmlOutput",
"clean:svgOutput",
// Extensions. You probably want to leave the set matching your choices.
"clean:extensionsAsciimath",
"clean:extensionsMathml",
//"clean:extensionsTeX",
"clean:extensionHtmlCss",
// Other items
"clean:locales",
"clean:miscConfig",
//        "clean:miscExtensions", // you probably want that
"clean:images",
"clean:notcode"
```
其中不需要的注释即可。
* 在 Mathjax 根目录下，运行下列命令即可清除(`grunt`命令在上面步骤中已经安装)：
```
grunt template
```

**版权所有，欢迎转载，转载请注明出处**
