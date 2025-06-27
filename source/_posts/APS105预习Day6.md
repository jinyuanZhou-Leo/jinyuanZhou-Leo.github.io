---
title: APS105预习 - 06
date: 2025-06-27 16:13:18
tags: [APS105,C/C++,预习,笔记]
categories: [C/C++,APS105]
cover_image: /assets/images/APS105-cover.webp
---

## Chapter 9 Multi-dimensional Array 多维数组

### 9.2.1 练习

编写一个使用函数来求二维数组所有元素之和的程序

### 9.3 二维数组的动态内存分配

#### 方法1：动态分配指针数组

这种方法较为直观，为二维数组的每一行都创建一个指针，指向该行第一个元素的位置。用如下代码即可实现

``````c
int** arr = (int**) malloc(3*sizeof(int*));
``````

然后，我们需要将该指针数组的每一位赋值成**指向每一行第一个元素的指针**

``````c
for (int row = 0; row<3; row++){
	*(arr+row) = (int*)malloc(3*sizeof(int));
  //这里也可以写成arr[row]
}
``````

如果需要遍历或者赋值，可以使用如下For循环：

``````c
for(int row = 0; row<3; row++){
	for(int col = 0; col<3; col++){
    *(*(arr+row)) = 3*row + col+1;
    // Equivalent to arr[row][col]
  }
}
``````

使用完毕后，一定要记得释放使用的内存。**注意，二维数组在释放内存的时候，释放的顺序非常重要**。如果我们先释放`arr`,会导致`arr[row]`变得不可访问。所以，在释放二维数组内存的时候，**务必先释放指针数组的每一位，再释放指针数组**

``````c
for(int row = 0; row<3; row++){
	free(arr[row]);
  arr[row] = NULL; // VERY IMPORTANT
}

free(arr);
arr = NULL;
``````

#### 方法2: 指针数组静态分配

这种方法我们选择使用静态栈内存来存储指针数组，这种方法的问题是，该数组的长度（二维数组的行数）必须在编译时已知。

该方法的其他具体实现与**方法1：动态分配指针数组**相同，此处不赘述。

#### 方法3: 动态分配一维数组

这种方法将二维数组展开，看作是一个一维数组，使用一维数组的动态内存分配方法。

``````c
int rows = 3, cols = 4;
int* arr = (int*)malloc(rows * cols * sizeof(int));
``````

**注意⚠️：使用这种方法意味着你不能使用`arr[row][col]`去访问该“二维数组”**

示例代码：

``````c
#include <stdlib.h>
int main(void) {
  int rows = 3, cols = 4;
  int* arr = (int*)malloc(rows * cols * sizeof(int));
  
  for (int row = 0; row < rows; row++) {
    for (int col = 0; col < cols; col++) {
      *(arr + row * cols + col) = row * cols + col + 1;
    }
  }
  
  free(arr);
  
  return 0;
}
``````
