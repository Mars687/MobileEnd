# 移动端页面

## 像素

像素是web页面布局的基础，那么到底什么才是一个像素呢？
像素：一个像素就是计算机屏幕所能显示一种特定颜色的最小区域。 这是像素的概念，实际上，在web前端开发领域，像素有以下两层含义：
1、设备像素：设备屏幕的物理像素，对于任何设备来讲物理像素的数量是固定的。
2、CSS像素：这是一个抽象的像素概念，它是为web开发者创造的。

当我给一个元素设置了 width: 200px; 这条样式的时候，到底放生了什么事情？

这个200px指的是什么呢？因为我们知道，对于web前端来讲像素有两层含义，那么到底是设备像素还是CSS像素？实际上我们控制的是CSS像素，因为前面提到了，CSS像素是给我们web前端开发者创造的抽象概念。所以你要记住：当你给元素设置了 width: 200px 时，这个元素的宽度跨越了200个CSS像素。但是它并不一定跨越200个设备像素，至于会跨越多少个设备像素，就取决于手机屏幕的特性和用户的缩放了。

苹果手机的视网膜屏幕，是一个高密度屏幕，它的像素密度是普通屏幕的2倍，所以当我们设置 width: 200px; 时，200个CSS像素跨越了400个设备像素。如果用户缩小页面，那么一个CSS像素会明显小于一个设备像素，这个时候 width: 200px; 这条样式中所设置的200个CSS像素跨越不了200个设备像素。

小结：
1、web前端领域，像素分为设备像素和CSS像素。
2、一个CSS像素的大小是可变的，比如用后缩放页面的时候，实际上就是在缩小或放大CSS像素，而设备像素无论大小还是数量都是不变的。

## 移动端的三个视口
你一定写过这样一条样式：width: 25%; 但是你有想过给一个元素加上这样一条样式之后发生了什么吗？25%是基于谁的25%？明白的同学可能知道了：一个块元素默认的宽度是其父元素的100%，是基于起父元素的，所以25%指的是父元素宽度的25%，所以，body元素的默认宽度是html元素宽度的100%，那么你有没有想过html元素的宽度是基于谁的呢？这个时候，就要引出一个概念：初始包含块和视口了

记住一句话：视口是html的父元素，所以我们称视口为初始包含块。 这样你就明白了，html元素的百分比是基于视口的。

### 第一个视口：布局视口
首先你需要了解一个原因：浏览器厂商是希望满足用户的要求的，即在手机也能浏览为PC端设计的网站。在PC浏览器中，视口只有一个，并且 视口的宽度 = 浏览器窗口的宽度，但是在移动端也要根据这个来设计的话，那么PC端设计的网站在移动端看起来会很丑，因为PC端的网页宽度在800 ~ 1024个CSS像素，而手机屏幕要窄的多，这个时候再PC端25%的宽度在移动端看起来会很窄。所以，布局视口的概念产生了。

布局视口：移动端CSS布局的依据视口，即CSS布局会根据布局视口来计算。
也就是说，在移动端，视口和浏览器窗口将不在关联，实际上，布局视口要比浏览器窗口大的多(在手机和平板中浏览器布局视口的宽度在768~1024像素之间)。

可以通过以下JavaScript代码获取布局视口的宽度和高度：

``` javascript
document.documentElement.clientWidth
document.documentElement.clientHeight
```

### 第二个视口：视觉视口
visual viewport（视觉视口）备物理屏幕的可视区域，屏幕显示器的物理像素，同样尺寸的屏幕，像素密度大的设备，硬件像素会更多。例如iPhone的物理像素：

* iPhone5 ：640 * 1136
* iPhone6：750 * 1334
* iPhone6 Plus：1242 * 2208

## 第三个视口：理想视口
理想视口，定义了理想视口的宽度，比如对于iphone5来讲，理想视口是320*568。但是最终作用的还是布局视口，因为我们的css是依据布局视口计算的，所以你可以这样理解理想视口：理想的布局视口。下面这段代码可以告诉手机浏览器要把布局视口设为理想视口：

>     <meta name="viewport" content="width=device-width" />
上面那段代码告诉浏览器：将布局视口的宽度设为理想视口。所以，上面代码中的width指的是布局视口的宽 device-width 实际上就是理想视口的宽度。

好了，移动端的三个视口介绍完了，让我们总结一下：

1、在PC端，布局视口就是浏览器窗口。
2、在移动端，视口被分为两个：布局视口、视觉视口。
3、移动端还有一个理想视口，它是布局视口的理想尺寸，即理想的布局视口。（注：理想视口的尺寸因设备和浏览器的不同而不同，但这对于我们来说无所谓）
4、可以将布局视口的宽度设为理想视口。

## 设备像素比(Device Pixel Ratio 简称：DPR)
下面是设备像素比的计算公式：

公式成立的大前提：（缩放比例为1）。
设备像素比(DPR) = 设备像素个数 / 理想视口CSS像素个数(device-width)

与理想视口一样，设备像素比对于不同的设备是不同的，但是他们都是合理的，比如早期iphone的设备像素是320px，理想视口也是320px，所以早起iphone的DPR=1，而后来iphone的设备像素为640px，理想视口还是320px，所以后来iphone的DPR=2。在开发中，打开浏览器的调试工具可以看到设备像素比。

## meta标签

meta视口标签存在的主要目的是为了让布局视口和理想视口的宽度匹配，meta视口标签应该放在HTML文档的head标签内，语法如下：

>     <meta name="viewport" content="name=value,name=value" />

其中 content 属性是一个字符串值，字符串是由逗号“，”分隔的 名/值 对组成，共有5个：

1、width：设置布局视口的宽
2、init-scale：设置页面的初始缩放程度
3、minimum-scale：设置了页面最小缩放程度
4、maximum-scale：设置了页面最大缩放程度
5、user-scalable：是否允许用户对页面进行缩放操作

下面是一个常用的meta标签实例

>     <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">

上面代码的意思是，让布局视口的宽度等于理想视口的宽度，页面的初始缩放比例以及最大缩放比例都为1，且不允许用户对页面进行缩放操作。

## 媒体查询
媒体查询是响应式设计的基础，他有以下三点作用：

1、检测媒体的类型，比如 screen，tv等
2、检测布局视口的特性，比如视口的宽高分辨率等
3、特性相关查询，比如检测浏览器是否支持某某特性（这一点不讨论，因为它被目前浏览器支持的功能对于web开发来讲很无用）

css中使用媒体查询的语法：

``` css
@media 媒体类型 and (视口特性阀值){
    // 满足条件的css样式代码

}
```

下面是一段在css中使用媒体查询的示例:

``` css
@media all and (min-width: 321px) and (max-width: 400px){
    .box{
        background: red;
    }
}
```

上面代码中，媒体类型为all，代表任何设备，并且设备的布局视口宽度大于等于321px且小于等于400px时，让拥有box类的元素背景变红。

## rem

### 什么是rem？

rem是相对尺寸单位，相对于html标签字体大小的单位，举个例子：
如果html的font-size = 18px;
那么1rem = 18px，需要记住的是，rem是基于html标签的字体大小的。

相信你已经明白了，对没错，我们要把之前用px做元素尺寸的单位换成rem，所以，现在的问题就是如果转换，因为rem是根据html标签的font-size值确定的，所以我们只要确定html标签的font-size值就行了，我们首先自己定一个标准，就是让font-size的值等于设备像素的十分之一(即设备像素可显示10个字)，即：

>     document.documentElement.style.fontSize = document.documentElement.clientWidth / 10 + 'px';

以iphone6为例，html标签的font-size的值就等于 750 / 10 = 75px 了，这样 1rem = 75px，所以红色方块200px换算为rem单位就是 200 / 75 = 2.6666667rem。
那么在iphone5中呢？因为iphone5的设备像素为640，所以iphone的html标签的font-size的值为 640 / 10 = 64px，所以 1rem = 64px，所以在iphone6中显示为200px的元素在iphone5中会显示为 2.6666667 * 64 像素，这样，在不同设备中就实现了让元素等比缩放从而不影响布局。而上面的方法也是手机淘宝所用的方法。所以，现在你只需要将你测量的尺寸数据除以75就转换成了rem单位，如果是iPhone5就要除以64，即除以你动态设置的font-size的值。

另外需要注意的是：做页面的时候文字字体大小不要用rem换算，还是使用px做单位。后面会讲到。

让我们来总结一下我们现在了解的方法：

1、将布局视口大小设为设备像素尺寸：

``` javascript
var scale = 1 / window.devicePixelRatio;
document.querySelector('meta[name="viewport"]').setAttribute('content','width=device-width,initial-scale=' + scale + ', maximum-scale=' + scale + ', minimum-scale=' + scale + ', user-scalable=no');
```

2、动态设置html字体大小：

>     document.documentElement.style.fontSize = document.documentElement.clientWidth / 10 + 'px';

3、将设计图中的尺寸换算成rem

元素的rem尺寸 = 元素的psd稿测量的像素尺寸 / 动态设置的html标签的font-size值

说了一大堆，其实我们使用下面的html莫板就可以写页面了，唯一需要你做的就是计算元素的rem尺寸，所以即使你没看懂上面的讲述也不重要，你只要将模板拿过去用就好了：

``` html
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta name="viewport" content="" />
</head>
<body>
    <script>
    var scale = 1 / window.devicePixelRatio;
    document.querySelector('meta[name="viewport"]').setAttribute('content','width=device-width,initial-scale=' + scale + ', maximum-scale=' + scale + ', minimum-scale=' + scale + ', user-scalable=no');

    document.documentElement.style.fontSize = document.documentElement.clientWidth / 10 + 'px';
    </script>
</body>
</html>
```

现在我们使用上面的方法修改我们的代码如下：

``` html
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta name="viewport" content="" />
    <style>
    body{
        margin: 0;
        padding: 0;
    }
    .box{
        width: 2.66666667rem;
        height: 2.66666667rem;
        background: red;
    }
    </style>
</head>
<body>
    <div class="box"></div>

    <script>
    var scale = 1 / window.devicePixelRatio;
    document.querySelector('meta[name="viewport"]').setAttribute('content','width=device-width,initial-scale=' + scale + ', maximum-scale=' + scale + ', minimum-scale=' + scale + ', user-scalable=no');

    document.documentElement.style.fontSize = document.documentElement.clientWidth / 10 + 'px';
    </script>
</body>
</html>
```

打开浏览器，分别在iPhone6和iPhone5下查看页面，我们会发现，现在的元素可以根据手机的尺寸不同而等比缩放了。

上面的方法是手机淘宝的方法，有一个缺点，就是转化rem单位的时候，需要除以font-size的值，淘宝用的是iPhone6的设计图，所以淘宝转换尺寸的时候要除以75，这个值可不好算，所以还要借用计算器来完成，影响开发效率，另外，在转还rem单位时遇到除不尽的数时我们会采用很长的近似值比如上面的2.6666667rem，这样可能会使页面元素的尺寸有偏差。

除了上面的方法比较通用之外，还有一种方式，我们来重新思考一下：

上面做页面的思路是：拿到设计图，比如iPhone6的设计图，我们就将浏览器设置到iPhone6设备调试，然后使用js动态修改meta标签，使布局视口的尺寸等于设计图尺寸，也就是设备像素尺寸，然后使用rem替代px，使得页面在不同设备中等比缩放。

现在假如我们不去修改meta标签，正常使用缩放为1:1的meta标签，即使用如下meta标签：

>     <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no" />

还以iPhone6为例，我们知道，在缩放为1:1的情况下，根据公式：

设备像素比（DPR） = 设备像素个数 / 理想视口像素个数（device-width）

我们知道：
设备像素 = 设计图尺寸 = 750px
布局视口 = 375px

假设我们以iPhone6设计图尺寸为标准，在设计图的尺寸下设置一个font-size值为100px。
也就是说：750px宽的页面，我们设置100px的font-size值，那么页面的宽度换算为rem就等于 750 / 100 = 7.5rem。

我们就以页面总宽为7.5rem为标准，那么在布局视口中，也就是页面总宽为375px下，font-size值应该是多少？很简单：

>font-size = 375 / 7.5 = 50px

那么在iPhone5下呢？因为iPhone5的布局视口宽为320px，所以如果页面总宽以7.5为标准，那么iPhone5下我们设置的font-size值应该是：

>font-size = 320 / 7.5 =42.666666667px

也就是说，不管在什么设备下，我们都可以把页面的总宽度设为一个以rem为单位的定值，比如本例就是7.5rem，只不过，我们需要根据布局视口的尺寸动态设置font-size的值：

>     document.documentElement.style.fontSize = document.documentElement.clientWidth / 7.5 + 'px';

这样，无论在什么设备下，我们页面的总宽度都是7.5rem，所以我们直接在设计图上测量px单位的尺寸，然后除以100转换成rem单位后直接使用就可以了，比如，在iPhone6设计图中测量一个元素的尺寸为200px，那么转换成rem单位就是 200 / 100 = 2rem，因为在不同设备下我们动态设置了html标签的font-size值，所以不同设备下相同的rem值对应的像素值是不同的，这样就实现了在不同设备下等比缩放。我们修改html代码如下：

``` html
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no" />
    <style>
    body{
        margin: 0;
        padding: 0;
    }
    .box{
        width: 2rem;
        height: 2rem;
        background: red;
    }
    </style>
</head>
<body>

    <div class="box"></div>

    <script>
    document.documentElement.style.fontSize = document.documentElement.clientWidth / 7.5 + 'px';
    </script>
</body>
</html>
```

刷新页面，分别在iPhone6和iPhone5下调试查看结果，会发现如下图，使我们想要的效果，等比缩放，ok，实际上这种做法也是网易的做法。

下面，我们来总结一下第二种做法：

1、拿到设计图，计算出页面的总宽，为了好计算，取100px的font-size，如果设计图是iPhone6的那么计算出的就是7.5rem，如果页面是iPhone5的那么计算出的结果就是6.4rem。

2、动态设置html标签的font-size值：

>     document.documentElement.style.fontSize = document.documentElement.clientWidth / 以rem为单位的页面总宽 + 'px';

  如iPhone6的设计图就是：

>     document.documentElement.style.fontSize = document.documentElement.clientWidth / 7.5 + 'px';

  iPhone5的设计图就是：

>     document.documentElement.style.fontSize = document.documentElement.clientWidth / 6.4 + 'px';

3、做页面是测量设计图的px尺寸除以100得到rem尺寸。

4、和淘宝的做法一样，文字字体大小不要使用rem换算。

下面是这种做法的html莫板：

```html
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no" />
</head>
<body>
    <script>
    document.documentElement.style.fontSize = document.documentElement.clientWidth / 7.5 + 'px';
    </script>
</body>
</html>
```

由于这种做法在开发中换算rem单位的时候只需要将测量的尺寸除以100即可，所以不需要使用计算器我们就可以很快的完成计算转换，所以这也会提升开发效率，本人也比较青睐这种做法。

另外，无论是第一种做法还是第二种做法，我们都提到了，文字字体大小是不要换算成rem做单位的，而是使用媒体查询来进行动态设置，比如下面的代码就是网易的代码：

代码片段一：
``` css
@media screen and (max-width: 321px) {
    body {
        font-size:16px
    }
}

@media screen and (min-width: 321px) and (max-width:400px) {
    body {
        font-size:17px
    }
}

@media screen and (min-width: 400px) {
    body {
        font-size:19px
    }
}
```

代码片段二：

``` css
@media screen and (max-width: 321px) {
    header,footer {
        font-size:16px
    }
}

@media screen and (min-width: 321px) and (max-width:400px) {
    header,footer {
        font-size:17px
    }
}

@media screen and (min-width: 400px) {
    header,footer {
        font-size:19px
    }
}
```

我们总结一下网易在文字字体大小上的做法，在媒体查询阶段，分为三个等级分别是：
* 321px以下
* 321px - 400px之间
* 400px以上

具体文字大小要多少个像素这个以设计图为准，但是这三个等级之间是有规律的，仔细观察发现，321px以下的屏幕字体大小比321px - 400px之间的屏幕字体大小要小一个像素，而321px - 400px之间的屏幕字体大小要比400以上屏幕字体大小要小2个像素。依照这个规律，我们根据设计图所在的像素区段先写好该区段的字体大小，然后分别写出另外两个区段的字体大小媒体查询代码就可以了。


# 移动端页面开发总结

## meta标签相关知识

1、移动端页面设置视口宽度等于设备宽度，并禁止缩放。

>     <meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />

2、移动端页面设置视口宽度等于定宽（如640px），并禁止缩放，常用于微信浏览器页面。

>     <meta name="viewport" content="width=640,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />

3、禁止将页面中的数字识别为电话号码

>     <meta name="format-detection" content="telephone=no" />

4、忽略Android平台中对邮箱地址的识别

>     <meta name="format-detection" content="email=no" />

5、当网站添加到主屏幕快速启动方式，可隐藏地址栏，仅针对ios的safari

``` html
<meta name="apple-mobile-web-app-capable" content="yes" />
<!-- ios7.0版本以后，safari上已看不到效果 -->
```

6、将网站添加到主屏幕快速启动方式，仅针对ios的safari顶端状态条的样式

``` html
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
<!-- 可选default、black、black-translucent -->
```

## viewport模板

``` html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no" name="viewport">
<meta content="yes" name="apple-mobile-web-app-capable">
<meta content="black" name="apple-mobile-web-app-status-bar-style">
<meta content="telephone=no" name="format-detection">
<meta content="email=no" name="format-detection">
<title>title</title>
<link rel="stylesheet" href="index.css">
</head>
<body>
    content...
</body>
</html>
```

## CSS样式技巧

1、禁止ios和android用户选中文字

>     .css{-webkit-user-select:none}

2、禁止ios长按时触发系统的菜单，禁止ios&android长按时下载图片

>     .css{-webkit-touch-callout: none}

3、webkit去除表单元素的默认样式

>     .css{-webkit-appearance:none;}

4、修改webkit表单输入框placeholder的样式

>     input::-webkit-input-placeholder{color:#AAAAAA;}

>     input:focus::-webkit-input-placeholder{color:#EEEEEE;}

5、去除android a/button/input标签被点击时产生的边框 & 去除ios a标签被点击时产生的半透明灰色背景

>     a,button,input{-webkit-tap-highlight-color:rgba(255,0,0,0);}

6、ios使用-webkit-text-size-adjust禁止调整字体大小

>     body{-webkit-text-size-adjust: 100%!important;}

7、android 上去掉语音输入按钮

>     input::-webkit-input-speech-button {display: none}

8、移动端定义字体，移动端没有微软雅黑字体

``` html
/* 移动端定义字体的代码 */
body{font-family:Helvetica;}
```

9、禁用Webkit内核浏览器的文字大小调整功能。

>     -webkit-text-size-adjust: none;

10、去掉 input[type=search] 搜索框默认的叉号

``` html
input[type="search"]{
    -webkit-appearance:none;
}
input::-webkit-search-cancel-button {
    display: none;
}
```

## 其他技巧

### 手机拍照和上传图片

``` html
<!-- 选择照片 -->
<input type=file accept="image/*">
<!-- 选择视频 -->
<input type=file accept="video/*">
```

2、取消input在ios下，输入的时候英文首字母的默认大写

>     <input autocapitalize="off" autocorrect="off" />

3、打电话和发短信

``` html
<a href="tel:0755-10086">打电话给:0755-10086</a>
<a href="sms:10086">发短信给: 10086</a>
```

四、CSS reset

``` css
/* hcysun  */
@charset "utf-8";
/* reset */
html{
    -webkit-text-size-adjust:none;
    -webkit-user-select:none;
    -webkit-touch-callout: none
    font-family: Helvetica;
}
body{font-size:12px;}
body,h1,h2,h3,h4,h5,h6,p,dl,dd,ul,ol,pre,form,input,textarea,th,td,select{margin:0; padding:0; font-weight: normal;text-indent: 0;}
a,button,input,textarea,select{ background: none; -webkit-tap-highlight-color:rgba(255,0,0,0); outline:none; -webkit-appearance:none;}
em{font-style:normal}
li{list-style:none}
a{text-decoration:none;}
img{border:none; vertical-align:top;}
table{border-collapse:collapse;}
textarea{ resize:none; overflow:auto;}
/* end reset */
```

五、常用公用CSS style

``` css
/* public */

/* 清除浮动 */
.clear { zoom:1; }
.clear:after { content:''; display:block; clear:both; }

/* 定义盒模型为怪异和模型（宽高不受边框影响） */
.boxSiz{
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    -ms-box-sizing: border-box;
    -o-box-sizing: border-box;
    box-sizing: border-box;
}

/* 强制换行 */
.toWrap{
    word-break: break-all;       /* 只对英文起作用，以字母作为换行依据。 */
    word-wrap: break-word;       /* 只对英文起作用，以单词作为换行依据。*/
    white-space: pre-wrap;     /* 只对中文起作用，强制换行。*/
}

/* 禁止换行 */
.noWrap{
    white-space:nowrap;
}

/* 禁止换行,超出省略号 */
.noWrapEllipsis{
     white-space:nowrap; overflow:hidden; text-overflow:ellipsis;
}

/* 多行显示省略号，less写法，@line是行数 */
.ellipsisLn(@line) {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: @line;
}

/* 1px 边框解决方案，示例中设置上边框，可以调整 top、right、bottom、left 的值分别设置上下左右边框 */
#box2:after{
    content: " ";
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    height: 1px;
    border-top: 1px solid #000;
    color: #C7C7C7;
    transform-origin: 0 0;
    transform: scaleY(0.5);
}

/* 文字两端对齐 */
.text-justify{
    text-align:justify; 
    text-justify:inter-ideograph;
}

/* 定义盒模型为 flex布局兼容写法并让内容水平垂直居中 */
.flex-center{
    display: -webkit-box;
    display: -moz-box;
    display: -ms-flexbox;
    display: -o-box;
    display: box;

    -webkit-box-pack: center;
    -moz-box-pack: center;
    -ms-flex-pack: center;
    -o-box-pack: center;
    box-pack: center;

    -webkit-box-align: center;
    -moz-box-align: center;
    -ms-flex-align: center;
    -o-box-align: center;
    box-align: center;
}

/* public end */
```

六、flex布局
1、定义弹性盒模型兼容写法

``` css
/*
    box
    inline-box
*/
display: -webkit-box;
display: -moz-box;
display: -ms-flexbox;
display: -o-box;
display: box;
```

2、box-orient 定义盒模型内伸缩项目的布局方向

``` css
/**
 * vertical column    垂直
 * horizontal row    水平 默认值
 */
-webkit-box-orient: horizontal;
-moz-box-orient: horizontal;
-ms-flex-direction: row;
-o-box-orient: horizontal;
box-orient: horizontal;
```

3、box-direction 定义盒模型内伸缩项目的正序(normal默认值)、倒叙(reverse)

``` css
/* Firefox */
display:-moz-box;
-moz-box-direction:reverse;
/* Safari、Opera 以及 Chrome */
display:-webkit-box;
-webkit-box-direction:reverse;
```

4、box-pack 对盒子水平富裕空间的管理

``` css
/*
    start
    end
    center
    justify
*/
-webkit-box-pack: center;
-moz-box-pack: center;
-ms-flex-pack: center;
-o-box-pack: center;
box-pack: center;
```

5、box-pack 对盒子垂直方向富裕空间的管理

``` css
/*
    start
    end
    center
*/
/* box-align */
-webkit-box-align: center;
-moz-box-align: center;
-ms-flex-align: center;
-o-box-align: center;
box-align: center;
```

6、定义伸缩项目的具体位置

``` css
/*-moz-box-ordinal-group:1;*/ /* Firefox */
/*-webkit-box-ordinal-group:1;*/ /* Safari 和 Chrome */
.box div:nth-of-type(1){-webkit-box-ordinal-group:1;}
.box div:nth-of-type(2){-webkit-box-ordinal-group:2;}
.box div:nth-of-type(3){-webkit-box-ordinal-group:3;}
.box div:nth-of-type(4){-webkit-box-ordinal-group:4;}
.box div:nth-of-type(5){-webkit-box-ordinal-group:5;}
```

7、定义伸缩项目占空间的份数

``` css
-moz-box-flex:2.0; /* Firefox */
-webkit-box-flex:2.0; /* Safari 和 Chrome */

.box div:nth-of-type(1){-webkit-box-flex:1;}
.box div:nth-of-type(2){-webkit-box-flex:2;}
.box div:nth-of-type(3){-webkit-box-flex:3;}
.box div:nth-of-type(4){-webkit-box-flex:4;}
.box div:nth-of-type(5){-webkit-box-flex:5;}
```

# 静态布局、自适应布局、流式布局、响应式布局、弹性布局等的概念和区别

## 静态布局（Static Layout）
即传统Web设计，网页上的所有元素的尺寸一律使用px作为单位。

1、布局特点：不管浏览器尺寸具体是多少，网页布局始终按照最初写代码时的布局来显示。常规的pc的网站都是静态（定宽度）布局的，也就是设置了min-width，这样的话，如果小于这个宽度就会出现滚动条，如果大于这个宽度则内容居中外加背景，这种设计常见与pc端。
2、设计方法：
　　PC：居中布局，所有样式使用绝对宽度/高度(px)，设计一个Layout，在屏幕宽高有调整时，使用横向和竖向的滚动条来查阅被遮掩部分；
　　移动设备：另外建立移动网站，单独设计一个布局，使用不同的域名如wap.或m.。

　   在移动端开发中采用静态布局的两种方式：

　（1）在viewport meta标签上设置width=320，页面的各个元素也采用px作为单位。通过用JS动态修改标签的initial-scale使得页面等比缩放，从而刚好占满整个屏幕。（见前端开发-web app 变革之rem）

　（2）设在viewport meta标签上设置content"width=640,user-scalable=no，页面的各个元素也采用px作为单位。由于640px超出了手机宽度，浏览器会自动缩小页面至刚好全屏。（具体见content"width=640,user-scalable=no" 然后再进行固定尺寸的px设计？ - 前端开发）

优点：这种布局方式对设计师和CSS编写者来说都是最简单的，亦没有兼容性问题。

缺点：显而易见，即不能根据用户的屏幕尺寸做出不同的表现。

当前，大部分门户网站、大部分企业的PC宣传站点都采用了这种布局方式。固定像素尺寸的网页是匹配固定像素尺寸显示器的最简单办法。但这种方法不是一种完全兼容未来网页的制作方法，我们需要一些适应未知设备的方法。

## 流式布局（Liquid Layout）

流式布局（Liquid）的特点（也叫"Fluid") 是页面元素的宽度按照屏幕分辨率进行适配调整，但整体布局不变。代表作栅栏系统（网格系统）。

网页中主要的划分区域的尺寸使用百分数（搭配min-*、max-*属性使用），例如，设置网页主体的宽度为80%，min-width为960px。图片也作类似处理（width:100%, max-width一般设定为图片本身的尺寸，防止被拉伸而失真）。

1、布局特点：屏幕分辨率变化时，页面里元素的大小会变化而但布局不变。【这就导致如果屏幕太大或者太小都会导致元素无法正常显示】

2、设计方法：使用%百分比定义宽度，高度大都是用px来固定住，可以根据可视区域 (viewport) 和父元素的实时尺寸进行调整，尽可能的适应各种分辨率。往往配合 max-width/min-width 等属性控制尺寸流动范围以免过大或者过小影响阅读。

这种布局方式在Web前端开发的早期历史上，用来应对不同尺寸的PC屏幕（那时屏幕尺寸的差异不会太大），在当今的移动端开发也是常用布局方式，但缺点明显：主要的问题是如果屏幕尺度跨度太大，那么在相对其原始设计而言过小或过大的屏幕上不能正常显示。因为宽度使用%百分比定义，但是高度和文字大小等大都是用px来固定，所以在大屏幕的手机下显示效果会变成有些页面元素宽度被拉的很长，但是高度、文字大小还是和原来一样（即，这些东西无法变得“流式”），显示非常不协调。

## 自适应布局（Adaptive Layout）
自适应布局的特点是分别为不同的屏幕分辨率定义布局，即创建多个静态布局，每个静态布局对应一个屏幕分辨率范围。改变屏幕分辨率可以切换不同的静态局部（页面元素位置发生改变），但在每个静态布局中，页面元素不随窗口大小的调整发生变化。可以把自适应布局看作是静态布局的一个系列。
1、布局特点：屏幕分辨率变化时，页面里面元素的位置会变化而大小不会变化。
2、设计方法：使用 @media 媒体查询给不同尺寸和介质的设备切换不同的样式。在优秀的响应范围设计下可以给适配范围内的设备最好的体验，在同一个设备下实际还是固定的布局。

## 响应式布局（Responsive Layout）

随着CSS3出现了媒体查询技术，又出现了响应式设计的概念。响应式设计的目标是确保一个页面在所有终端上（各种尺寸的PC、手机、手表、冰箱的Web浏览器等等）都能显示出令人满意的效果，对CSS编写者而言，在实现上不拘泥于具体手法，但通常是糅合了流式布局+弹性布局，再搭配媒体查询技术使用。——分别为不同的屏幕分辨率定义布局，同时，在每个布局中，应用流式布局的理念，即页面元素宽度随着窗口调整而自动适配。即：创建多个流体式布局，分别对应一个屏幕分辨率范围。可以把响应式布局看作是流式布局和自适应布局设计理念的融合。

响应式几乎已经成为优秀页面布局的标准。

1、布局特点：每个屏幕分辨率下面会有一个布局样式，即元素位置和大小都会变。

2、设计方法：媒体查询+流式布局。通常使用 @media 媒体查询 和网格系统 (Grid System) 配合相对布局单位进行布局，实际上就是综合响应式、流动等上述技术通过 CSS 给单一网页不同设备返回不同样式的技术统称。

优点：适应pc和移动端，如果足够耐心，效果完美

缺点：（1）媒体查询是有限的，也就是可以枚举出来的，只能适应主流的宽高。（2）要匹配足够多的屏幕大小，工作量不小，设计也需要多个版本。


响应式页面在头部会加上这一段代码：

``` html
<meta name="applicable-device" content="pc,mobile">
<meta http-equiv="Cache-Control" content="no-transform ">
```

总结：

响应式与自适应的原理是相似的，都是检测设备，根据不同的设备采用不同的css，而且css都是采用的百分比的，而不是固定的宽度，不同点是响应式的模板在不同的设备上看上去是不一样的，会随着设备的改变而改变展示样式，而自适应不会，所有的设备看起来都是一套的模板，不过是长度或者图片变小了，不会根据设备采用不同的展示样式，流式就是采用了一些设置，当宽度大于多少时怎么展示，小于多少时怎么展示，而且展示的方式向水流一样，一部分一部分的加载，静态的就是采用固定宽度的了。

流式布局是用于解决类似的设备不同分辨率之间的兼容(一般分辨率差异较少)；响应式是用于解决不用设备之间不用分辨率之间的兼容问题(一般是指PC，平板，手机等设备之间较大的分辨率差异)。

如何实现响应式布局：折腾响应式布局设计，应运而生的web页面响应布局

## 弹性布局（rem/em布局）

参考：流布局与响应式网页设计有什么区别？
1、rem,em区别：rem,em都是顺应不同网页字体大小展现而产生的。其中，em是相对其父元素，在实际应用中相对而言会带来很多不便；而rem是始终相对于html大小，即页面根元素。

2、使用 em 或 rem 单位进行相对布局，相对%百分比更加灵活，同时可以支持浏览器的字体大小调整和缩放等的正常显示，因为em是相对父级元素的原因没有得到推广。【中国站点制作网页的时候，习惯用CSS强制定义字体大小，保证每个人都看到一致的效果，包括网易、搜狐这些门户网站在内的大部分站点，用的都是绝对单位px（像素）。但是，如果从网站易用性方面考虑，字体大小应该是可变的，一些视力不是那么好的人需要放大字体才能看得清页面内容。然而，占据大部分浏览器市场的IE无法调整那些使用px作为单位的字体大小。国外人士非常重视网站的易用性，相当一部分外国站点已经使用em作为字体单位。】
3、这类布局的特点是，包裹文字的各元素的尺寸采用em/rem做单位，而页面的主要划分区域的尺寸仍使用百分数或px做单位（同「流式布局」或「静态/固定布局」）。早期浏览器不支持整个页面按比例缩放，仅支持网页内文字尺寸的放大，这种情况下。使用em/rem做单位，可以使包裹文字的元素随着文字的缩放而缩放。

4、浏览器的默认字体高度一般为16px，即1em:16px，但是 1:16 的比例不方便计算，为了使单位em/rem更直观，CSS编写者常常将页面跟节点字体设为62.5%，比如选择用rem控制字体时，先需要设置根节点html的字体大小，因为浏览器默认字体大小16px*62.5%=10px。这样1rem便是10px，方便了计算。

Set body font-size to 62.5% for Easier em Conversion:
If you would like to use relative units (em) for your font sizes, declaring 62.5% for the font-size property of the body will make it easier to convert px to em. By doing it this way, converting to em is a matter of dividing the px value by 10 (e.g. 24px = 2.4em).
那么为什么一般多是 html{font-size:62.5%;} 而不是 html{font-size:10px;}呢？

因为有些浏览器默认的不是16px，或者用户修改了浏览器默认的字体大小（因浏览器分辨率大小，视力，习惯等因素）。如果我们将其设置为10px，一定会影响在这些浏览器上的效果，所以最好用绝大多数用户默认的16作为基数 * 62.5% 得到我们需要的10px。

``` html
html {font-size: 62.5%;/*10 ÷ 16 × 100% = 62.5%*/}
body {font-size: 1.4rem;/*1.4 × 10px = 14px */}
h1 { font-size: 2.4rem;/*2.4 × 10px = 24px*/}
```

实际项目设置成 font-size: 62.5%可能会出现问题，因为chrome不支持小于12px的字体，计算小于12px的时候，会默认取12px去计算，导致chrome的em/rem计算不准确。

针对这个现象，可以尝试设置html字体为100px，body 修正为16px，这样 0.1rem 就是 10px，而body的字体仍然是默认大小，不影响未设置大小的元素的默认字体的大小。

5、用em/rem定义尺寸的另一个好处是更能适应缩进/以字体单位padding或margin／浏览器设置字体尺寸等情况（因为em/rem相对于字体大小，会同步改变）。例如：p{ text-indent: 2em; }

6、使用rem单位的弹性布局在移动端也很受欢迎。
工具ViewtoREM：PX转换到REM（自动预处理）
rem的定义：font size of the root element，rem是相对于根元素<html>来设置字体大小的，这就意味着，我们只需要根据自己的需求在根元素确定一个参考值。
rem与em、px的区别：
px：像素，比较精确的单位，但不好做响应式布局
em：以父节点font-size大小为参考点，标准不统一，容易造成混乱
REM支持的浏览器：Mozilla Firefox 3.6+、Apple Safari 5+、Google Chrome、IE9+和Opera11+。IE6-8无法支持。

对于不同尺寸的屏幕，可以统一假设屏幕宽度为640px后编写CSS（当然你也可以假定统一为320px）。此时，我们设定html元素的font-size为40px（同样，只是举例），然后各处（元素尺寸、文字大小）使用rem作为单位，随后搭配媒体查询或JS，根据屏幕的大小来动态控制html元素的font-size（特定屏幕尺寸下，html元素的font-size应当设置为何值，是使用这个方案时设计师和程序员需要反复考虑后确定的，以下试举一段相关的CSS媒体查询代码），即可自动改变所有用rem定义尺寸的元素的大小（且CSS编写者在脑中进行换算的计算过程比em简单得多）。

``` html
html {
    font-size : 20px;
}
@media only screen and (min-width: 401px){
    html {
        font-size: 25px !important;
    }
}
@media only screen and (min-width: 428px){
    html {
        font-size: 26.75px !important;
    }
}
@media only screen and (min-width: 481px){
    html {
        font-size: 30px !important; 
    }
}
@media only screen and (min-width: 569px){
    html {
        font-size: 35px !important; 
    }
}
@media only screen and (min-width: 641px){
    html {
        font-size: 40px !important; 
    }
}
```

其实在移动端使用所谓的弹性布局，是比较勉强的。移动端弹性布局流行起来的原因归根结底是rem单位对于（根据屏幕尺寸）调整页面的各元素的尺寸、文字大小时比较好用。其实，使用vw、vh等后起之秀的单位，可以实现完美的流式布局（高度和文字大小都可以变得“流式”），弹性布局就不再必要了。详细可参考：视区相关单位vw, vh..简介以及可实际应用场景

优点：理想状态是所有屏幕的高宽比和最初的设计高宽比一样，或者相差不多，完美适应。

缺点：这种rem+js只不过是宽度自适应，高度没有做到自适应，一些对高度，或者元素间距要求比较高的设计，则这种布局没有太大的意义。如果只是宽度自适应，更推荐响应式设计。

响应式和弹性布局之间的对比：

响应式布局：改变浏览器宽度，“布局”会随之变化，不是一成不变的，例如导航栏在大屏幕下是横排，在小屏幕下是竖排，在超小屏幕下隐藏为菜单，也就是说如果有足够的耐心，在每一种屏幕下都应该有合理的布局，完美的效果。

rem布局：改变浏览器宽度，页面所有元素的高宽都等比例缩放，也就是大屏幕下导航是横的，小屏幕下还是横的只不过变小了。

结论：

1.如果只做pc端，那么静态布局（定宽度）是最好的选择；
2.如果做移动端，且设计对高度和元素间距要求不高，那么弹性布局（rem+js）是最好的选择，一份css+一份js调节font-size搞定；
3.如果pc，移动要兼容，而且要求很高那么响应式布局还是最好的选择，前提是设计根据不同的高宽做不同的设计，响应式根据媒体查询做不同的布局。
