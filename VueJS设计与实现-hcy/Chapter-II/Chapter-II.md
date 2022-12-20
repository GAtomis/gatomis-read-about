<!--
 * @Author: GAtomis 850680822@qq.com
 * @Date: 2022-12-19 14:02:57
 * @LastEditors: GAtomis 850680822@qq.com
 * @LastEditTime: 2022-12-20 15:58:34
 * @FilePath: /workspace/gatomis-read-about/VueJS设计与实现-hcy/Chapter-II/Chapter-II.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
## 前言
最近重新翻了一下书堆发现一本绝世好书《vuejs设计与实现》——霍春阳，里面描述了vue框架的实现思路，也详细的介绍了vue各个模块设计实现思路，激发了我将它读下去的欲望，为了加强自制力（其实是懒），我会每天尽量分享我的读书笔记以及个人理解。如果大家感兴趣可以去商城买一本支持我们的霍春阳大大，这本书真的很好。
## 上期回顾
上期学习内容是从概念上定义一个声明式框架,这期的核心要素分上下两篇来整理(原谅我脑容量有限)

[「每日一读」Vuejs设计与实现 第一章-框架设计概念](https://juejin.cn/post/7178859127618142266)


# Chapter-II 框架设计核心要素


## Topic
1. 开发体验-错误提示
2. 建立DEV模式和prod模式的区别
3. Tree-shaking如何应对副作用
4. 框架构建的产物


### 开发体验-错误提示
在我们vue的开发模式中我们可以看到很多warn提示，这些提示可以让我们快速定位问题，一个良好的框架需要给用户一个很好的开发体验，所以对于
错误提示的是设计显的尤为重要
### 建立ENV模式，区分生产以及开发环境，控制代码体积
在我们的日常代码中经常会使用ENV常量来控制的代码,例如下面代码
```
if(__DEV__ &&num){
    //执行
    console.warn("执行内容");
}
```
以上__DEV__为常量当值为true的时候,这个提示报错才会触发，以此在开发环境给用户很好的提示,同时服务于生产和运行环境时框架大小就尤为重要,
当在运行环境中时__DEV__被视为false,也意味着上面的if代码块不会被执行,我们称这个代码块为dead code，它在最终的打包过程不会被框架打包,从而大大优化了打包后运行环境代码的体积。
### Tree-Shaking的支持
> Tree-Shaking和ESM

Tree-Shaking书上原话是-指的就是消除那些永远不会被执行的代码。也就是上面提到的dead code消除的最佳实践。Rollup，和webpack都支持这种模式,但是想要实现Tree-Shaking必须使用ESM引入模块方式,具体实例可以查看[demo](https://github.com/GAtomis/gatomis-read-about/tree/main/VueJS%E8%AE%BE%E8%AE%A1%E4%B8%8E%E5%AE%9E%E7%8E%B0-hcy/Chapter-II/Tree-Shaking)。

>副作用

由于JS动态语言的特性，在JS动态语言中顶层调用的代码有些会产生一些副作用,例如代理某些对象时的gethook中调用修改了某个环境变量的参数，这样在静态层面分析就变得不怎么牢靠,通过标记某个函数或者片段,让打包工具识别这个片段是不会产生副作用。
## 框架构建的产物
略...在书中p18页,有些可能通过自己语言无法去描述，就不献丑了。

## 总结
开发错误提示是对设计一个框架来说非常重要的内容，友好的错误提示，有利于开发者快速定位问题。
通过环境变量控制输出警告内容实现生产和开发模式分离,利用tree-shaking机制优化我们的代码,减少不必要的代码被打包如生产环境。
