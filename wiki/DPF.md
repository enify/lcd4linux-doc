# DPF设备

> 官网地址：[https://lcd4linux.bulix.org/wiki/DPF](https://lcd4linux.bulix.org/wiki/DPF)
<br>

> 入口地址：首页 \> Displays \> DPF

## 显示类的节
```
Display <name> {               # 控件名
    Driver       'DPF'         # 设备名
    Port         '<string>'    # 端口
    Font         '<string>'    # 字体大小
    Orientation  <number>      # 屏幕方向
    Backlight    <number>      # 背光亮度
    Foreground   color in hex  # 前景色
    Background   color in hex  # 背景色
    Basecolor    color in hex  # 底色
}
```

<br>

## 参数
|参数|含义|说明|
|-|-|-|
|Dirver|设备名|'DPF'|
|Port|端口|正常情况下是'usb0'.如果你连接了多个DPF设备的话: 'usb0"是第一个设备, 'usb1'是第二个设备，以此类推。|
|FontFont|字体大小|默认值为'6x8'，设置成其它大小也是有效的。|
|Orientation|屏幕方向|DPF设备的逻辑方向。对于高度不等于宽度的DPF设备来说：0 = 横向( landscape)，1 = 纵向(portrait)，2 = 反转横向(reverse landscape)，3 = 反转纵向(reverse portrait)；对于高度等于宽度的DPF设备来说：0 = 正常方向，1 = 顺时针旋转90°，2 = 旋转180°，3 = 逆时针旋转90°。注意：旋转是依靠驱动的，与DPF设备的实际(物理)方向无关。|
|Backlight|背光亮度|设置DPF设备的背光/亮度。值: 0-7 (0 = 关闭(off), 7 = 最大亮度(max))。|
|Foreground|前景色|文字的颜色。|
|Background|背景色|背景的颜色。|
|Basecolor|底色|...|

<br>

## 例子
```
Display dpf {
   Driver 'DPF'
   Port 'usb0'
   Font '8x10'
   Orientation 0
   Backlight 7
   Foreground 'ffffff' #white
   Background '000000' #black
   Basecolor 'ff0000' #red
}
```

<br>

## 依赖
这个显示类是自带的，不需要依赖于外部的库。
