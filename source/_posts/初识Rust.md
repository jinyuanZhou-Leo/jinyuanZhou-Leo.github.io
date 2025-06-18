---
title: 呆在家里要生锈啦 - Day 1
date: 2025-06-18 18:42:13
tags: [Rust, 杂记]
categories: [Rust]
---

## 哇塞！

从`rustup`到`cargo`，`rust`工具链优秀的设计和人文关怀，给我留下了深刻的影响。那就……从今天开始生锈(rustify)！

## Rust 圣经

被名字耽误的神级教材：[Rust 圣经](https://course.rs/about-book.html)

一定要：点:star:,点:star:,点:star2:!!!



## 第一章：优雅的变量

### 变量绑定

在Rust中变量赋值被称为**变量绑定**，这涉及到Rust当中的所有权概念。任何内存对象都是有主人的(耶，又是sm环节), 一般情况下内存对象完全属于他的主人。**绑定**操作就是把这个对象绑定给一个变量，让这个变量成为他的主人。

和其他语言不同的是，在Rust中，变量在默认情况下是不可变的常量

``````rust
let const_a = 1;
let mut mutable_a = 1;
``````

### 这个没有用到哦！

Rust编译器会在有未使用的变量时抛出Warning，在变量名称前加`_`即可解决。

### 变量遮蔽 Variable Shadowing

Rust允许存在相同的变量名，后面声明的会遮蔽掉前面声明的

``````rust
fn main() {
    let x = 5;
    // 在main函数的作用域内对之前的x进行遮蔽
    let x = x + 1; //6

    {
        // 在当前的花括号作用域内，对之前的x进行遮蔽
        let x = x * 2; //在大括号内为12
        println!("The value of x in the inner scope is: {}", x);
    }

    println!("The value of x is: {}", x); //6
}
``````

## 第二章：基本类型

Rust的类型:dizzy_face:有种别致的美

- 数值类型：`i8,i16,i32,i64,isize` or `u8,u16,u32,u64,usize` (u for unsigned), `f32,f64`(f for float)
- 字符串`&str`
- Boolean
- 字符类型: 单个unicode字符，4-byte

### 比较浮点数？算了吧

``````Rust
assert(0.1+0.2==0.3) // 程序会panic
``````

因为二进制精度问题，导致了 0.1 + 0.2 并不严格等于 0.3，它们可能在小数点 N 位后存在误差。

### NaN

Rust使用`NaN`来表示在数学上未定义的结果，可以使用`is_nan()`来检查变量是否为`NaN`

### Rust是强类型语言！！！

不同于C/C++中的自动类型提升，在Rust中`i32`和`i64`被看作不通类型，**不能混合运算**！

### 序列

- `1..5`表示1-4的序列

- `1..=5`表示1-5的序列
- `'a'..='z'`表示a到z的序列

### 类型转换

Rust使用 `As` 关键字进行类型转换

### 单元类型

单元类型就是 `()` ，对，你没看错，就是 `()` ，唯一的值也是 `()`

### 语句与表达式

Rust 的函数体是由一系列语句组成，最后由一个表达式来返回值，例如：

```rust
fn add_with_extra(x: i32, y: i32) -> i32 {
    let x = x + 1; // 语句
    let y = y + 5; // 语句
    x + y // 表达式
}
```

### 无返回值和发散函数

无返回值函数返回一个`()`单元类型，而发散函数(diverging functions)用不返回

``````rust
fn dead_end() -> ! {  
  panic!("你已经到了穷途末路，崩溃吧！"); 
}
``````

## 第三章：所有权， 引用和借用 Ownership, Reference and borrowing

Rust是一门没有GC的内存安全语言，于是引入了一些奇特的概念

### 所有权原则

1. Rust 中每一个值都被一个变量所拥有，该变量被称为值的所有者
2. 一个值同时只能被一个变量所拥有，或者说一个值只能拥有一个所有者
3. 当所有者（变量）离开作用域范围时，这个值将被丢弃(drop)



**Rust 永远也不会自动创建数据的 “深拷贝”**



具体请看 [2.3.1所有权](https://course.rs/basic/ownership/ownership.html)

