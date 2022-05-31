<!--
 * @Description: love Artemis
 * @Author: Gavin
 * @Date: 2022-05-30 14:28:52
 * @LastEditTime: 2022-05-31 11:42:23
 * @LastEditors: Gavin
-->
# Chapter-II 框架设计核心要素


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
 