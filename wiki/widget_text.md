# 文本控件

> 官网地址：[https://lcd4linux.bulix.org/wiki/widget_text](https://lcd4linux.bulix.org/wiki/widget_text)
<br>

## 文本控件
文本控件是最常见的一种控件，它可以用来显示任何文本和数字(不管是静态的还是动态的)。 

一个功能完整的文本控件的格式如下：
```
Widget 'name' {          # 控件名
   class      'Text'     # 控件类型
   expression <expr1>    # 表达式
   prefix     <expr2>    # 前缀
   postfix    <expr3>    # 后缀
   width      <number>   # 宽度
   align      <char>     # 排列方式
   style      <string>   # 字体
   foreground <color>    # 前景色
   background <color>    # 背景色
   speed      <number>   # 滚动速度
   update     <number>   # 刷新频率
}
``` 
<br>

## 参数
|参数|含义|说明|
|-|-|-|
|expression|表达式|表达式将会执行，其结果将会显示出来。|
|prefix,postfix|前缀，后缀|这些表达式的执行结果将在实际值的前面/后面显示出来。(注：显示格式为：'prefix + actual value  + postfix'')|
|width|宽度|整个控件的宽度(包含前缀与后缀)。|
|precision|精度|(最大限度的)小数位数。|
|align|排列方式|它的值应下列值中的一个：**'L'** (左对齐), **'C'** (居中), **'R'** (右对齐), **'M'** (滚动字幕), **'A'** (自动滚动字幕), **'PL'** (来回滚动字幕，文本太短的话将其左对齐), **'PC'** (来回滚动字幕，文本太短的话将其居中) 、 **'PR'** (来回滚动字幕，文本太短的话将其右对齐)。|
|style|字体|文本的字体。 **'bold'**代表粗体，其它的按正常字体显示。(仅对图形显示设备有效)。|
|foreground|前景色|活动像素的颜色，(格式：RRGGBBAA 或 RRGGBB)，默认为不透明的黑色('000000ff')，点击~[颜色](https://lcd4linux.bulix.org/wiki/colors)~了解更多。 |
|background|背景色|非活动像素的颜色，(格式：RRGGBBAA 或 RRGGBB), 默认为透明('ffffff00')，点击~[颜色](https://lcd4linux.bulix.org/wiki/colors)~了解更多。|
|speed|滚动速度|字幕滚动的时间间隔(单位：ms)，默认值为500ms|
|update|刷新频率|刷新频率(单位：ms)，默认值为1000ms(即1秒)，为 0 时代表从不刷新(用于静态文本)。|

>注：'align A' 在0.10.2版本上可用，'align P_' 在0.11.0版本上可用。
<br>

你可能会问，为什么 `prefix` 和 `postfix` 应该是表达式，而不仅仅是静态文本？答案很简单：为了给你最大限度的灵活性。想象一下，你有一个名为load_avg（平均负载）的控件，它可以把系统的平均负载显示出来。而当负载值大于2时，你想要显示一个感叹号(以起警示作用)，这时你可以这么做：
```
Widget loadavg {
    class      'Text'
    expression  loadavg(1)
    prefix     'Load'
    postfix     loadavg(1) > 2.0 ? '!' : ' '
    width       10
    precision   1
    align      'R'
    update      200
}
```
<br>

## 对齐方式
注意：默认情况下，只有实际值（注：expr1的执行结果）会按照你 `align` 参数指定的方式排列，而 `prefix` 则总是左对齐， `postfix` 则总是右对齐。如果你使用的是 '滚动字幕' 排列方式(align 'M')的话，情况则不同。这时前缀，实际值，后缀只是简单地拼接在一起，将结果字符串以循环的方式显示，每个speed msec步进一个字符。
<br>

## 精度
如果你省略了 `precision` 参数，那么LCD4Linux将把表达式的执行结果当做一个字符串来显示。

如果你指定了 `precision` 参数，那么表达式的执行结果将转换成浮点数，然后截取相应的小数位数后显示。如果这个数的长度大于可用空间【 (width - length(prefix) - length(postfix) 】的话，小数位将会从右边开始丢弃，直到有足够的空间容纳它为止。如果最后一位小数也被剪去(就是一位小数也没有了)，那么小数点也将被省略掉。如果这个值还是不适合，那么它将显示为若干个星号'*'.

所以说 "precision 0" 和不指定精度是有很大区别的！

换一种说法，就是：如果你想要一个表达式作为一个字符串显示，那么请不要给它指定精度！否则LCD4Linux将尝试将它转换成数字！ 