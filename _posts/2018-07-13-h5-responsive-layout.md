---
ID: 300
post_title: 聊聊H5移动端适配方案
post_name: h5-responsive-layout
author: 盛闯
post_date: 2018-07-13 10:44:49
layout: post
link: >
  https://devops.vpclub.cn/h5-responsive-layout/
published: true
tags:
  - h5
  - layout
  - responsive
  - web
categories:
  - 前端
---


# 聊聊H5适配
关于移动端H5适配一直是个热门话题，尽管公司已经在用的那一套经过了市场的检验，但是现在vw、wh兼容性足够好了，所以我们可以开始使用更好的新适配方案了，接下来会列举三种适配方案，当然你也可以重组他们，个人推荐第三种，如果你是处女座，只能使用第三种。

>1. 以下适配都基于.html文件中添加了 `<meta name="viewport" content="width=device-width,initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">`
>这行代码简单来讲就是视口（`viewport`）的宽度为设备宽度，初始化缩放、最小缩放、最大缩放都为1倍，且不允许用户缩放。关于`viewport`知识点再论。 
>2. vm单位，在移动端 iOS 8 以上以及 Android 4.4 以上获得支持，并且在微信 x5 内核中也得到完美的全面支持。先补充一点vw、vh、vmin、vmax的知识：
>>* `vw` : `1vw` 等于`window.innerWidth`的1%
>>* `vh` : `1vh` 等于`window.innerHeihgt`的1%
>>* `vmin` :  `vw` 和 `vh` 中最小的那个
>>* `vmax` :  `vw` 和 `vh` 中最大的那个  

###### 1px边框（严格来讲≤1px单位）不建议使用以下适配方案，下次再聊。

## 通过js设置根元素的font-size  
目前我们公司基本都在用这套方案，直接上代码：  

```javascript
//rem.js
(function (document, window) {
	    /**
	     * 7.5=设计稿尺寸/100
	     * css元素尺寸=设计稿元素尺寸/100;
	     */
	    var change = 'orientationchange' in window ? 'orientationchange' : 'resize';
	    function calculate() {
			var deviceWidth = document.documentElement.clientWidth;
	        if (deviceWidth < 320) {
	            deviceWidth = 320;
	        } else if (deviceWidth > 750) {
	            deviceWidth = 750;
	        }
			document.documentElement.style.fontSize = deviceWidth / 7.5 + 'px';
		
	    };
		window.addEventListener(change, calculate, false);
		document.addEventListener('DOMContentLoaded', calculate, false);
		calculate();

})(document, window);
```
使用方法：
> 1. 将以上文件引入到项目中
> 2. 写css的时候用设计图的px尺寸/100。  
    这里是以设计稿为750x1334为例，例如设计稿中元素宽度为100，书写样式的时候就是`width: 1rem`。
> 3. 当然也可以用postcss来做。

## vm+rem+scss+postcss。
这种方式同样是采用rem的方式，区别第一种是根元素`font-size`采用`vw`。

```javascript
// rem.scss

$v_design: 750;// 750x1334设计稿
$v_fontsize: $v_design/10;

@function rem($px) {
     @return ($px / $v_fontsize ) * 1rem;
}

html {
    // 根元素font-size使用 vw 单位
    font-size: ($v_fontsize / ($v_design / 2)) * 100vw; 
    // 通过Media Query 限制根元素最大最小值
    @media screen and (max-width: 320px) {
        font-size: 64px;
    }
    @media screen and (min-width: 540px) {
        font-size: 108px;
    }
}

body {
    max-width: 540px;
    min-width: 320px;
    margin: 0 auto;
}
```
使用方法：
> 1. 引入`rem.scss`
> 2. `npm i postcss-pxtorem -D`
> 3. 修改`.postcssrc.js`文件，这样就可以愉快的用`px`啦。
> ```javascript
>   module.exports = {
>      "plugins": {
>          "postcss-pxtorem": {
>                "rootValue": 75,
>                "propList": ["*"]   
>            }
>       }
>   }

- 注意：不支持style中使用@import url()的方式。这种时候可以用定义的scss函数`rem()`  
- 优点：只需要通过改变根元素大小的计算方式，就可以无缝过渡到另一种CSS单位

## postcss-px-to-viewport

 直接通过[postcss-px-to-viewport](https://github.com/evrone/postcss-px-to-viewport)将`px`转换`vw`。  
 使用方法：
 > 1. `npm i postcss-px-to-viewport -D`
 > 2. 修改`.postcssrc.js`文件，这样就可以愉快的用`px`啦。
 > ```javascript
 >   module.exports = {
 >        "plugins": {
 >           "postcss-px-to-viewport": { 
 >               viewportWidth: 750,// 视窗的宽度，对应的是我们设计稿的宽度，一般是750 
 >               viewportHeight: 1334, // 视窗的高度，根据750设备的宽度来指定，一般指定1334，也可以不配置著作权归作者所有。
 >               unitPrecision: 3, // 指定`px`转换为视窗单位值的小数位数（很多时候无法整除）
 >               viewportUnit: 'vw', // 指定需要转换成的视窗单位，建议使用vw
 >               selectorBlackList: [], // 指定不转换为视窗单位的类，可以自定义，可以无限添加,建议定义一至两个通用的类名
 >               minPixelValue: 1, // 小于或等于`1px`不转换为视窗单位，你也可以设置为你想要的值
 >               mediaQuery: false //允许在媒体查询中转换`px`
 >          }
 >       }
 >   }
> ```
如果你不需要去管那种被程序员唾弃的，被时代抛弃的脑残机完全可以用这种方式。
## 改变meta标签
还有一种是通过改变`<meta>`标签来适配，因为限制性比较多，例如引入第三方库、嵌入第三方页面是有问题的，所以这里就不多做介绍，也不推荐大家使用这种方法。

```html
<!-- dpr = 1--> 
`<meta name="viewport" content="width=device-width,initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">`
<!-- dpr = 2--> 
`<meta name="viewport" content="width=device-width,initial-scale=0.5, minimum-scale=0.5, maximum-scale=0.5, user-scalable=no">`
<!-- dpr = 3--> 
`<meta name="viewport" content="width=device-width,initial-scale=0.333333, minimum-scale=0.333333, maximum-scale=0.333333, user-scalable=no">`
```












    
