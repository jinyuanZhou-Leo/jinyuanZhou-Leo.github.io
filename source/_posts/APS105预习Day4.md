---
title: APS105预习Day4
date: 2025-06-20 13:32:33
tags: [APS105,C/C++,预习,笔记]
categories: [C/C++,APS105]
cover_image: /assets/images/APS105-cover.png
---

## 前言

第四天！这章过后APS105也算是过半了

## Chapter 8 Dynamic Memory Allocation 动态内存分配

浅浅recap一下Ch 7的内容，数组的大小是不可以改变的。可是如果我就是一身反骨，想要一个动态大小的数组呢？没错，**动态内存分配有助于动态改变我们所使用的数组的大小**

### 8.1 什么是动态内存分配

#### 计算机内存管理基础

当一个程序运行的时候，占用的内存空间会被分成4个段

- 程序代码
- 程序的常量和全局变量
- 程序中动态分配的内存， 称为**堆（Heap）**
- 函数的局部变量，称为**栈（Stack）**

![内存分配图](memory-piece.png)

正如上图所示，堆栈之间**并没有**明确的分界线，如果说程序的堆栈耗尽，就无法创建或调用其他函数。

> 冷知识：不断的递归调用会[Stack Overflow](https://stackoverflow.com/)

#### 堆上动态分配内存

堆上动态分配内存主要使用以下函数

`void *malloc(size_t size);`

该函数接受一个`size`函数，表示分配的字节数。返回指向堆中分配的数组第一个字节的指针

例如，要动态分配5个元素到一个`int`数组中

``````C
int* myArray = (int*) malloc(5*sizeof(int));
``````

**当malloc函数无法在堆中分配内存的时候，会返回NULL，所以在使用其返回值前，请先检查是否为NULL**

#### 内存泄露

如果数组是在一个函数中动态分配的，函数执行结束后（该数组超出其作用域后），数组会依然存储于堆中。可是，**此时保存其第一个元素地址的指针也会超出其作用域**。**所以，在函数返回值之前，一定一定要记得释放分配的内存**

以下是一个典型的内存泄露程序：

``````C
#include <stdlib.h>

double getAverage(int);

int main(void) {
  int size;
  printf("Enter size of array:");
  scanf("%d", &size);
  double avg = getAverage(size);
  printf("Average is %.2lf\n", avg);
  return 0;
}

double getAverage(int size) {
  int* myArray = (int*)malloc(size * sizeof(int));
  
  printf("Enter grades:");
  for (int index = 0; index < size; index++) {
    scanf("%d", &myArray[index]);
  }
  int sum = 0;
  for (int index = 0; index < size; index++) {
    sum += myArray[index];
  }
  return (double)sum / size;
``````

在第 26 行，我们最终将平均值返回给 `main` 。当我们返回到 `main` 时，包括 `myArray` 在内的所有局部变量都将从栈中消失。然而，动态分配的内存仍然存在于堆中。由于我们失去了保存第一个元素地址的变量，因此无法再访问这个数组。我们确实没有任何办法可以接触到这块动态分配的内存空间。**这种现象被称为内存泄漏，即我们失去了对一个为变量/数组保留的内存空间的访问权限。**

#### 释放内存

做事情得善始善终，分配的内存得及时释放

释放内存主要使用以下函数：

`void free(void *pointer);`

该函数接受一个指针`*pointer`，指向你想要释放的动态分配内存的第一个元素的地址。注意，及时我们使用`free()`将内存清空后，`*pointer`依旧存储着一个已经被释放的内存地址，所以，此时的最佳实践是**在内存释放后将`*pointer`设置为NULL以防止未定义的访存行为**







