# 插件

> 官网地址：[https://lcd4linux.bulix.org/wiki/Plugins](https://lcd4linux.bulix.org/wiki/Plugins)
<br>

## 插件
[计算/求值模块](https://github.com/enify/lcd4linux-doc/blob/master/wiki/Evaluator.md)可以以插件的形式扩展额外的功能，每个插件都至少提供一个函数。他们可能由显示设备所提供，你可以在~[这里](https://lcd4linux.bulix.org/wiki/Displays)~找到一些资料。

如果一个插件只提供一个函数，那么函数的命名应该和插件名一样； 如果一个插件提供了多个函数，那么所有的函数的命名都应该添加上前缀 `'plugin_name::'`，即：**命名格式为： '插件名::函数名'** 。LCD4Linux未来将支持动态加载（现在，所有插件都是静态地链接到可执行文件，将来LCD4Linux将支持按需动态加载插件）。

- ~[APM plugin](https://lcd4linux.bulix.org/wiki/plugin_apm)~
- ~[Asterisk plugin](https://lcd4linux.bulix.org/wiki/plugin_asterisk)~
- ~[Config plugin](https://lcd4linux.bulix.org/wiki/plugin_cfg)~
- ~[/proc/cpuinfo plugin](https://lcd4linux.bulix.org/wiki/plugin_cpuinfo)~
- ~[/proc/diskstats plugin](https://lcd4linux.bulix.org/wiki/plugin_diskstats)~
- ~[DVB plugin](https://lcd4linux.bulix.org/wiki/plugin_dvb)~
- ~[exec (external command) plugin](https://lcd4linux.bulix.org/wiki/plugin_exec)~
- ~[FIFO plugin](https://lcd4linux.bulix.org/wiki/plugin_fifo)~
- ~[file reading plugin](https://lcd4linux.bulix.org/wiki/plugin_file)~
- ~[GPS (NMEA) plugin](https://lcd4linux.bulix.org/wiki/plugin_gps)~
- ~[hddtemp (hard disk temperature)](https://lcd4linux.bulix.org/wiki/plugin_hddtemp)~
- ~[HUAWEI plugin](https://lcd4linux.bulix.org/wiki/plugin_huawei)~
- ~[I2C sensors plugin](https://lcd4linux.bulix.org/wiki/plugin_i2c_sensors)~
- ~[iconv (charset converter) plugin](https://lcd4linux.bulix.org/wiki/plugin_iconv)~
- ~[ISDN Monitor (imon) plugin](https://lcd4linux.bulix.org/wiki/plugin_imon)~
- ~[ISDN plugin](https://lcd4linux.bulix.org/wiki/plugin_isdn)~
- ~[KVV plugin](https://lcd4linux.bulix.org/wiki/plugin_kvv)~
- ~[/proc/loadavg plugin](https://lcd4linux.bulix.org/wiki/plugin_loadavg)~
- ~[mathematical functions plugin](https://lcd4linux.bulix.org/wiki/plugin_math)~
- ~[/proc/meminfo plugin](https://lcd4linux.bulix.org/wiki/plugin_meminfo)~
- ~[MPd plugin](https://lcd4linux.bulix.org/wiki/plugin_mpd)~
- ~[MPRIS plugin](https://lcd4linux.bulix.org/wiki/plugin_mpris_dbus)~
- ~[MySQL plugin](https://lcd4linux.bulix.org/wiki/plugin_mysql)~
- ~[/proc/net/dev plugin](https://lcd4linux.bulix.org/wiki/plugin_netdev)~
- ~[netinfo plugin](https://lcd4linux.bulix.org/wiki/plugin_netinfo)~
- ~[POP plugin](https://lcd4linux.bulix.org/wiki/plugin_pop3)~
- ~[PPP plugin](https://lcd4linux.bulix.org/wiki/plugin_ppp)~
- ~[/proc/stat plugin](https://lcd4linux.bulix.org/wiki/plugin_proc_stat)~
- ~[Python plugin](https://lcd4linux.bulix.org/wiki/plugin_python)~
- ~[Qnaplog plugin](https://lcd4linux.bulix.org/wiki/plugin_qnaplog)~
- ~[RasPi plugin](https://lcd4linux.bulix.org/wiki/plugin_raspi)~
- ~[Example plugin](https://lcd4linux.bulix.org/wiki/plugin_sample)~
- ~[SETI plugin](https://lcd4linux.bulix.org/wiki/plugin_seti)~
- ~[statfs plugin](https://lcd4linux.bulix.org/wiki/plugin_statfs)~
- ~[string functions plugin](https://lcd4linux.bulix.org/wiki/plugin_string)~
- ~[Test plugin](https://lcd4linux.bulix.org/wiki/plugin_test)~
- ~[Time plugin](https://lcd4linux.bulix.org/wiki/plugin_time)~
- ~[uname plugin](https://lcd4linux.bulix.org/wiki/plugin_uname)~
- ~[uptime plugin](https://lcd4linux.bulix.org/wiki/plugin_uptime)~
- ~[WLAN plugin](https://lcd4linux.bulix.org/wiki/plugin_wireless)~
- ~[XMMS plugin](https://lcd4linux.bulix.org/wiki/plugin_xmms)~ 

- ~[ 如何编写新的插件](https://lcd4linux.bulix.org/wiki/plugin_howto)~
