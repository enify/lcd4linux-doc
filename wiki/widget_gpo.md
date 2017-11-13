# 通用输出控件

> 官网地址：[https://lcd4linux.bulix.org/wiki/widget_gpo](https://lcd4linux.bulix.org/wiki/widget_gpo)
<br>

## 通用输出控件/通用输入设备

 通用输出控件(GPO widget)用来控制所谓的通用输出口，你的显示器可能有这个东西。这个通用输出口可以用来连接发光二极管，蜂鸣器，继电器等器件。一些显示器上可能有电路图/引脚符号(特别是当你使用一些拆自电话机上的显示器的时候)，这些引脚符号可以被通用输出控件控制。

通用输入输出设备(GPIO driver)不仅提供通用输出控件(GPO widget)，还提供通用输入控件(GPI widget)功能。这些输入可以是逻辑值(0/1)或者模拟值，以提供真实(世界)的值(如：风扇转速，温度，电压...) 

通用输出控件与输出线的对应关系写在布局类的节里：就像你用Row\*.Col\*放置其它控件一样，你可以用'GPO\*'这么一行来说明通用输出控件和实际输出线的对应关系。发送到通用输出线的值由控件的表达式( 'expression'参数 )决定。

定义一个通用输出控件的方法如下： 
```
Widget <widget_name> {     # 控件名
    class      'GPO'       # 控件类型
    expression <string>    # 表达式
    update     <number>    # 刷新频率
}
```

下面是把控件(表达式的值)与实际输出线对应起来的方法：
```
Layout <layout_name> {               # 布局名
    GPO<num>      '<widget_name>'    # GPO对应关系
}
```

> 注意：只有当你的显示器支持通用输出时，通用输出控件和*LCD::GPO()*插件才可用(同时需要配置好它们)！

<br>

## 参数
|参数|含义|说明|
|-|-|-|
|expression|表达式|传递值给通用输出口的表达式。|
|update|刷新频率|刷新频率(单位：毫秒)。|

<br>

## 插件
LCD4Linux为通用输入输出设备提供了以下几个函数： 
|函数|作用|
|-|-|
|LCD::GPI(num)|返回从通用输入口*num* 获取到的值。|
|LCD::GPO(num)|返回通用输出口*num* 当前的值。|
|LCD::GPO(num, val)|设置通用输出口*num* 的值为val，并返回其当前值。|

<br>

## 例子
这是一个简单的通用输出控件的例子。在这个例子中，系统的平均负载将会在GPO_Test这个控件里计算，然后把它的值发送到显示器的通用输出口1 (GPO1).如果负载大于0的话GPO1为高电位，等于0的话GPO1为低电位。
```
Widget GPO_Test {
    class 'GPO'
    expression loadavg(1) > 0
    update 1000
}

Layout TestGPO {
    Row1.Col1 'AnyWidget'
    GPO1      'GPO_Test'
}
```

使用通用输入口的使用例子（在[MatrixOrbital](https://lcd4linux.bulix.org/wiki/MatrixOrbital) LK202设备上的测试）:
```
Widget Fan_Set {
    class 'GPO'
    expression test::bar(0,255, 0, 1)
    update 100
}

Widget Fan_PWM {
    class 'Text'
    expression LCD::GPO(1)
    prefix 'PWM'
    width 8
    precision 0
    align 'R'
    update 100
}

Widget Fan_RPM {
    class 'Text'
    expression LCD::GPI(1)
    prefix 'RPM'
    width 8
    precision 0
    align 'R'
    update 100
}

Layout FanDemo {
    Row1.Col1  'Fan_PWM'
    Row1.Col10 'Fan_RPM'
    GPO1       'Fan_Set'
}
```
风扇的PWM值在0和255之间徘徊，且实际的PWM值和风扇速度将会打印到显示器上。