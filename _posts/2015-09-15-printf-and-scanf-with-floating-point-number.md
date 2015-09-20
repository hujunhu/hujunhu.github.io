---
layout: post
title: printf 和 scanf 输入输出浮点数
---
在 C 语言中，浮点数包括单精度浮点数 float 和双精度浮点数 double 两种。

下面是它们分别在输出和输入情况下的对比。

## printf 输出浮点数

printf 在输出float 与double 类型数据时，格式化字符 %f 与 %lf 没有区别，一般使用 %f。
printf 在接收 float 类型数据时会自动转换为 double。

PS：严格按照 C 标准手册，%f 对应 double，%lf 未定义，%Lf 对应 long double。

## scanf 输入浮点数

按照 C 标准手册，%f 仅对应 float，%lf 对应 double，%Lf 对应 long double。

PS：对于unsigned char，应使用 %hhu 格式化；unsigned short int 使用 %hu。
