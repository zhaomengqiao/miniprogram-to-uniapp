# 微信小程序转换为uni-app项目   
   
输入小程序项目路径，输出uni-app项目。
   
实现项目下面的js+wxml+wxss转换为vue文件，模板语法、生命周期函数等进行相应转换，其他文件原样复制，生成uni-app所需要的配置文件。   

   
        
## 安装   
   
```js
$ npm install miniprogram-to-uniapp -g
```
   
## 升级版本   
   
```js
$ npm update miniprogram-to-uniapp -g
```
   
## 使用方法

```sh
Usage: wtu [options]

Options:

  -V, --version     output the version number [版本信息]
  -i, --input       the input path for weixin miniprogram project [输入目录]
  -h, --help        output usage information [帮助信息]
  -c, --cli         the type of output project is vue-cli, which default value is false [是否转换为vue-cli项目，默认false]
  -w, --wxs         transform wxs file to js file, which default value is false [是否将wxs文件转换为js文件，默认false]

```

Examples:

```sh
$ wtu -i miniprogramProject
```

vue-cli mode [转换项目为vue-cli项目]:
```sh
$ wtu -i miniprogramProject -c
```

Transform wxs file to js file [转换项目并将wxs文件转换为js文件]:
```sh
$ wtu -i miniprogramProject -w
```

## 使用指南

本插件详细使用教程，请参照：[miniprogram-to-uniapp使用指南](http://ask.dcloud.net.cn/article/36037)。

对于使用有疑问或建议，欢迎加入QQ群：780359397进行讨论。


## 已完成   
* 支持有/无云开发的小程序项目转换为uni-app项目   
* 支持*.js', *.wxml和*.wxss文件进行相应转换，并做了大量的优化   
* 支持app.js、page和component生命周期函数的转换   
* 区分app.js/component，两者解析规则略有不同   
* 添加setData()函数于methods下，解决this.setData()【代码出处：https://ask.dcloud.net.cn/article/35020】  
* ~~App.vue里，this.globalData.xxx替换为this.$options.globalData.xxx(后续uni-app可以支持时，此功能将回滚，已回滚)~~   
* 支持wxs文件转换，可以通过参数配置(-w)，默认为false
* 支持vue-cli模式，即生成为vue-cli项目，转换完成需运行npm -i安装包，然后再导入hbuilder x里开发  
* 导出```<template data="abc"/>``` 标签为abc.vue，并注册为全局组件   
* 使用[uParse修复版](https://ext.dcloud.net.cn/plugin?id=364)替换wxParse   
* 搜索未在data声明，而直接在setData()里使用的变量，并修复   
* 合并使用require导入的wxs文件   
* 因uni-app会将所有非static目录的资源文件删除，因此将所有资源文件移入static目录，并修复所有能修复到的路径   
* 修复变量名与函数重名的情况   
* 支持解析include标签
   
   
## 更新记录   
### v1.0.33(20191125)   
* [修复] pages为空的bug    
* [修复] wxParseData不为xxx.nodes的形态，导致转换报错的bug    


### v1.0.32(20191125)   
* [新增] 支持```<include src="url"></include>```标签
* [优化] 更新babel版本   
* [优化] 漏网之鱼里对url的提示   
* [优化] 组件里Behavior转换为mixins   
* [优化] 重构转换流程来适应include标签转换   
* [优化] 变量名和函数名为JS关键字时，对其重命名   
* [优化] 不再推荐转换为vue项目(因为初始化太繁琐，让给有时间的人去折腾吧)   
* [优化] 更新u-parse，修复百度小程序解析img不显示、富文本解析不出来的bug   
* [优化] 重新转换时，不删除unpackage和node_modules目录，减少因文件占用而清空文件夹失败的机率   
* [修复] catchtap事件转换    
* [修复] 单引号含双引号的属性    
* [修复] app.vue也添加了组件的bug   
* [修复] 单独js文件里路径没有处理的bug   
* [修复] Vue.component()组件名对应不上的bug   
* [修复] url('{{}}xxx')含引号，导致转换错误的bug   
* [修复] 当package.json内容为空时解析报错的bug   
* [修复] wxParseData含三元/二元表达式的变量提取   
* [修复] ```var app = getApp(), http = app.http;```转换失败的bug   
* [修复] setData()时，props含此变量，但data里没有，而导致变量重复声明的bug(此问题未完美解决，因为props里变量不能setData)   

### v1.0.31(20191119)   
* [优化] setData()支持回调函数   
* [修复] 解析app时判断prop报错的bug   
* [修复] 一些小bug   

### v1.0.30(20191118)   
* [优化] 重构大部分代码，优化页面类型判断逻辑，app、page和component分别进行解析       
* [优化] 组件里生命周期的转换(ready-->mounted，ready-->mounted)、pageLifetimes、lifetimes、behaviors、externalClasses、relations和options等节点处理     
* [优化] 组件里observer和observers替换换为watch   
* [修复] 组件名同时包含驼峰和短横线转换时转换错误，导致找不到组件的bug(如```diy-imageSingle```应转换为```diyImageSingle```) 
* [修复] 替换wxParse时，参数里this的指向问题，以及优化wxParse代码的判断   
* [修复] 模板绑定里使用单引号包含双引号导致转换失败(如: ```<view style='font-family: "Guildford Pro";'></view>```)   
* [修复] props里默认值字段名value替换为default; 当type为Array或Object时，默认值使用工厂函数返回   
   
## [历史更新记录](ReleaseNote.md)   
    
## [关于不支持转换的语法说明](Unsupported.md)  


## 感谢   
* 感谢转转大佬的文章：[[AST实战]从零开始写一个wepy转VUE的工具](https://juejin.im/post/5c877cd35188257e3b14a1bc#heading-14)， 本项目基于此文章里面的代码开发，在此表示感谢~   
* 感谢网友[没有好名字了]给予帮助。   
* 感谢官方大佬DCloud_heavensoft的文章：[微信小程序转换uni-app详细指南](http://ask.dcloud.net.cn/article/35786)，补充了我一些未考虑到的规则。   
* this.setData()代码出处：https://ask.dcloud.net.cn/article/35020，在些表示感谢~  
* 工具使用[uParse修复版](https://ext.dcloud.net.cn/plugin?id=364)替换wxParse，表示感谢~
* 感谢为本项目提供建议以及帮助的热心网友们~~   
    
      
## 参考资料   
0. [[AST实战]从零开始写一个wepy转VUE的工具](https://juejin.im/post/5c877cd35188257e3b14a1bc#heading-14)   此文获益良多   
1. [https://astexplorer.net/](https://astexplorer.net/)   AST可视化工具   
2. [Babylon-AST初探-代码生成(Create)](https://summerrouxin.github.io/2018/05/22/ast-create/Javascript-Babylon-AST-create/)   系列文章(作者还是个程序媛噢~)   
3. [Babel 插件手册](https://github.com/jamiebuilds/babel-handbook/blob/master/translations/zh-Hans/plugin-handbook.md#toc-inserting-into-a-container)  中文版Babel插件手册   
5. [Babel官网](https://babeljs.io/docs/en/babel-types)   有问题直接阅读官方文档哈   
6. [微信小程序转换uni-app详细指南](http://ask.dcloud.net.cn/article/35786)  补充了我一些未考虑到的规则。   
7. 更新babel版本，命令：npx babel-upgrade --write
   
   
## 最后
如果觉得帮助到你的话，点个赞呗~

打赏一下的话就更好了~

![微信支付](src/img/WeChanQR.png)![支付宝支付](src/img/AliPayQR.png)


## LICENSE
This repo is released under the [MIT](http://opensource.org/licenses/MIT).
