# 图像控件

> 官网地址：[https://lcd4linux.bulix.org/wiki/widget_image](https://lcd4linux.bulix.org/wiki/widget_image)
<br>

## 图像控件

该控件只在图形显示器上有效！ 

图像控件可以让你在显示器上放置一个单独的图像。 目前**只支持PNG格式的文件**，但支持全彩和透明度！ 

定义一个图像控件的方法如下： 
```
Widget myImage {         # 控件名
    class    'Image'     # 控件类型
    file     <string>    # 图片路径
    update   <number>    # 刷新频率
    reload   <0|1>       # 重新加载
    visible  <0|1>       # 可见性
    inverted <0|1>       # （颜色）反转
}
```
<br>

## 参数
|参数|含义|说明|
|-|-|-|
|file|路径|PNG文件的路径 。|
|update|刷新频率|刷新频率(单位：毫秒)，0 用于静态图像。|
|reload|重新加载|参数为表达式，该表达式用于控制图像在刷新时重新加载与否(即可以通过这个属性来实现更换图片)。|
|visible|可见性|参数为表达式，该表达式用于控制图像的可见性(用于闪烁效果)。|
|inverted|反转|参数为表达式，用于控制(颜色)反转(用于在显示器的暗色背景上绘制白色像素点)。|

<br>

## 布局
图像控件并不放在固定的行或者列中(因为当你在显示类的节里改变了字体大小后，行和列的数目/位置就改变了，而图像却不能随它而变)，它的位置是由真实的像素坐标确定。所以这个布局类的节看起来有点不一样：
```
Layout TestImage {
    Row1 {
	Col1  'OS'
    }
    Layer 2 {
	X1.Y1 'ImageTest'
    }
}
```
注意：把图像放在额外的图层中是非常有用的！ 
<br>

## 例子
```
Widget ImageTest {
    class 'Image'
    file '/home/michi/avatar.png'
    update 1000
    visible 1
    inverted 0
}

Layout Avatar {
    X1.Y1 'ImageTest'
}
```
<br>

## 图像重加载
当 `reload` 参数的值为 0 时，图像文件将不会在刷新时重新加载。所以只有 `visible` 和 `inverted` 参数会改变显示内容。如果你想每次都重新加载图像(可能是因为图像改变了)，那么请把 `reload` 的参数值设为 1 。
<br>

## 可见性
 你可以为 `visible` 参数指定一个表达式，它将控制(每个刷新毫秒内)图像的刷新(即控制图像在刷新时显示与否)。如果这个这个表达式的值为0，那么图像将不可见，(图像的内容)将由透明的像素点代替。如果表达式的值不为0的话，图像将会显示。这对于制造闪烁效果和动态显示来说非常有用。
 