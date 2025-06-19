---
title: APS105预习Day3
date: 2025-06-19 13:50:20
tags: [APS105,C/C++,预习,笔记]
categories: [C/C++,APS105]
---

## 前言

第三天啦，继续看指针。这一期内容有点多

## Chapter 6 Pointers

指针一动心飞扬，调错地址泪两行。 -- 无家可归的野指针

### 6.1-6.3 指针

为了解释指针的作用，`swap`函数会是一个生动形象的例子。在朴素的理解中，如果要交换两个变量的值，可以使用如下函数

``````C
#include <stdio.h>
void swap(int, int);
int main(void) {
  int a = 9, b = 13;
  printf("Before swapping\nValue of a: %d\nValue of b: %d\n", a, b);
  swap(a, b);
  printf("After swapping\nValue of a: %d\nValue of b: %d\n", a, b);
  return 0;
}
void swap(int x, int y) {
  int temp = x;
  x = y;
  y = temp;
}
``````

**但是！**如果运行该段代码就会发现，该`swap`函数并不能交换`a`,`b`的值。那是因为函数的参数在C语言中遵循**按值传递**的规则，在函数运行时，会在主存中开辟一块新的内存存储函数中需要使用的所有变量

> 值得注意的是：此处所说的按值传递规则并不适用于数组，数组在传参的时候会自动退化为指针!

#### 指针的基本用法

在C语言中可以使用以下语句初始化一个指针：

``````C
int *p; // initialize a pointer
p = &x; // assign the memory location of x to p
``````

该段代码中

- `*`是解引用操作符， `*p`表示获取存储在`p`位置的数据
- `&`是引用操作符，`&x`表示获取`x`变量的内存地址

**指针的格式说明符是`%p`**

``````C
scanf("The value store at %p is %d", p, *p);
``````

所以，上述`swap()`函数的正确写法是：

``````c
#include <stdio.h>
void swap(int*, int*);
int main(void) {
  int a = 9, b = 13;
  printf("Before swapping\nValue of a: %d\nValue of b: %d\n", a, b);
  swap(&a, &b);
  printf("After swapping\nValue of a: %d\nValue of b: %d\n", a, b);
  return 0;
}
void swap(int* x, int* y) {
  int temp = *x;
  *x = *y;
  *y = temp;
}
``````

#### 指针的大小

在现代计算机中，所有的指针使用**64bit**存储，也就是**8个字节**

#### 套娃指针

指针可以存储另一个指针的地址，但是这时候指针的类型就变成了`**[type]`

#### 备注

`int *p;`等价于`int* p;`

#### 好习惯

在解引用指针之前，强烈建议先**检查指针是否为空**

``````c
#include <stdio.h>

int main() {
  int* p = NULL;

  if (p == NULL) {
    printf("Cannot dereference it!\n");
  } else {
    printf("The value at address p is %d.\n", *p);
  }

  return 0;
}
``````

### 6.4 作用域

老生常谈啦，变量必须得定义后才能使用，且只能在定义的作用域中生效，使用。

全局变量需要在`main`函数之前定义

#### 作用域重叠

``````C
#include <stdio.h>
int main(void) {
  int i = 1;
  printf("Outer i = %d.\n", i);
  {
    int i = 2;
    printf("Inner i = %d.\n", i);
  }
  printf("Outer i = %d.\n", i);
  return 0;
}
``````

例如这段代码，内层的变量`i`会遮蔽掉外层的`i`

### 6.5 实战 - 哥德巴赫猜想

哥德巴赫猜想指出：

> 每个大于 0 的偶数都可以表示为两个素数之和。

``````C
#include <math.h>
#include <stdbool.h>
#include <stdio.h>

bool isInputValid(int);
bool isPrime(int);
void nextPrime(int* px);
bool satisfyGoldBach(int);

int main(void) {
  int num;

  while (true) {
    printf("Please type in an even number that is greater than 2: ");
    scanf("%d", &num);
    if (isInputValid(num)) {
      break;
    } else {
      printf("Invalid value!\n");
    }
  }

  printf("The result is %d", satisfyGoldBach(num));
  return 0;
}

bool isInputValid(int num) { return (num > 2 && num % 2 == 0); }

bool isPrime(int x) {
  if (x <= 1) {
    return false;
  }

  for (int i = 2; i <= (int)sqrt(x); i++) {
    if (x % i == 0) {
      return false;
    }
  }
  return true;
}

bool satisfyGoldBach(int x) {
  for (int i = 2; i < x;) {
    if (isPrime(x - i) == true) {
      return true;
    } else {
      nextPrime(&i);
    }
  }
  return false;
}

void nextPrime(int* px) {
  do {
    (*px)++;
  } while (isPrime(*px) == false);
}

``````

**注意：在第55行，`++`运算符的优先级要高于`*`解引用运算符，所以要加括号**



## Chapter 7 Arrays

闲着没事干，再看一章吧。感觉自己还记得不少这章的内容，检验一下:smile:

### 7.1 数组基础

#### 声明一个数组

``````C
int grades[3] = {100,100,100};
``````

**在声明时赋初始值的情况下，可以不写数组的长度，C编译器会自动推导**

### 7.2 数组在计算机中的存储方式

在上面给出的`grade`数组中，`grade`作为`array identifier`同时也是一个**指向数组第一个元素的指针**

![数组标识符与数据获取](identifier-pointer.png)

### 7.3 把数组传递给函数

给函数来点大家伙！

不同与数值类型的按值传递，数组在传递的时候，**虽然标注的数据类型是`int []`，但是实际传入的是数组标识符**

可是！

**函数中的数组大小是未知的**，如果我们需要遍历数组，就需要**同时也讲数组的大小也传递进去！**

### 使用指针语法

既然数组标识符也是指针，那么……嘿嘿

没错！也可以使用指针语法来索引数组中的元素，以下是样例代码：

``````C
#include <stdio.h>

int sumData(int*, int);

int main(void) {
  int x[3] = {1, 7, 3};
  int result = sumData(x, 3);
  printf("Sum of elements in the array: %d.\n", result);
  return 0;
}

int sumData(int* list, int size) {
  int sum = 0;
  for (int index = 0; index < size; index++) {
    sum = sum + *(list + index);
  }
  return sum;
}
``````
