+++
date = "2017-02-04 T15:20:58+08:00"
title = "ie兼容问题"
draft = true

+++
1. first
1. 最后兼容ie8  用的是这种写法

        background-color: rgba(0,0,0,0.6);
        filter: progid:DXImageTransform.Microsoft.gradient(startColorstr=#60000000,endColorstr=#60000000);
    \#60000000前两位为透明度，后面是色值， 渐变也这么写从xx颜色到xx颜色
2. 参考摘自[blog](http://blog.csdn.net/tenpage/article/details/33725909)
3. CSS [opacity介绍](https://developer.mozilla.org/en-US/docs/Web/CSS/opacity?redirectlocale=en-US&redirectslug=CSS%2Fopacity)

首先，Opacity属性用来设置一个元素的透明度，取值范围是0~1之间，不可为负值。
opacity取值为1是完全不透明，取值为0是完全透明，视觉上看不见。
关于浏览器对opacity属性的兼容性请继续往下看：

从Firefox3.5+不再支持私有属性-moz-opacity了，
在Mozilla 1.7 (Firefox 0.9)之前FF都是使用这个私有属性的，
Firefox 0.9-Firefox3同时支持-moz-opacity和opacity这两个属性，
现在回想起刚入职场不久那时候，正好是Firefox升级到3.5之后，
一些做好的页面透明效果突然没有了，如今已经CSS3铺天盖地，概叹时光荏苒啊。

IE9+才开始支持CSS3 opacity，而对IE6-IE8我们习惯使用filter滤镜属性来进行实现。
IE4-IE9都支持滤镜写法progid:DXImageTransform.Microsoft.Alpha(Opacity=xx).

IE8又引入了特殊的-ms-filter，IE认为这种写法是对旧写法的一次更正，更符合规范，
这个写法的属性值只是多了一对引号，效果同前。不过，这种写法的寿命也不长，
到IE10对filter与-ms-filter都已经不再支持。

Safari 1.2之前的版本，是基于khtml的浏览器内核，1.2版发布后，
不再支持-khtml-opacity的写法，-khtml-opacity也随之成为历史。

Konqueror从未支持过-khtml-opacity，从4.0版本开始已经支持opacity。

除IE外，目前主流浏览器 Opera 9.0+，Safari  1.2(WebKit 125) +，chrome等等都支持opacity这个透明度属性。

IE 从4.0版开始，就提供了一些内置的多媒体滤镜特效，具体的使用方法是：
语法：

filter : filter

参数：

filter : 　要使用的滤镜效果。多个滤镜之间用空格隔开。

说明：

设置或检索对象所应用的滤镜效果。
要使用该属性，对象必须具有height，width，position三个属性中的一个。
滤镜的机制是可扩展的。可以开发和使用第三方滤镜。
该属性在MAC平台上不可用。
对应的脚本特性为filter。

IE4.0以上版本，支持以下14种滤镜：

滤镜名    说明

Alpha     让HTML元件呈现出透明的渐进效果
Blur     让HTML元件产生风吹模糊的效果
Chroma     让图像中的某一颜色变成透明色
DropShadow     让HTML元件有一个下落式的阴影
FlipH     让HTML元件水平翻转
FlipV     让HTML元件垂直翻转
Glow     在元件的周围产生光晕而模糊的效果
Gray     把一个彩色的图片变成黑白色
Invert     产生图片的照片底片的效果
Light     在HTML元件上放置一个光影
Mask     利用另一个HTML元件在另一个元件上产生图像的遮罩
Shadow     产生一个比较立体的阴影
Wave     让HTML元件产生水平或是垂直方向上的波浪变形
XRay     产生HTML元件的轮廓，就像是照X光一样

Alpha 滤镜参数详解

参数名     说明     取值说明
Opacity     不透明的程度，百分比。    从0到100，0表是完全透明，100表示完全不透明。
FinishOpacity     这是一个同Opacity一起使用的选择性的参数，当同时Opacity和FinishOpacity时，可以制作出透明渐进的效果，比较酷。    从0到100，0表是完全透明，100表示完全不透明。
Style     当同时设定了Opacity和finishOpacity产生透明渐进时，它主要是用赤指定渐进的显示形状。    0：没有渐进；1：直线渐进；2：圆形渐进；3：矩形辐射。
StartX     渐进开始的 X 坐标值
StartY     渐进开始的 Y 坐标值
FinishX     渐进结束的 X 坐标值
FinishY     渐进结束的 Y 坐标值

下面通过一个例子来测试filter和opacity的兼容性：
Html代码
```
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset=utf-8 />
    <title>JS Bin</title>
    </head>
    <body>
      <div class="transparent_class">测试透明度</div>
    </body>
    </html>
````
注意：测试不要忘了写DOCTYPE，否则会偏离真实效果。
对应CSS代码：
```
    .transparent_class {
        /* Required for IE 5, 6, 7 */
        /* ...or something to trigger hasLayout, like zoom: 1; */
        width:300px;
        height:300px;
        line-height:300px;
        text-align:center;
        background:#000;
        color:#fff;
        /* older safari/Chrome browsers */
        -webkit-opacity: 0.5;
        /* Netscape and Older than Firefox 0.9 */
        -moz-opacity: 0.5;
        /* Safari 1.x (pre WebKit!) 老式khtml内核的Safari浏览器*/
        -khtml-opacity: 0.5;
        /* IE9 + etc...modern browsers */
        opacity: .5;
        /* IE 4-9 */
        filter:alpha(opacity=50);
        /*This works in IE 8 & 9 too*/
        -ms-filter:"progid:DXImageTransform.Microsoft.Alpha(Opacity=50)";
        /*IE4-IE9*/
        filter:progid:DXImageTransform.Microsoft.Alpha(Opacity=50);
    }
```
使用中，我们可以根据要适配的浏览器/版本，从上面选择自己需要的代码行。如果要全面支持所有浏览器，至少需要有关opacity或filter的前5句。
需要声明的是，如果你要同时使用filter和-ms-filter，请将-ms-filter写在filter的前面。原文描述如下：

If you want opacity to also work in IE8′s emulating IE7 mode, the order should be:

    [css] view plain copy
    -ms-filter:”progid:DXImageTransform.Microsoft.Alpha(Opacity=50)”; // first
    filter: alpha(opacity=50); // second
    If you don’t use this order, IE8 emulating IE7 doesn’t apply the opacity, although IE8 and IE7 native do.

基于统计的CSS属性支持可以参照caniuse网站http://caniuse.com/css-opacity
参考文献：CSS opacity介绍 https://developer.mozilla.org/en-US/docs/Web/CSS/opacity?redirectlocale=en-US&redirectslug=CSS%2Fopacity

