---
layout: post
title: Verilog调用模块注意事项
date: 2019-4-21
categories: blog
tags: [Verilog,模块,调用]
description: 无
---
在verilog中调用模块主要有两种写法：

1，第一种需要将模块变量与所调用的模块的端口顺序摆放一致。

   一个简单的全加器模块描述如下：

module adder（a,b,cin,s,cout）;
   input a,b,cin;
   output cout,s;
   assign {cout,s} = a + b + cin;
endmodule
  若一个模块temp需要调用adder模块时，temp中的与adder想连的端口需要与adder中声明的端口顺序一致。端口的介绍，可以参阅点击打开链接。 调用首先写被调用模块的名称(adder)  ,随后的是实例名（add，用户自己定义），然后按adder中端口的顺序写下实例的端口名即可。

module temp（B,A,COUT,CIN,Sum);
...
    adder  add(A,B,CIN,Sum,Cout);
...
endmodule
  若不需要全部的端口则如下调用： 

adder add(A , , CIN , , COUT);
 此实例中没有引用adder中的b，s两个端口，故省略，但逗号一定要保留。

2，第二种调用端口顺序可以随意摆放，同样以调用adder模块为例。

module temp（B,A,COUT,CIN,Sum）;
...
...
 adder  add(.a(A), .s(Sum), .b(B), .cout(Cout), .cin(CIN));
...
endmodule
当不需要调用所有端口时，可如下调用:

 adder  add(.a(A), .s(), .b(), .cout(Cout), .cin(CIN));
此实例中没有引用adder中的b，s两个端口.

良好的coding style推荐使用第二种模块调用方式
