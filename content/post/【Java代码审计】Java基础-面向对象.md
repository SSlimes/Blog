---
title: 【Java代码审计】Java基础-面向对象
date: 2025-10-10
categories:
  - 代码审计
image: images/89.webp
---
# 0x00 类
## 类的创建
这里以银行卡作为例子：
首先创建一个BankAccount类
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250919142902.png)
```java
public class BankAccount {  
    // 账户  
    public String number;  
    // 持卡人  
    public String name;  
    // 余额  
    public double balance;  
}
```
加上一些参数。
## 对象的创建
在主函数中**new**一个对象出来。
```java
public class Main {  
    public static void main(String[] args) {  
        // new 关键字来创建  
        // 设计的类是什么就要通过什么来接收  
        // 给对象定义一个变量名 back1        BankAccount back1 = new BankAccount();  
        back1.number="123123123";  
        // 给对象的属性赋值  
        // 可以直接 . 属性来赋值  
        back1.name="jc";  
        back1.balance=100.1;  
  
        System.out.println(back1.number);  
        System.out.println(back1.name);  
        System.out.println(back1.balance);  
    }  
}
```
输出：
```java
123123123
jc
100.1
```
这时候你就会发现，不能把银行卡上的钱直接输出出来吧。这样太不安全了。这时候就出现了三要素。
# 0x01 封装，继承，多态。
## 1. 封装
### public
```java
public class BankAccount {  
    // 账户  
    public String number;  
    // 持卡人  
    public String name;  
    // 余额  
    public double balance;  
}
```
其他类也可以进行调用，或者是new。
### private
```java
public class BankAccount {  
    // 账户  
    private String number;  
    // 持卡人  
    private String name;  
    // 余额  
    private double balance;  
}
```
这里再进行调用的话就会出错。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251009140138.png)
### 封装属性Get/Set
封装属性：使用get（获取）和set（修改）
这个时候我们就可使用get/set进行调用了。
```java
public class BankAccount {  
    // 账户  
    private String number;  
    // 持卡人  
    private String name;  
    // 余额  
    private double balance;  
    //封装属性，使用get（获取）和set（修改）  
    public String getNumber() {  
        return number;  
    }  
    public void setNumber(String number) {  
        this.number = number;  
    }  
}
```
怎么调用呢？直接在主函数：
```java
public class Main {  
    public static void main(String[] args) {  
    BankAccount bankAccount = new BankAccount();  
    bankAccount.setNumber("1111");  
        System.out.println(bankAccount.getNumber());  
    }  
}
```
输出：
```
1111
```
那这样的和直接点有什么区别呢？
方法一（public方法，直接`.`即可）：
```
back1.number="123123123";   //获取
```
方法二（封装）：
```
BankAccount bankAccount = new BankAccount();   
bankAccount.setNumber("1111");  //set设置
```
### 快速生成Set/Get方法
连点两次SHIFT，直接搜索，可以看到Getter和Setter。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251009141223.png)
这里可以看到，还未生成的Get/Set方法的变量。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251009141301.png)
将name和balance都设置好Set/Get方法。
```java
public class BankAccount {  
    // 账户  
    private String number;  
    // 持卡人  
    private String name;  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public double getBalance() {  
        return balance;  
    }  
  
    public void setBalance(double balance) {  
        this.balance = balance;  
    }  
  
    // 余额  
    private double balance;  
    //封装属性，使用get（获取）和set（修改）  
    public String getNumber() {  
        return number;  
    }  
    public void setNumber(String number) {  
        this.number = number;  
    }  
}
```
### 封装的意义
这里我们看余额的时候，设置一个密码，当查看余额的时候，进行密码校验。
首先可以看一下BankAccount类
```java
public class BankAccount {  
    // 账户  
    private String number;  
    // 持卡人  
    private String name;  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public String getNumber() {  
        return number;  
    }  
  
    public void setNumber(String number) {  
        this.number = number;  
    }  
  
    //密码  
    private String pwd="123123";  
    // 余额  
    private double balance;  
    //封装属性，使用get（获取）和set（修改）  
    public double getBalance() {  
        return balance;  
    }  
    public void setBalance(double balance, String pwd) throws Exception {  
        //密码校验  
        if (this.pwd.equals(pwd)) {  //抛出一个异常，验证一下密码，如果密码错误。直接抛出密码错误
            this.balance = balance;  
        }else {  
            throw new Exception("密码错误");  
        }  
        this.number = number;  
    }  
}
```
主函数
```java
public class Main {  
    public static void main(String[] args) {  
    BankAccount bankAccount = new BankAccount();  //new一个对象
    try {  //抛出一个异常
        bankAccount.setBalance(100.1,"123");  //向银行卡里存钱，并输入密码，如果密码错误，则抛出异常
    } catch (Exception e){  
        throw new RuntimeException(e);  
    }  
    }  
}
```
输出：
```java
Exception in thread "main" java.lang.RuntimeException: java.lang.Exception: 密码错误
	at Main.main(Main.java:7)
Caused by: java.lang.Exception: 密码错误
	at BankAccount.setBalance(BankAccount.java:36)
	at Main.main(Main.java:5)
```
**最终效果**
可以看到，我们并没有封装后的，我们没有办法对它的属性进行修改。从而保证了它的属性的安全。
BankAccount函数
```java
public class BankAccount {  
    // 账户  
    private String number;  
    // 持卡人  
    private String name;  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public String getNumber() {  
        return number;  
    }  
  
    public void setNumber(String number) {  
        this.number = number;  
    }  
  
    //密码  
    private String pwd="123123";  
    // 余额  
    private double balance;  
    //封装属性，使用get（获取）和set（修改）  
    public double getBalance() {  
        return balance;  
    }  
    //余额的修改  
    public void setBalance(double balance, String pwd) throws Exception {  
        //密码校验  
        if (this.pwd.equals(pwd)) {  
            this.balance = balance;  
        }else {  
            throw new Exception("密码错误");  
        }  
    }  
}
```
主函数
```java
public class Main {  
    public static void main(String[] args) {  
    BankAccount bankAccount = new BankAccount();  
    try {  
        bankAccount.setBalance(100.1,"123123");  
    } catch (Exception e){  
        throw new RuntimeException(e);  
    }  
        System.out.println(bankAccount.getBalance());  
    }  
}
```
输出：
```
100.1
```
**这就是封装的一个主要特性，保护了类的属性安全。**
### 构造函数
从上面的代码来看，又产生了一个问题，每张银行卡的密码都不一样啊，不可能所有的银行卡都是一样的，那我们怎么处理这个问题呢，这时候就会用到了构造函数，直接在类创建有参或者无参函数解决。
```java
//构造函数  
//无参构造  
public BankAccount(){  
  
}  
public BankAccount(String number, String name,double balance, String pwd) {  
    //this关键字:表示当前对象。  
    //作用域 就近原则  
    this.number = number;  
    this.name = name;  
    this.balance = balance;  
    this.pwd = pwd;  
}
```
主函数（直接new的时候，赋值就行）
```java
public class Main {  
    public static void main(String[] args) {  
    BankAccount bankAccount1 = new BankAccount("123213","zs",100.1,"123456");//这个代码的意思就是，这是银行卡是zs的，密码是123456  
    BankAccount bankAccount2 = new BankAccount("321321","ls",200.1,"123");//这个代码的意思就是，这是银行卡是ls的，密码是123  
        try {  
        bankAccount.setBalance(100.1,"123456");  
    } catch (Exception e){  
        throw new RuntimeException(e);  
    }  
        System.out.println(bankAccount.getBalance());  
    }  
}
```
## 2. 继承
这里我们新建一个类，作为例子，新建一个动物类，Animal类。
我们先把基础的东西创建好。
**Animal类**
```java
public class Animal {  
    private String name; //动物名称  
    private int age; //动物年龄  
  
    //创建一个构造方法  
    public Animal(String name) {  
        this.name = name;  
        this.age = 1;  
    }  
     //设置两个方法，一个是吃饭，一个是睡觉。
    public void eat(){  
        System.out.println(this.name+"吃饭");  
    }  
    public void sleep(){  
        System.out.println(this.name+"睡觉");  
    }  
}
```
**主类**
```java
public class Main {  
    public static void main(String[] args) {  
    Animal animal = new Animal("小红");  //new一个animal对象
    animal.eat();  
    }  
}
```
**输出**
```
小红吃饭
```
然后动物还有分类，比如狗，猫，大象等等。
这里我们需要再创建一个类，比如狗类，猫类。
### 继承extends的使用
**Dog类**
这里Dog也会吃饭，睡觉，可以直接继承Animal。
`Dog extends Animal`
Dog：是子类
Animal：是父类
```java
public class Dog extends Animal {   
 
}
```
好了，这里我们可以完善一下Animal类，将Animal中的Get/Set方法都调用出来。
**Animal类**
这里先使用共有的==public==。
```java
public class Animal {  
    public String name; //动物名称  
    public int age; //动物年龄  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public int getAge() {  
        return age;  
    }  
  
    public void setAge(int age) {  
        this.age = age;  
    }  
  
    public void eat(){  
        System.out.println(this.name+"吃饭");  
    }  
    public void sleep(){  
        System.out.println(this.name+"睡觉");  
    }  
}
```
好了，我们可以直接在主函数中，new一个Dog对象了。
**主类**
```java
public class Main {  
    public static void main(String[] args) {  
    Dog dog = new Dog();  
    dog.setName("狗");  
    dog.eat();  
    dog.sleep();  
        System.out.println(dog.getName());  
    }  
}
```
**输出**
```
狗吃饭
狗睡觉
狗
```
我们已经学过封装了，就不可能再使用公有的了，不安全。换成私有的==private==。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251009153108.png)
### 方法重写&方法重载
**方法重载**：多个相同名称方法，不同参数。
**方法重写**：子类覆盖父类的方法。
如果狗吃饭和其他动物不一样的话，我们需要修改吃饭的方法，这时候我们就可以在Dog类中重写吃饭的方法
这时候我们重写一个eat方法，但是发现，name不能用，这里显示的是private访问权限的问题，但是如果我们就想使用呢？
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251009154816.png)
如果我们直接改成public之后就可以使用了，但是这里的话，满足不了面向对象封装的特性了。
这时候出现了，一个新的关键字：**protected**，受保护的，在同一个包里可以直接使用。
好了，那么我们直接更换**protected**，再去重新。
**Animal类**
```java
public class Animal {  
    protected String name; //动物名称  
    private int age; //动物年龄  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public int getAge() {  
        return age;  
    }  
  
    public void setAge(int age) {  
        this.age = age;  
    }  
  
    public void eat(){  
        System.out.println(this.name+"吃饭");  
    }  
    public void sleep(){  
        System.out.println(this.name+"睡觉");  
    }  
  
}
```
**Dog类**
```java
public class Dog extends Animal {  
  
    @Override  
    public void eat() {  
        System.out.println(this.name+"吃肉");  
    }  
}
```
**主类**
```java
public class Main {  
    public static void main(String[] args) {  
    Dog dog = new Dog();  
    dog.setName("狗");  
    dog.eat();  
    dog.sleep();  
        System.out.println(dog.getName());  
    }  
}
```
输出
```
狗吃肉
狗睡觉
狗
```
可以看到，这里的eat方法，成功重写。
### 关键字super
我们之前学习过构造方法，这里我将Animal类，添加一个有参构造。
```java
public Animal(String name) {  
    this.name = name;  
    this.age = 1;  
}
```
这里会发现Dog类出现了一个报错，发现这个类里没有无参构造。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251009160016.png)
那么我们将Dog类中新增一个有参构造，这里使用super关键字，发现其实super就是父的意思。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251009160218.png)
```java
public class Dog extends Animal {  
    public Dog(String name) {  
        super(name);  
    }  
  
    @Override  
    public void eat() {  
        System.out.println(this.name+"吃肉");  
    }  
}
```
这时候我们再使用的话，就要给狗加一个名字了。
**主类**
```java
public class Main {  
    public static void main(String[] args) {  
    Dog dog = new Dog("小明");  
    dog.eat();  
    dog.sleep();  
        System.out.println(dog.getName());  
    }  
}
```
**输出**
```
小明吃肉
小明睡觉
小明
```
## 3. 多态
在学习多态之前，我们需要学习一下接口。
### 接口
关键字：**interface**
特性：接口方法不能实现。
我们先举个例子：
我们先定义一个变量，使用protected进行修饰。发现，报错了。不能使用修饰符。
可以得出一个结论：**接口不能使用修饰符**
```java
package a;  
public interface Animal {  
    protected String name;  
}
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251009161012.png)
那我们创建一个方法试试：
发现又报错了，这里提示“接口 abstract 方法不能有主体”
```java
package a;  
  
public interface Animal {  
    public void eat(){  
        System.out.println("<UNK>");  
    }  
}
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251009161251.png)
这里的意思不能有代码块，所以我们只能这样创建。
```java
package a;  
  
public interface Animal {  
    public void eat();  
    public void sleep();  
}
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251009161349.png)
那么我们创建两个类，实现了一下这个接口。
这里创建两个类，一个是Dog，另一个是Cat。
**Dog类**
```java
package b;  
  
public class Dog {  
  
}
```
**Cat类**
```
package b;  
  
public class Cat {  
  
}
```
然后实现一个这个接口。
**Dog类**
```java
package b;  
  
import a.Animal;  
  
public class Dog  implements Animal {  
    @Override  
    public void eat() {  
        System.out.println("狗吃饭");  
    }  
    @Override  
    public void sleep() {  
        System.out.println("狗睡觉");  
    }  
  
}
```
**Cat类**
```java
package b;  
  
import a.Animal;  
  
public class Cat implements Animal {  
    @Override  
    public void eat() {  
        System.out.println("猫吃饭");  
    }  
    @Override  
    public void sleep() {  
        System.out.println("猫睡觉");  
    }  
}
```
然后我们在主函数中直接调用方法。
**主类**
```java
import b.Cat;  
import b.Dog;  
  
public class Main {  
    public static void main(String[] args) {  
    Dog d = new Dog();  
    Cat c = new Cat();  
    d.eat();  
    c.eat();  
    }  
}
```
**输出**
```
狗吃饭
猫吃饭
```
好了，我们可以通过之前学过的构造方法，每个动物都设置一个名字。
**Dog类**
```java
package b;  
  
import a.Animal;  
  
public class Dog  implements Animal {  
    public String name;  
    public Dog(String name) {  
        this.name = name;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    @Override  
    public void eat() {  
        System.out.println("狗吃饭");  
    }  
    @Override  
    public void sleep() {  
        System.out.println("狗睡觉");  
    }  
  
}
```
**Cat类**
```java
package b;  
  
import a.Animal;  
  
public class Cat implements Animal {  
    public String name;  
    public Cat(String name) {  
        this.name = name;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    @Override  
    public void eat() {  
        System.out.println("猫吃饭");  
    }  
    @Override  
    public void sleep() {  
        System.out.println("猫睡觉");  
    }  
  
}
```
**Animal接口**
```java
package a;  
  
public interface Animal {  
    public void eat();  
    public void sleep();  
}
```
**Main类**
```java
import b.Cat;  
import b.Dog;  
  
public class Main {  
    public static void main(String[] args) {  
    Dog d = new Dog("哈士奇");  
    Cat c = new Cat("耶罗");  
    d.eat();  
    c.eat();  
    play(d);  
    play(c);  
    }  
    //在Main函数下，创建一个方法  
    public static void play(Dog dog) {  
        System.out.println(dog.getName()+"玩耍");  
    }  
    //方法重写  
    public static void play(Cat cat) {  
        System.out.println(cat.getName()+"玩耍");  
    }  
}
```
这时候就会发现，创建了2个一样的play方法，这就出现了代码复用的情况。
那怎么处理呢？这时候就是**接口的重要性**了。
### 多态
注：学习完接口后，我们就可以考虑一下多态的问题了，简单的来说，就是一种接口，多种实现。
**最简单是就是，我们的电脑，USB接口，既可以插鼠标也可以插U盘。**
#### 多态的条件
1. 必须要有实现和继承。
2. 必须要有方法重写。
3. 必须要有父类的引用。

我们直接在Main类中进行修改即可。
**Main类**
这时候发现，Animal接口里面并没有getName方法。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251009163605.png)
这时候需要在Animal接口中添加一个这个方法即可。
```java
package a;  
  
public interface Animal {  
    public void eat();  
    public void sleep();  
  
    public String getName();  
}
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251009163725.png)
这时候主函数也不报错了。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251009163742.png)
**Main类**
```java
import b.Cat;  
import b.Dog;  
import a.Animal;  
  
public class Main {  
    public static void main(String[] args) {  
    Animal dog = new Dog("哈士奇");  
    Animal cat = new Cat("耶罗");  
    dog.eat();  
    cat.eat();  
    play(dog);  
    play(cat);  
    }  
    //在Main函数下，创建一个方法  
    public static void play(Animal animal) {  
        System.out.println(animal.getName()+"玩耍");  
    }  
}
```
输出：
```
狗吃饭
猫吃饭
哈士奇玩耍
耶罗玩耍
```
可以看出，代码复用的难题，被我们解决了。
**这样一种模式就是多态。**
父类对象，子类实现。
# 0x02 抽象（abstract）
 基本上和接口类似，但是不一样。
 在 Java 中，`abstract`（抽象）是一个关键的关键字，主要用于修饰类和方法，其核心作用是**定义抽象的概念框架，约束子类的实现，同时实现代码的复用与扩展**。
 `abstract` 的核心价值是**建立抽象模型，约束子类行为，同时实现代码的灵活扩展和复用**，是面向对象设计中 “抽象” 思想的直接体现。
根据之前的代码，我们做一下修改。学习一下有什么用。
这里创建一个Animal类。
```java
package a;  
  
public abstract class Animal {  
    //表示抽象的意思  
    public abstract void eat();  
  
    public abstract void sleep();  
    public void play(){  
        System.out.println("玩游戏");  
    }  
}
```
这时候我们的子类继承我们的父类的话，不管有没有方法体的情况下，都可以调用。  
**Dog类**
```java
package b;  
  
import a.Animal;  
  
public class Dog  extends Animal {  
    public String name;  
    public Dog(String name) {  
        this.name = name;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    @Override  
    public void eat() {  
        System.out.println("狗吃饭");  
    }  
    @Override  
    public void sleep() {  
        System.out.println("狗睡觉");  
    }  
  
}
```
**Cat类**
```java
package b;  
  
import a.Animal;  
  
public class Cat extends Animal {  
    public String name;  
    public Cat(String name) {  
        this.name = name;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    @Override  
    public void eat() {  
        System.out.println("猫吃饭");  
    }  
    @Override  
    public void sleep() {  
        System.out.println("猫睡觉");  
    }  
  
}
```
**主类**
可以看到，其实子类Dog和Cat并没有play方法，但是可以直接进行调用。
```java
import a.Animal;  
import b.Cat;  
import b.Dog;  
  
public class Main {  
    public static void main(String[] args) {  
    Animal dog = new Dog("哈士奇");  
    Animal cat = new Cat("耶罗");  
    dog.play();  
    cat.play();  
    }  
  
}
```
**输出**
```
狗玩游戏
猫玩游戏
```
这样可以看到名字并没有修改，那么我们怎么才能把名字也改了呢
这里可以看到我们直接把name变量直接写到了Animal类中，然后直接调用即可。
```java
package a;  
  
public abstract class Animal {  
    public String name;  
    //表示抽象的意思  
    public abstract void eat();  
  
    public abstract void sleep();  
    public void play(){  
        System.out.println(this.name+"玩游戏");  
    }  
}
```
**主类**
```java
import a.Animal;  
import b.Cat;  
import b.Dog;  
  
public class Main {  
    public static void main(String[] args) {  
    Animal dog = new Dog("哈士奇");  
    Animal cat = new Cat("耶罗");  
    dog.play();  
    cat.play();  
    }  
  
}
```
**输出**
```
哈士奇玩游戏
耶罗玩游戏
```
# 0x03 包
在 Java 中，**包（Package）** 是用于管理类和接口的组织机制，类似于文件系统中的文件夹，主要作用是解决类名冲突、控制访问权限、便于代码组织和管理。
这个没什么好说的，基本上没啥理解的。
包的核心价值是**通过分类管理解决命名冲突、控制访问范围、组织代码结构**，是 Java 中大型项目开发不可或缺的机制。合理设计包结构能显著提升代码的可读性、可维护性和复用性。
### 包的基本使用
#### （1）声明包
每个 Java 源文件的**第一行**（注释除外）必须用 `package` 声明所属的包，格式：
```java
package com.example.demo;  // 声明当前类属于 com.example.demo 包
public class MyClass { ... }
```
包名通常采用**小写字母**，遵循 “逆域名” 规则（如公司域名 `example.com`，包名常用 `com.example.xxx`），避免与其他组织的包名冲突。
```java
// 导入单个类
import java.util.ArrayList;

// 导入整个包（不推荐，可能增加编译时间）
import java.util.*;

public class Test {
    public static void main(String[] args) {
        ArrayList list = new ArrayList();  // 已导入，可直接用类名
        java.sql.Date date = new java.sql.Date(0);  // 未导入，需写全类名
    }
}
```
#### （3）包与目录结构的关系
Java 要求**包名必须与文件系统的目录结构一致**。例如：
- 类声明为 `package com.example.demo`，则该类的 `.java` 文件必须放在 `com/example/demo` 目录下（层级对应包的层级）。
- 编译后生成的 `.class` 文件也会按包结构存放，确保 JVM 能正确找到类。
### 3. 常见系统包
Java 标准库提供了许多常用包，例如：
- `java.lang`：包含基础类（如 `String`、`Object`、`System`），无需手动导入。
- `java.util`：工具类（如集合 `ArrayList`、`HashMap`，日期 `Date`）。
- `java.io`：输入输出相关类（如文件操作 `File`、`InputStream`）。
- `java.net`：网络编程相关类（如 `Socket`、`URL`）。