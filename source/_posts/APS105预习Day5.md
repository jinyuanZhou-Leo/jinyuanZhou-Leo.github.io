---
title: APS105预习 - 05
date: 2025-06-26 21:43:01
tags: [APS105,C/C++,预习,笔记]
categories: [C/C++,APS105]
cover_image: /assets/images/APS105-cover.webp
---

## 前言

因为一些原因断了几天，今天接着看吧

## Chapter 9 Multi-dimensional Arrays 多维数组

### 9.1 Declaration

声明二维数组的方式和声明一维数组的方式十分类似

``````c
int myArray[2][3]; //Declare a matrix of 2 rows and 3 columns
``````

#### Initialization 初始化

二维数组的初始化也十分直觉

``````C
int myArray[2][3] = {{1,2,3},{1,2,3}};
int myArray[2][3]= {1,2,3,4,5,6};
int myArray[][3] = {1,2,3,4,5,6};
``````

### How do multi-dimensional arrays look like in memory

二维数组的元素是按行依次存储在内存中的(row major order)。

同样的，二维数组的数组标识符是第一个元素地址的别名，**即`&myArray[0][0]`**

所以可以很容易推导得出二维数组每一个元素的内存地址公式：

```````C
&myArray[i][j] = &myArray[0][0] + (i * num of columns + j) * sizeof(int)
```````

#### 二维数组指针的解引用

当你试图解引用二维数组的数组标识符的时候(`*myArray`)， **返回的是一个只想数组第一行第一个元素的指针，而不是返回值**。

这可能有些难理解，用符号表示便是:

``````C
myArray = *myArray = &myArray[0][0]
``````

举个例子，如果给二维数组的标识符+1，那么会得到指向第二行第一个元素的指针

``````C
*(myArray + 1) = myArray[1]
``````

所以，当我们要访问指定的位置的时候，我们可以这样做

```````C
*(*(myArray + row) + column)
```````

 ####  9.1.3 练习 - 查找三个水平连续的1

``````C
#include <stdio.h>

int main(void) {
  int board[6][6] = {
      {0, 1, 1, 0, 0, 0}, {0, 1, 0, 1, 1, 0}, {0, 1, 1, 1, 0, 0},
      {0, 0, 1, 1, 1, 0}, {0, 0, 0, 1, 1, 0}, {1, 1, 1, 0, 1, 1},
  };

  int cnt = 0;
  for (int row = 0; row < 6; row++) {
    for (int col = 0; col < 6; col++) {
      if (col < 4) {
        if (board[row][col] == 1 && board[row][col + 1] == 1 &&
            board[row][col + 2]) {
          cnt++;
        }
      }
    }
  }

  printf("Count: %d", cnt);
  return 0;
}
``````

### 9.2 二维数组传参

假如我们需要写一个函数，该函数接受指向二维数组第一个元素的指针，行数，列数，将该二维数组**行索引4**和**列索引5**的元素改为**6**

以下是一个看似正确的典型错误代码

```````c
void func(int arr2D[][], int rows, int cols){
    arr2D[4][5] = 6; 
    // should go dereference the address:
    // arr2D + row (= 4) * number of columns + col (= 5)
    // the number of columns is unknown!!!
}
```````

因为我们要参照公式去推导得到元素的内存地址

``````c
&myArray[i][j] = &myArray[0][0] + (i * num of columns + j) * sizeof(int)
``````

而此时，我们又不知道`arr2D`的`row`，故无法得到内存地址



所以正确的写法应该是**给出二维数组的列数**

``````C
void func(int rows, int cols, int arr2D[][cols]){
    arr2D[4][5] = 6; 
    // should go dereference the address:
    // arr2D + row (= 4) * cols + col (= 5)
}
``````

**注意⚠️：`cols`需要在二维数组前声明**

