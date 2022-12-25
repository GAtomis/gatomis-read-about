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
接着上期内容，生产和开发环境如何通过技术手段去做减重,接下来会是框架设计的一些小细节以及需要注意的地方。

[「每日一读」Vuejs设计与实现 第二章-框架设计核心要素-上](https://juejin.cn/post/7179063307653873721)


# Chapter-II 框架设计核心要素-下


## Topic
1. 特性开关
2. 正确的错误处理
3. typescript的支持



### 特性开关
框架开发中常见的3种问题
* 迭代更新
* 更新后的 breakchange
* 运行速度和用户灵活度

面对遗留且重要的特性时，开发者往往很难处理这些问题，特性开关概念设定就是解决这个问,下面是书中vue3中源码的举例
```
__FEATURE_OPTION_API__:isBundlerESMBuild?`__VUE_OPTIONS_API__`:true
//support for 2.x options
if(__VUE_OPTIONS_API__){
   option_api的一些操作

}
```
当vue构建时候,用户通过自己在框架暴露在用户层的配置文件配置相对的配置或者环境变量,选择最适合自己的项目的兼容版本,如果不需要breakchange后上一版本的重要特性可以通过开发者暴露的配置去进行打包方向的优化,Tree-shaking通过这些配置去清除一些不必要的deadcode,从减少打包体积。


### 错误处理
错误处理在日常开发中也是很常见,特别是js特性动态且运行时执行的脚本语言,且对错误的预估无法于java,golang这种静态语言去比较,所以处理错误显得尤为重要。框架处理错误的逻辑以及机制直接决定了用户应用的开发速度以及代码健壮性。
直接上书中的代码
```
//a.js
export default {
    foo(fn){
        fn&&fu()//callback函数
    }
}
//index.js
import foo from './a.js'
foo(()=>{
     let num=1
     num.trim()//代码出错
)

```
用户层面解决方案
```
//index.js
import foo from './a.js'
foo(()=>{

try{
     let num=1
     num.trim()//代码出错
     }catch(e){
     //处理出错
     
)

```
这样会很麻烦且同时增加了用户的心智负担和代码的可维护性,如果换一种思路通过框架内部封装方法来进行错误提示，就可以完美规避以上问题

框架层面解决
```
//a.js
import  app from 'utils.js'
export default {
    foo(fn){
         app.callWaitErrorHandling(fn)
    }
    bar(fn){
         app.callWaitErrorHandling(fn)
    
    }
    //自定义错误提示
    changeError(cb){
       app.customErrorHandle=cb
    }
}
//utils.js

export default {
    function callWaitErrorHandling(fn){
        try{
         fn&&fu()//callback函数

        }catch(e){
          customErrorHandle(e)
        }
    }
//错误处理
    function customErrorHandle (e){
            console.warn(e)
    }
}
```
通过上面的代码优化后,错误在用户层完全由用户掌握,且可以通过暴露的api去自定修改错误的处理提示等......

### Typescript支持
Typescript想必大家不陌,作为js的超集,近几年一直受到各大热门库以及框架热爱,包括react,vue3,threejs等主流库都离不开他的身影，现在越来越多的开发团队都在使用这门语言（包括我）,
与js动态特性不同,ts能为js支持很多静态类型,从而使得我们在多人开发中避免很多低级别的bug。更多详情内容可以去ts中的[官方网站](https://www.tslang.cn/)了解并学习。




## 总结
特性开关通过插拔方式很好的解决用户在对框架以往版本的的某些特性进行按需打包需求，把自己不需要的代码通过tree-shaking方式排出在自己的应用外。

框架的错误处理决定了应用代码的健壮性以及开发速度，通过框架暴露自己定义封装错误方法,可以减重不必要的重复劳动工作。

使用typescript让自己的框架的得到更加优化的类型支持。
### 关于我
一部分demo和笔记我已经放在了
[链接](https://github.com/GAtomis/gatomis-read-about/tree/main/VueJS%E8%AE%BE%E8%AE%A1%E4%B8%8E%E5%AE%9E%E7%8E%B0-hcy)上
以上内容是我对这本书的一些自己的感悟和见解，如果有错误，希望大家努力开喷，赶快纠正我的错误。
