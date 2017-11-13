# 定时器控件

> 官网地址：[https://lcd4linux.bulix.org/wiki/widget_timer](https://lcd4linux.bulix.org/wiki/widget_timer)
<br>

## 定时器控件
定时器控件是一种特殊的控件，它除了被定时调用之外不会做任何事。它的参数为表达式(只有当 `active` 参数的值不为零时才会执行)，但是它的执行结果会被丢弃。你可以在表达式中设置变量，然后调用函数来做一些有用的事(例如：设置对比度或者背光等)。 定义一个定时器控件的方法如下： 
```
Widget <name> {
    class      'Timer'
    expression <string>
    active     <number>
    update     <number>
}
```
<br>

## 参数
|参数|含义|说明|
|-|-|-|
|expression|表达式|主表达式。|
|active|活动性|当它的值为 0 时定时器是活动的。|
|update|刷新频率|刷新频率(单位：毫秒)。|

> 注意：`update` 和 `active` 参数在每次定时器被调用时被赋值，所以你可以在LCD4Linux运行时修改它们的值。
<br>

## 例子
暂时没有例子。