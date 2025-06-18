---
title: APS105预习Day2
date: 2025-06-18 11:32:08
tags: [APS105,C/C++,预习,笔记]
categories: [C/C++,APS105]
---
## 前言

非常好，这个系列居然能来到第二天🔥

## Chapter 2 Data operation and representation

### 2.5 Random

在C中，生成随机数的操作依赖 `stdlib.h`库。

**需要注意的是，C中的rand函数是伪随机数**

伪随机数的生成依托于一个种子（seed），当种子相同的时候，生成的数字也会相同，在C中我们使用`srand()`来指定随机数生成器所使用的种子

#### 我们真的在生成随机数吗？？

为了生成更加**随机**的随机数，可以使用来自`time.h`库中的`time()`函数来获取当前时间作为种子。当调用`time(NULL)`时，该函数会返回一个UNIX时间（即1970.1.1以来的秒数）

以下是一个基于时间的随机数代码

``````c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main(void) {
  srand(time(NULL));
  for (int i = 0; i < 3; i++) {
    printf("Random number %d is %d\n", i, rand());
  }
  return 0;
}

``````

#### 控制rand生成数字的大小

不同于Python等其他高级语言，C中的`rand()`函数没有提供如此丰富的高级功能。回顾之前的内容，当取模运算时，**任何模运算的结果都在0和2nd operand - 1之间**

利用这个特性，如果想得到0-3之间的数我们只需要：

``````c
int result = rand() % 4;
``````

扩展一下上面的结论，我们很容易得到一个通式：

``````C
int result = rand() % (MAX-MIN+1) + MIN; // [MIN,MAX]
``````

## Chapter 3 Decision-Making Statements

耶！下一章

If-else就略过，稍稍记一点点🤏

### 3.2 Multiple Choices

作为一个合格的s，C当然不会允许形如`0<=x<=10`这种 Pythonic 的写法啦

#### Lazy Evaluation

1. **<LHS>** `||` **<RHS>**: the *<LHS>* will be evluated first, if `true`, the whole condition is `true` and the program will not evaluate the *<RHS>*. If *<LHS>* is `false`, the program will evaluate the *<RHS>*.
2. **<LHS>** `&&` **<RHS>**: the *<LHS>* will be evaluated first, if `false`, the whole condition is `false` and the program will not evaluate the *<RHS>*. If *<LHS>* is `true`, the program will evaluate the *<RHS>*.

#### De Morgan's Law 德摩根定律

括号外的NOT运算符分配至括号内后AND变OR，OR变AND

1. `!(A && B)` is equivalent to `!A || !B`
2. `!(A || B)` is equivalent to `!A && !B`

![De Morgan's Law Example](de-morgan-law-exp.png)

## Chapter 4 Repetition

伟大的脑子啊，这章也记得（傲娇脸

#### Compact for loop

``````C
for(int i = 1; i<=3; printf("%d",i), i++)
	;
``````

啊……对，结束了，下一章

## Chapter 5 Functions

不同于Python，C语言当中定义函数并不靠某些特定的关键词，而是使用函数的返回值类型，举个例子：

``````c
type fooName(type paramName){
  code...
  return ...
}
``````

在`int main(void)`之前，需要写下函数的原型(Function Prototypes)，形如

``````C
type fooName(type);
// OR
type fooName(type paramName);
``````

#### 为何不直接在函数原型下面实现函数？

编译器会从程序顶部开始编译，此时如果你直接实现了函数，那么函数的实现顺序会变得很重要，因为所有函数中调用的函数都需要在被调用之前被声明。

相反，如果在main函数之前声明函数原型，那么在main函数之后函数的实现顺序便不重要了
