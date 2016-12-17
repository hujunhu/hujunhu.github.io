---
layout: post
title: Windows ipv6设置
---
Windows操作系统某些版本中，默认全局ipv6地址是随机生成，并且还有一个临时ipv6。按照微软的解释，这样做的目的是防止计算机被跟踪或逆向溯源，实现匿名访问ipv6网路。但是，这样的配置有时候会导致ipv6可用，传输速度却非常之慢，因此需要关闭这些功能，具体指令如下：

```
# 关闭全局地址随机生成功能
netsh interface ipv6 set global randomizeidentifiers=disable
# 关闭临时地址功能
netsh interface ipv6 set privacy state=disable
```
此外，Windows系统默认还开启了其他的几个ipv6接口，不需要的话，也可关闭。

```
netsh interface teredo set state disable
netsh interface 6to4 set state disable
netsh interface isatap set state disable
```
如果需要恢复为原来配置，根据需要执行下列指令：

```
netsh interface ipv6 set global randomizeidentifiers=enable
netsh interface ipv6 set privacy state=enable
netsh interface teredo set state default
netsh interface 6to4 set state default
netsh interface isatap set state default
```

