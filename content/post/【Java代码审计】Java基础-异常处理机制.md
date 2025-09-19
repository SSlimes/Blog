---
title: 【Java代码审计】Java基础-异常处理机制
date: 2025-09-19
categories:
  - 代码审计
image: images/80.webp
---
# 0x00 异常处理机制
这里我打个比方，0不能当被除数，如果当了被除数就会报错，然后这个报错了之后，后续的程序就不会运行。
代码：
```java
public class Main {  
    public static void main(String[] args) {  
        int sa = divide(1,0);  
        System.out.println(sa);  
    }  
    //divide方法  
    public static int divide(int a,int b) {  
        return a/b;  
    }  
}
```
结果：
```java
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at Main.divide(Main.java:10)
	at Main.main(Main.java:5)
```
这里的话就会影响整个程序的运行，只要这里出错，后续代码都不会执行，那么怎么才能让他抛出整个异常，继续执行后续代码呢？
这里就用到了**try/catch**语句：
```java
public class Main {  
    public static void main(String[] args) {  
        //异常抛出语句，如果try里面的代码块出错了，会执行catch里面的代码块。  
        //这里必须要把sa给弄出来，这里是全局变量里的问题。  
        int sa = 0;  
        try {  
         sa = divide(1,0);  
        }catch (Exception e){  
            System.out.println("程序出错");  
        }  
        //虽然上面的代码出错了，但是这句语句还是执行了。  
        System.out.println(sa);  
    }  
    //divide方法  
    public static int divide(int a,int b) {  
        return a/b;  
    }  
}
```
输出：
```java
程序出错
0
```
# 0x01 多个异常捕获
**第一种：java里报错有很多种，这里说一下空指针异常：**
示例：
```java
public class Main {  
    public static void main(String[] args) {  
        //这里是一个a的字符串，内容为null，输出a的长度。  
        String a = null;  
        System.out.println(a.length());  
    }  
}
```
输出：（这里表示空指针异常的报错信息）
```java
Exception in thread "main" java.lang.NullPointerException: Cannot invoke "String.length()" because "a" is null
	at Main.main(Main.java:5)
```
**第二种：然后比较一下0不能做被除数的报错报出来的异常：**
```java
public class Main {  
    public static void main(String[] args) {  
        int sa = divide(1,0);  
        System.out.println(sa);  
    }  
    //divide方法  
    public static int divide(int a,int b) {  
        return a/b;  
    }  
}
```
结果：
```java
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at Main.divide(Main.java:10)
	at Main.main(Main.java:5)
```
那怎么做到，这两个异常，分别进行抛出呢？
示例
```java
public class Main {  
    // 捕获异常  
    public static void main(String[] args) {  
        // 如果try里面的代码块出现了异常，会执行 catch 里面的代码块  
        int i=0;  
        try {  
            String str = null;  
            System.out.println(str.length());  
            // 遇到异常后不再执行，跳转到catch  
            divide(1,0);  
        }catch (ArithmeticException e){  
            // catch 代码块报错执行  
            // 0 不能作为除数  
            System.out.println("0 不能作为除数");  
        }catch (NullPointerException e){  
            System.out.println("空指针异常");  
        }  
  
        // 捕获异常后代码正常执行  
        System.out.println("i = " + i);  
    }  
  
    public static int divide(int a,int b) {  
        return a/b;  
    }  
}
```
输出：
```java
空指针异常
i = 0
```
# 0x02 自定义抛出异常
这里可以直接在根源上报错。
示例：
```
public class Main {  
    // 捕获异常  
    public static void main(String[] args) {  
       int a = divide(1,2);  
        System.out.println(a);  
    }  
  
    public static int divide(int a,int b) throws Exception {  
        if (a == 1){  
            //自定义条件，a不能等于1，如果等于1就抛出异常，后面的代码不再执行。  
            //throw 抛出  
            throw new Exception("a不能等于1");  
        }  
        return a/b;  
    }  
}
```
从这里可以直接看出，直接在方法上出现了错误。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250919134058.png)
这里出错，使用try/catch进行捕获。
示例：
```java
public class Main {  
    // 捕获异常  
    public static void main(String[] args) {  
        int a = 0;  
        try {  
            a = divide(10,0);  
        }catch (Exception e){  
            System.out.println(e.getMessage());  
        }  
        System.out.println(a);  
    }  
  
    public static int divide(int a,int b) throws Exception {  
        if (a == 1){  
            //自定义条件，a不能等于1，如果等于1就抛出异常，后面的代码不再执行。  
            //throw 抛出  
            throw new Exception("a不能等于1");  
        }  
        return a/b;  
    }  
}
```
输出：
```java
/ by zero
0
```
# 0x03 总结
## 异常的关键字
```
try      捕获异常
catch 输出异常
throw 抛出异常
throw 在方法上抛出异常
```
## 自定义抛出异常
比如自定义一个类NameException：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250919140010.png)
**NameException类：**
```java
//需要继承Exception  
//自定义 黑名单姓名异常  
public class NameException extends Exception {  
    public NameException() {  
        super("名称在黑名单里不能使用");  
    }  
}
```
**主类（Main）:**
```java
public class Main {  
    // 捕获异常  
    public static void main(String[] args) {  
        try {  
            name("locks");  
        } catch (NameException e) {  
            System.out.println(e.getMessage());
        }  
    }  
  
    public static String name(String name) throws NameException {  
        //name 不能叫locks  
        if(name.equals("locks")){  
            throw new NameException();  
        }  
        return name;  
    }  
}
```
输出：
```java
名称在黑名单里不能使用
```