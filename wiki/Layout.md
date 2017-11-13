# 控件与布局

> 官网地址：[https://lcd4linux.bulix.org/wiki/Layout](https://lcd4linux.bulix.org/wiki/Layout)
<br>

## 布局与控件类
  - [文本控件](https://github.com/enify/lcd4linux-doc/blob/master/wiki/widget_text.md)
  - [栏控件](https://github.com/enify/lcd4linux-doc/blob/master/wiki/widget_bar.md)
  - [图符控件](https://github.com/enify/lcd4linux-doc/blob/master/wiki/widget_icon.md)
  - [图像控件](https://github.com/enify/lcd4linux-doc/blob/master/wiki/widget_image.md)
  - [定时器控件](https://github.com/enify/lcd4linux-doc/blob/master/wiki/widget_timer.md)
  - [通用输出控件](https://github.com/enify/lcd4linux-doc/blob/master/wiki/widget_gpo.md) (包括通用输出与输入)
  - [键盘控件](https://github.com/enify/lcd4linux-doc/blob/master/wiki/widget_keypad.md)

LCD4Linux 0.10使用了全新的布局引擎，替换了原来的基于令牌(token-based )的引擎。整个布局由若干个控件组成，放在 **布局类的节** 内。

注意：LCD4Linux没有全局的显示更新事件或者显示刷新。每个控件都有它自己的更新时间间隔，只有到每个时间间隔过去后控件才会重新显示(即刷新)。控件的这个刷新时间间隔并不一定都要一样(虽然刷新时间间隔一样的话会显得更整齐一点)。同时，把刷新事件间隔设置为变量的话可能是个不错的想法。

LCD4Linux广泛地使用了双缓冲技术，每个控件先渲染进入一个布局帧，然后这个布局帧的缓冲区与显示帧对比，将不同之处传递到显示器。所以尽管有时控件的刷新频率很高，但只有当值改变时，才会有数据传递到显示器(节省了大量带宽和CPU周期)。 

显示帧通常与显示器的尺寸相同，但是布局帧会随着布局的增大而自动增大，因此布局帧可能比显示器的尺寸还要大(例如：你有一个20x4的显示器，然后你在布局类中定义了一个尺寸为5x6的控件，这时显示帧的尺寸将会是5x4，而布局帧的尺寸会是5x6 )。当显示器刷新时，显示帧作为布局帧的一个窗口(显示出来)，目前这个窗口固定在(布局帧的)左上角，在将来的LCD4Linux版本这个窗口将支持(在布局帧上)移动。-----为了在LCD4Linux-0.9版本上实现一些类似于虚拟行(virtual rows)的功能。

每个控件都有一个名字(name)(即"节"的名字)和一个具体的类，用来描述控件的种类(例如：文本(text)，栏(bar)，图符(icon)，定时器(timer)，通用输出(gpo)...)，你可以在相应插件的描述页面找到详细的说明。

下面是一个建立控件的例子：
```
Widget RAM {                               # 控件名
    class      'Text'                      # 控件类型
    expression  meminfo('MemTotal')/1024   # 表达式
    postfix    ' MB RAM'                   # 前缀
    width      11                          # 宽度
    precision  0                           # 精度
    align     'R'                          # 对齐方式
    update     1000                        # 刷新频率
```
在例子中，我们定义了一个名为RAM的控件：
  - 它是一个文本控件 (class 'Text' )
  - 它的值来自于内存信息插件 (meminfo plugin)
  - 它的值有一个后缀文本 (postfix)
  - 整个控件的宽度为11个字符 (width 11)
  - 它的值没有小数点 (precision 0)
  - 它的值将与右边对齐 (align 'R' )
  - 控件每1000毫秒刷新一次 (update 1000)  

<br>

## 布局类
你已经定义了一些控件，接下来你需要告诉LCD4Linux在何处显示你的控件。这就是 **布局类** 所做的事。

很容易就可以定义一个布局类的节：
```
Layout L20x2 {                 # 布局名
    Row1 {                     # 第一行
        Col1  'Busy'           # 第1列
        Col11 'BusyBar'        # 第11列
    }
    Row2 {                     # 第二行
        Col1  'Load'           # 第1列
        Col11 'LoadBar'        # 第11列
    }
    Timer1 'PollFan'           # 定时器控件
    GPO1   'ISDN_connected'    # 通用输出控件
}
```
在上例中，我们定义了一个名为 'L20x2' 的布局，它有两行(Row1和Row2)，然后我们把名为'Busy' , 'BusyBar' , 'Load' , 'LoadBar' 的四个控件分别放到特定的列。然后添加了一个定时器控件。我们还有一个发光二极管接在显示器的通用输出口1上，它由一个名为 'ISDN_connected' 的控件控制。

> 注意：LCD4Linux对于布局类的节的数量没有限制，但生效的节就只有你用 Layout '布局名' 字段选择的那个。在将来的版本，LCDLinux可能会支持自动切换布局(通过按键，定时器事件，或者其它什么的)。

<br>

## 图层

LCD4Linux支持图层，默认状态下有三个图层，但是还可以再添加。图层可以用来生成重叠的控件，这在图像控件里面是最有用处的(仅对图形显示器有效)。在每个控件都有独立的RGBA颜色的情况下，你可以在图层上使用Alpha透明混合处理(alpha-blending)。(注:RGBA是代表Red(红色) Green(绿色) Blue(蓝色)和Alpha的色彩空间(不透明度).) 

如果在布局类的节里你没有使用图层，那么默认地会使用图层1，图层0是最顶层的图层，这就意味着如果在图层0上有一个不透明的控件的话，它将把所有其它的控件都给遮住。

我们再用一个例子来解释它是如何工作的：
```
Layout with_Image {
    Row1 {                 # 默认放在图层1上
	Col1  'OS'
    }
    Layer 0 {              # 图层0
        Row6 {
            Col1  'CPU'
            Col10 'RAM'
        }
    }
    Layer 2 {              # 图层2
	X1.Y1 'my_Image'
    }
}
```
在上面的例子中，名为OS的控件使用默认图层1，名为CPU和RAM的控件放在图层0上，然后把背景图片放在图层2上，坐标X=1 , Y=1 。