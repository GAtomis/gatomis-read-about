<!--
 * @Description: love Artemis
 * @Author: Gavin
 * @Date: 2022-05-30 14:28:52
 * @LastEditTime: 2022-12-19 14:14:08
 * @LastEditors: GAtomis 850680822@qq.com
-->
# Chapter-II 框架设计核心要素




### 虚拟dom性能
#### 声明式代码相比指令式能带来的优势
答案在P7

#### 虚拟dom最小化差异更新
答案在P6

### tree-shaking（P16）
做好这个特性需要
1. 支持esm模式
2. 建立DEV模式和prod模式的区别
3. ENV的统一规划

### 从框架输出产物（P18）
导出文件格式
1. iife模式通过立即执行函数去声明全局变量(工程化工具需要配置)
2. esm模式现代浏览器模块模式(同上)
3. 工程化工具读取package.json核心文件 module大于min指向,一般是给webpack或rollup读取资源使用
 