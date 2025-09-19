---
title: 【Java代码审计】Java基础-变量
date: 2025-09-19
categories:
  - 代码审计
image: images/79.webp
---
# 0x00 简单介绍
一个简单的java程序
```java
public class Main {  
    public static void main(String[] args) {  
        System.out.printf("Hello and welcome!");  
        for (int i = 1; i <= 5; i++) {  
            System.out.println("i = " + i);  
        }  
    }  
}
```
代码审计的核心是什么？
```java
1. 关键函数 sink点
   	Runtime.getRuntime.exec(Cmd) ->Cmd 是否可控（输入点）
	过滤，web的流程 
2. 通读代码（逻辑漏洞：越权，登录绕过，支付漏洞等等）
```
熟悉项目:
```java
Spring 
Tomcat
Stust2
```
代码审计的工具：
```java
Codeql （学习成本很大）
https://codeql.github.com/codeql-standard-libraries/java/
```
敏感函数快速审计工具：
```java
semgrep  
https://semgrep.dev/docs/
```
# 0x01 变量
## 数字型
```java
整数
    byte b;// -128～127  -2（7）～2（7）-1 8
    short s;// 短整型 -32768～32767 -2（15）～2（15）-1 16
    int i; // 整型 -2（31）～2（31）-1 32
    long l; // 长整型 -2（63）～2（63）-1
小数（浮点数）
    float f;// 点精度 6-7 +-3.4*10(38)
    double d;// 双精度15-16 +-1.8*10(308)
字符型
    char c; // a b c d
     ascii 0-65535
布尔型
    boolean bool; // 真或假 0 或 1 流程控制阶段
```
## 变量的声明于初始化
```java
格式： 数据类型 变量名（声明格式）
String b = "S"; 
```
## 初始化
```java
赋值
方法1：int i = 10; // 声明时变量i赋值10
方法2：
	int j;
    j=20;   // 先声明后赋值
在同一作用域中，变量名不能重复
```
## 基本数据类型的转换
```java
自动转换（隐式转换）
		小范围-》大范围
		数据类型是有范围的
		byte->short->int->long->float->d
		char->int->long->float->d
强制转换（显式转换）
		int i=10;
      		byte b = (byte) i;
      		ps:溢出,布尔类型除外
```
## 输入输出
```java
System.out.print("Hello World!"); // 没有换行符的
System.out.println("Hello World!"); // 有换行符的
	// %s 字符串
    // %d 整数
    // %f 浮点
    // %c 字符
    // %b 布尔
    // %% 输出百分号
System.out.printf("%8d hello\n",1); //   默认右对齐
System.out.printf("%-8d hello\n",1); // 左对齐
```
## 作业
编写一个输出姓名，年龄的程序
	姓名：xxx 年龄：xxx
	输入两次，输出要对其，年龄可能所10 也 2，姓名可能三个字符也可能是4个字
代码：
```java
import java.util.Scanner;  
  
public class Main {  
    public static void main(String[] args) {  
        Scanner scanner = new Scanner(System.in);  
  
        // 输入第一个人的信息  
        System.out.print("请输入第一个人的姓名：");  
        String name1 = scanner.nextLine();  
        System.out.print("请输入第一个人的年龄：");  
        int age1 = scanner.nextInt();  
        scanner.nextLine(); // 消耗换行符  
  
        // 输入第二个人的信息  
        System.out.print("请输入第二个人的姓名：");  
        String name2 = scanner.nextLine();  
        System.out.print("请输入第二个人的年龄：");  
        int age2 = scanner.nextInt();  
  
        // 输出结果，使用格式化对齐  
        System.out.println("\n输出结果：");  
        // %-6s 表示左对齐，占6个字符位置  
        // %d 表示整数  
        System.out.printf("姓名：%-6s 年龄：%d%n", name1, age1);  
        System.out.printf("姓名：%-6s 年龄：%d%n", name2, age2);  
  
        scanner.close();  
    }  
}
```
输出：
```java
请输入第一个人的姓名：li
请输入第一个人的年龄：18
请输入第二个人的姓名：wang
请输入第二个人的年龄：12

输出结果：
姓名：li           年龄：18
姓名：wang   年龄：12
```

