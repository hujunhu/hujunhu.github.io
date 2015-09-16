---
layout: post
title: printf和scanf输入输出浮点数
---
在C语言中，浮点数包括单精度浮点数float和双精度浮点数double两种。

下面是它们分别在输出和输入情况下的对比。

## printf输出浮点数

printf在输出float与double类型数据时，格式化字符%f与%lf没有区别，一般使用%f。
printf在接收float类型数据时会自动转换为double。

PS：严格按照C标准手册，%f对应double，%lf未定义，%Lf对应long double。

## scanf输入浮点数

按照C标准手册，%f仅对应float，%lf对应double，%Lf对应long double。

PS：对于unsigned char，应使用%hhu格式化；unsigned short int使用%hu。
