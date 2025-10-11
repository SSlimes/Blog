---
title: 【Java代码审计】Java基础-流程控制
date: 2025-10-11
categories:
  - 代码审计
image: images/92.webp
---
# 0x01 Scannner对象
可以通过Scanner类获取用户的输入
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        //创建一个扫描器对象，用于接收键盘数据
        Scanner scanner = new Scanner(System.in);
        System.out.print("输入：");
        //接收输入
        String str = scanner.nextLine();
        System.out.println(str);
        scanner.close();
    }

}
```
# 0x01 顺序结构
- 顺序结构是最简单的算法结构
- JAVA的基本结构就是顺序结构，除非特别指明，否则就是按照顺序执行
# 0x02 if单选择结构
**语法**
```java
if(布尔表达式){
 //如果布尔表达式为true
}
else{
 //如果布尔表达式为false
}
```
**示例代码**
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        //创建一个扫描器对象，用于接收键盘数据
        Scanner scanner_str = new Scanner(System.in);
        System.out.print("输入：");
        //接收输入
        String str =  scanner_str.nextLine();
        //判断是否为相同
        if (str.equals("GetShell")){
            System.out.println("GetShell yes");
        }
        else {
            System.out.println("END!");
        }
        scanner_str.close();
    }
}
```
# 0x03 if多选择结构
**语法**
```java
if(布尔表达式){
 //如果布尔表达式为true
}
else if(布尔表达式){
 //如果布尔表达式为true
}else{
//如果布尔表达式为false
}
```
**示例代码**
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        //创建一个扫描器对象，用于接收键盘数据
        Scanner score_input = new Scanner(System.in);
        System.out.print("输入成绩：");
        //接收输入
        int score = score_input.nextInt();
        //判断是否为相同
        if (score == 100) {
            System.out.println("满分！");
        } else if (score >=90){
            System.out.println("优秀！");
        }else if (score >=60){
            System.out.println("及格！");
        }
        else {
            System.out.println("不及格！");
        }
            score_input.close();
    }

}
```
# 0x04 switch多选择结构
**示例代码**
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        char grade = 'C';

        switch (grade) {
            case 'A':
                System.out.println("优秀");
                break;
            case 'B':
            case 'C':
                System.out.println("良好");
                break;
            case 'D':
                System.out.println("及格");
                break;
            case 'F':
                System.out.println("你需要再努力努力");
                break;
            default:
                System.out.println("未知等级");
        }
        System.out.println("你的等级是 " + grade);
    }

}
```
# 0x05 while循环
**代码示例**
```java
public class Main {
    public static void main(String[] args) {
        int i = 0;
        int sum = 0;
        while (i <= 100) {
            sum += i++;
        }
        System.out.println(sum);
    }

}
```
# 0x06 do while循环
**代码示例**
```java
public class Main {
    public static void main(String[] args) {
        int i = 0;
        int sum = 0;
        do {
            sum += i++;
        } while (i <= 100);
        System.out.println(sum);
    }
}
```
# 0x07 for循环
**语法格式**
```java
for(初始化;布尔表达式;更新){
//代码语句
}
```
**代码示例**
```java
public class Main {
    public static void main(String[] args) {
        int sum = 0;
        for (int i = 0; i <= 100; i++) {
            sum += i;
        }
        System.out.println(sum);
    }
}
```
# 0x08 for循环数组
**示例代码**
```java
public class Main {
    public static void main(String[] args) {
        int[] number = {1, 2, 3, 4, 5};
        for (int num : number) {
            System.out.println(num);
        }
    }
}
```