# 配置文件

> 官网地址：[https://lcd4linux.bulix.org/wiki/lcd4linux_conf](https://lcd4linux.bulix.org/wiki/lcd4linux_conf)
<br>

## 配置文件

配置文件 (默认位置: /etc/lcd4linux.conf) 包含LCD4Linux的配置和布局. 从0.10版本开始, 配置文件的格式发生了很大的变化, 现在已经能够更加灵活地使用它了。

**注释符为符号“#”**，任何在“#”号之后的文本(或空行)将会被注释掉， 如果你想要在值(value)中使用“#”号，你需要在引用它时加上反斜杠“\” . 空行或者只有空格的行也将被注释掉。

配置文件由“节”、“分支”和“参数” 组成，节和参数对于自己起名(name)并不敏感，但是键(key)的起名却不能包含空格。

配置文件中一个参数的格式是这样子的：
```
name value
```

**注意**: 配置文件里的每个值(value)都将被视为一个表达式来处理, 所以它们会被执行。 这意味着在正常情况下,你需要把一个value用单引号括起来，以使它的计算结果为一个字符串！ （PS：意思是如果value用引号括号括起来，就是str(value)；不用引号括起来，就是eval(value)）

例如像这样: 
```
变量名      值
name1    value1
name2    'value2'
name3    '2+3'
name4    2+3
name5    '42'
name6    42
```
其中：
*name1* 的值将会从表达式value1那里得到，它可能返回0或者一个空字符串，一般情况下它并不是你想要的。 (除非value1是一个变量的名字).

*name2* 返回字符串”value2“，这个才是正确的方法。

*name3* 返回字符串'2+3'

*name4* 返回表达式2+3执行后的值，也就是5

*name5* 返回字符串'42'(或者数字42)

*name6* 返回值与name5相同

<br>

**"节"** 看起来长这样:
```
section_a {
   name1 value1
}
section_b name_b {
   name2 value2
}
section_c name_c {
   name3 value3
   section_d {
      name4 value4
   }
}
```
所以说一个节是可以不具有名字的.

**"节"** 也可以写成 紧凑的形式 , 所以下面的语句与上面的语句是完全相同的：
```
section_a.name1 value1
section_b:name_b.name2 value2
section_c:name_c.name3 value3
section_c:name_c.section_d.name4 value4
```
**节** 和节的名称之间用冒号(':')相连接；”节“，”分支“，”键“之间用点('.')相连接。 

这里有一个更加贴切的例子：下面两个条目(从布线结构上来说)是等效的： 
```
Wire.EX   'STROBE'
Wire.IOC1 'SELECT'
Wire.IOC2 'AUTOFD'
Wire.GPO  'INIT'
```

```
Wire {
   EX   'STROBE'
   IOC1 'SELECT'
   IOC2 'AUTOFD'
   GPO  'INIT'
}
```

<br>

## 节的选择
注意，*name value { }* 和 *name value* 是不同的： 第一个是定义一个”节“，它的名字叫value；第二个value是一个”参数“，表示把参数value传递给name，虽然它们的名字是相同，但第二种类型通常只用于选择一个特定的节。 

请看下面的例子: 
```
Display My_20x4 {
   ...
}

Display My_16x2 {
   ...
}

Display My_16x2
```
在上面的例子中，你定义了两个显示类的节，它们具有不同的名字。然后你选择了其中的一个节。

<br>

## 特殊的节

有些节有一些特殊的含义：
  - ~[显示设备](https://lcd4linux.bulix.org/wiki/Displays)~：用于配置显示方法的一个配置类，需要依赖于显示器(即LCD4Linux硬件)和驱动
  - [插件类](https://github.com/enify/lcd4linux-doc/blob/master/wiki/Plugins.md)：特殊插件的一个配置类(很少使用)
  - [控件类](https://github.com/enify/lcd4linux-doc/blob/master/wiki/Layout.md)：用于定义控件的配置类
  - [布局类](https://github.com/enify/lcd4linux-doc/blob/master/wiki/Layout.md)： 用于定义控件在显示器上显示的方法和位置
  - 变量：用于定义变量

下面是一个简单的配置示例：
```
Display HD44780-20x2 {
    Driver  'HD44780'
    Model   'generic'
    UseBusy  1
    Port    '/dev/parport0'	
    Size    '20x2'
    Wire {
	RW      'AUTOFD'
	RS      'INIT'
	ENABLE  'STROBE'
	ENABLE2 'GND'
	GPO     'GND'
    }
}


Widget CPU {
    class  'Text'
    expression  uname('machine')
    prefix 'CPU '
    width  9
    align  'L'
    update tick
}

Layout Default {
    Row1 {
        Col1 'CPU'
    }
}

Variables {
   tick 500
}

Display 'HD44780-20x2'
Layout  'Default'
```
总的来说，一个配置文件应该包含以下几个方面：
  - 至少包含一个 ~[显示类](https://lcd4linux.bulix.org/wiki/Displays)~的节, 它用来描述和配置你的显示器。
  - 包含若干个[控件类](https://github.com/enify/lcd4linux-doc/blob/master/wiki/Layout.md)的节, 它用于定义输出到显示器的数据的内容和显示方法。
  - 包含[布局类]((https://github.com/enify/lcd4linux-doc/blob/master/wiki/Layout.md))的节，它定义了控件的显示的位置和方法。

<br>

## 安全方面

由于一些安全方面的原因(配置文件内可能包含邮件账户的用户名/密码)， **要使配置文件生效，需要满足以下条件：**
  - 配置文件是正确的(或者 /dev/null)
  - 配置文件的所有者与程序的所有者相同
  - 配置文件无法通过"组"权限访问
  - 配置文件无法通过"其它"权限访问

所以，如果你以root权限运行LCD4Linux，你应该这样设置你的配置文件(/etc/lcd4linux.conf)：
```
chmod 600
chown root.root
```
