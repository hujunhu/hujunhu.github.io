---
layout: post
title: hexdump 的选项 -e
---
hexdump 是一个非常好用的十六进制查看工具。我最常用的命令是 hexdump -C，可以同时输出十六进制和对应字符。但是有时候，我们希望按指定格式输出，这时需要使用 -e 选项，形式如下：

{% highlight bash %}
hexdump -e '"format3" a1/a2 "format2" "format1"'
{% endhighlight %}

格式字符串由三部分组成，由空格分隔。上面的表达式指每行开始输出 format3，每 a2 个字节应用 format2，每 a1 个字节应用 format1。一般a1 > a2，且 a2 只能为1、2、4。format1、format2 和 format3 使用类似 printf 的格式字符串，如：

%02X：两位十六进制

%c：单个字符

此外，还有一些特殊的格式字符串：

%_a：标记下一个输出字节的序号

%_p：打印，对不能以常规字符显示的用.代替

同一行如果要显示多个格式字符串，则可使用多个 -e 选项组合。

一些例子：

``` bash
hexdump -e '16/1 "%02x " "\n"' filename
hexdump -e '"%08_aX  " 16/1 "%02X " "  \|"' -e '16/1 "%_p\|" "\n"' filename
```
