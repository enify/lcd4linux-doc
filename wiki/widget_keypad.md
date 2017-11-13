# 键盘控件

> 官网地址：[https://lcd4linux.bulix.org/wiki/widget_keypad](https://lcd4linux.bulix.org/wiki/widget_keypad)
<br>

## 键盘控件

键盘控件和键盘设备允许你使用(显示器)上的常见按钮，像是上，下，左，右，确定，取消等。该功能于2006年2月添加到CVS/SVN版本中，现在仍在发展。它只在少数几个显示器设备上可用：~[Crystalfontz](https://lcd4linux.bulix.org/wiki/Crystalfontz)~ 635, ~[LCD2USB](https://lcd4linux.bulix.org/wiki/LCD2USB)~ 和 ~[LW_ABP](https://lcd4linux.bulix.org/wiki/LW_ABP)~。LCD4Linux-0.10.1-RC1版本同时还支持~[CwLinux](https://lcd4linux.bulix.org/wiki/CwLinux)~。

键盘控件是唯一一个不用用到 `update` 参数的控件。它在按钮按下或者释放的时候刷新，而不是事先设定一个时间间隔来刷新。刷新时表达式 `expression` 被重新赋值。你可以通过它来~[执行一些外部程序](https://lcd4linux.bulix.org/wiki/plugin_exec)~（像是reboot之类的）。或者简单地改变一个变量的值(它可能在另一个控件里被用到)。 

键盘控件与输出线的对应关系写在布局类的节里：就像你用Row\*.Col\*放置其它控件一样。你可以用 `'Keypad#'` 这么一行来说明键盘控件与输出线的对应关系。注意：在布局类的节里'#'的取值并不重要，只要保证它不与其它键的取值冲突就行。

定义一个键盘控件的方法如下：
```
Widget <name> {                                      # 控件名
    class      'Keypad'                              # 控件类型
    position   <confirm|cancel|up|down|left|right>   # 身份
    state      <pressed|released>                    # 触发状态
    expression <string>                              # 表达式
}
```

<br>

## 参数
|参数|含义|说明|
|-|-|-|
|position|身份|(控件)绑定的按钮是什么，默认绑定确定键(confirm)。|
|state|触发状态|是在按钮按下时触发还是释放时触发，默认是按下(pressed)时触发|
|expression|表达式|控件绑定的表达式(它将在触发后执行)。|

<br>

## 例子：
下面是一个 '用按键来增加显示的变量值' 的例子：
```
Widget mytext {
    class 'Text'
    expression foo
    update 100
}

Widget keyok {
    class 'Keypad'
    position 'confirm'
    expression foo = foo + 1
}

Variable {
    foo 1
}

Layout demokeypad {
    Row1.Col1 'mytext'
    Keypad1 'keyok'
}
```

一个更加复杂的例子，用按键来滚动输出的启动信息(dmesg)：
```
Widget dmesg1 {
    class 'Text'
    expression file::readline('/var/log/dmesg',currline)
    width 20
    update tick
}

Widget dmesg2 {
    class 'Text'
    expression file::readline('/var/log/dmesg',currline+1)
    width 20
    update tick
}

Widget dmesg3 {
    class 'Text'
    expression file::readline('/var/log/dmesg',currline+2)
    width 20
    update tick
}

Widget dmesg4 {
    class 'Text'
    expression file::readline('/var/log/dmesg',currline+3)
    width 20
    update tick
}

Widget keyup {
    class 'Keypad'
    position 'up'
    state 'pressed'
    expression currline = currline-1
}

Widget keydown {
    class 'Keypad'
    position 'down'
    state 'pressed'
    expression currline = currline+1
}

Layout showdmesg {
    Row1 {
        Col1 'dmesg1'
    }
    Row2 {
        Col1 'dmesg2'
    }
    Row3 {
        Col1 'dmesg3'
    }
    Row4 {
        Col1 'dmesg4'
    }
    Keypad1 'keyup'
    Keypad2 'keydown'
}

Variables {
    tick 100
    currline 1
}
```
一个 '用按键确认重新启动' 的例子，(右按键用于确认重启，左按键用于取消，重启用到了exec命令)：
```
widget keyRight{
        class 'Keypad'
        position 'right'
        state 'pressed'
        expression msg = 'Reboot? OK to confirm' ; rebootmenu = 1
}

widget keyConfirm{
        class 'Keypad'
        position 'confirm'
        state 'pressed'
        expression  msg = rebootmenu == 1 ? exec('/sbin/shutdown -r now',1000000) : 'Right arrow to reboot'
        exec('/sbin/shutdown -r now', 1000000);
}

widget keyLeft{
        class 'Keypad'
        position 'left'
        state 'pressed'
        expression msg = 'Right arrow to reboot' ; rebootmenu = 0
}

widget msgtxt {
        class 'Text'
        expression msg
        width 20
        update 100
}


Layout Default {
        Row1.Col1 'msgtxt'
	Row2.Col1 rebootmenu
        Keypad1 'keyRight'
        Keypad2 'keyConfirm'
        Keypad3 'keyLeft'
}


Variables {
   tick 500
   msg 'Right arrow to reboot'
   rebootmenu 0
}
```
