# 栏控件

> 官网地址：[https://lcd4linux.bulix.org/wiki/widget_bar](https://lcd4linux.bulix.org/wiki/widget_bar)
<br>

## 栏控件 (类似于柱状图)

栏控件用来显示各种栏。

LCD4Linux在栏的处理上做的很好。它支持水平栏、垂直栏和拆分栏(两个独立的栏在同一行，用到expression2)，各种类型的栏可以同时使用。 

目前只有一种样式可用于方向( `direction` )为 'E' 和 'W' 的栏，即： `hollow` (空心)，它是一个围绕在栏外层的框架。(你可以点击~[CoolStuff](https://lcd4linux.bulix.org/wiki/CoolStuff)~来查看它的用例。)

处理栏控件是一件很棘手的工作：大部分显示器对于用户自定义的字符数有限制(通常是八个)，如果你想要精确地显示这些栏的话，这个数量实在是太少了。如果你同时还使用了[图符控件](https://github.com/enify/lcd4linux-doc/blob/master/wiki/widget_text.md)的话，这个数量还会更少。所以LCD4Linux对它做了一些误差扩散(error diffusion) 的处理：把字符替换成另一个与它相似的字符。在这里，你必须考虑另一点：如果你重定义一个正在显示的字符的话，显示器上将会闪烁。所以LCD4Linux不允许对正在显示的字符重定义，即使它在下一次刷新后不会被使用。只有当误差扩散(error diffusion)失败时，字符才会在被重定义后显示。 

如果你不相信我这个东西很奇怪的话，你可以看一下在  *drv_generic_text.c* 中处理栏控件的代码。

一个栏控件看起来是这样的：
```
Widget <name> {             # 控件名
    class       'Bar'       # 控件类型
    expression  <expr>      # 表达式1
    expression2 <expr>      # 表达式2 (注：用于拆分栏)
    length      <number>    # 栏的长度
    min         <number>    # 栏的最小值（起始位置）
    max         <number>    # 栏的最大值（终止位置）
    direction   <char>      # 方向
    style       <char>      # 样式
    foreground  <color>     # 前景色
    background  <color>     # 背景色
    update      <number>    # 刷新频率
    barcolor0   <color>     # 上半栏颜色
    barcolor1   <color>     # 下半栏颜色 (注：用于拆分栏)
}
```
<br>

## 参数
|参数|含义|说明|
|-|-|-|
|expression|表达式|表达式的执行结果作为(栏的)上半栏的长度。|
|expression2|表达式|表达式的执行结果作为(栏的)下半栏的长度。(注：用于拆分栏)|
|length|长度|栏的填充长度。|
|min|最小值|栏的最小长度。|
|max|最大值|栏的最大长度。|
|direction|方向|**'E'** (向东:从左边朝向右边,默认值), **'W'** (向西: 从右边朝向左边), **'N'** (向北:从底部朝向顶部) or **'S'** (向南:从顶部朝向底部) |
|style|样式|**'H'** (hollow样式:栏的外围有框架围绕)，默认值为: **none**|
|foreground|前景色|活动像素的颜色，(格式：RRGGBBAA 或 RRGGBB)，默认为不透明的黑色('000000ff')，点击~[颜色](https://lcd4linux.bulix.org/wiki/colors)~了解更多。|
|background|背景色|非活动像素的颜色，(格式：RRGGBBAA 或 RRGGBB), 默认为透明('ffffff00')，点击~[颜色](https://lcd4linux.bulix.org/wiki/colors)~了解更多。|
|update|刷新频率|刷新频率(单位：ms)，默认值为1000ms(即1秒)。|
|barcolor0|上半栏颜色|上半栏的颜色，默认与前景色相同|
|barcolor1|下半栏颜色|下半栏的颜色，默认为前景色相同|

<br>

## 例子
下面是一个例子：
```
Widget BusyBar {
    class      'Bar'
    expression  proc_stat::cpu('busy',   500)
    expression2 proc_stat::cpu('system', 500)
    length      10	
    direction  'E'
    style      'H'
    update      100
    BarColor0  'ff0000'
    BarColor1  '00ff00'
```

>  如果你省略 `expression2` 的值, 你将会得到一个只由 `expression` 控制的单一的栏(single bar)。如果你指定了 `expression2` 的值(同时你的显示器支持拆分栏的话)，那么你将得到一个拆分栏(split bars)，其中上半栏由 `expression` 控制，下半栏由 `expression2` 控制。

<br>

## 另一个例子
```
Widget MP3Bar {
    class 'Bar'
    expression  xmms('uSecPosition')
    length 20
    min 0
    max xmms('uSecTime')
    direction 'E'
    update tick
}
```
栏控件可以用在**XMMS**(一个音频播放器)插件上，作为进度条使用，参数 `length` 用于设置进度条的总长度。

<br>

## 缩放
通常情况下，栏控件可以自动缩放，这意味着它们从0开始，到最大值结束，然后根据最大值自动缩放。

有时候，这种特性并不是你想要的(例如对于温度来说)。在这种情况下，你可以通过指定参数 `min` 和 `max` 的值来自定义栏的长度。

从0.9版本开始，LCD4Linux开始支持对数栏(logarithmic bars)，只需在表达式里好函数即可。 