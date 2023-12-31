---
title: 韩顺平java学习
date: 2022-02-25 12:38:51
tags: 学习
---

# 基础知识

## 进制

进制|首位表示方式
--|--
二进制|0B
十进制|无
八进制|0
十六进制|0X

### 进制转换

#### x进制转十进制

正常，没什么问题

#### 十进制转x进制

将该数不断除以x，直到商为0为止，然后将每一步得到的余数倒过来，就是对应的x进制

#### 二进制转八进制/十六进制

从低位开始，将二进制三位/四位一组，转换成对应的八进制数即可。

#### 八进制/十六进制转二进制

将八进制的每一位转成对应的一个3位二进制数即可，十六进制同理

## 原码、反码、补码

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20220124110005226.png" alt="image-20220124110005226" style="zoom:40%;" />

## 位运算符

- 按位与 &：两位全为1，结果为1，否则为0；
- 按位或 |：两位只要有一个为1，即可为1，否则为0；
- 按位异或 ^：两位一个为0，一个为1，结果为1，否则为0；
- 按位取反 ~：0->1，1->0；
- &gt;&gt;算数右移低位溢出，符号位不变，用符号位补溢出的高位；（溢出：扔掉）本质是除2的n次方
- &lt;&lt;算数左移符号位不变，低位补0；本质是乘2的n次方
- &gt;&gt;&gt;逻辑右移（无符号右移），低位溢出，高位补0；

## 面向对象

## 第8章 面向对象（初级）

### 对象内存布局

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20220124195406956.png" alt="image-20220124195406956" style="zoom:30%;" />

### 属性

属性=成员变量

##### 对象的引用和对象名：

对象引用在栈里，对象在堆里

### 类和对象的内存分配机制

```java
Person p2 = new Person();
Person p1 = p2//把p1赋给p2，或者说让p2指向p1
```

先在堆中创建一个Person对象，再在方法区创建对象的常量和加载信息，并将其地址返回给堆，基本数据类型则直接存放于堆中，而后将堆中的对象地址返回给栈中的p2,p2指向该地址。p1在栈中指向p2复制得到的地址。

Java内存结构分析

1. 栈：一般存放基本数据类型（局部变量）；
2. 堆：存放对象（Person person，数组等）；
3. 方法区：常量池（常量，比如字符串），类加载信息

Java创建对象的流程简单分析

1. 先加载Person类信息（属性和方法信息，只加载一次）；
2. 在堆中分配空间，进行默认初始化；
3. 将地址赋给p，p指向对象；
4. 进行指定初始化，比如：`p.name = "jack";`

### 成员方法（方法）

方法调用机制

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20220126195934038.png" alt="image-20220126195934038" style="zoom: 33%;" />

### 重载

### 递归

1. 执行一个方法时，就创建一个新的受保护的独立空间（栈空间）
2. 方法的局部变量时独立的，不会相互影响，比如n变量
3. 如果方法中使用的是引用类型变量（比如数组、对象），就会共享该引用类型的数据
4. 递归必须向退出递归的条件逼近，否则就是无限递归，出现StackOverflowError，就会死龟
5. 当一个方法执行完毕，或者遇到return，就会返回，遵守谁调用，就结果返回给谁，同时当方法执行完毕或者返回时，该方法就执行完毕

### 可变参数

#### 注意事项和使用细节

1. 可变参数的实参可以为0个或者任意多个
2. 可变参数的实参可以为数组
3. 可变参数的本质就是数组
4. 可变参数可以和普通参数类型的参数一起放在形参列表，但必须保证可变参数在最后
5. 一个形参列表中只能出现一个可变参数

```java
public int sum(String str,int... a)
```

### 作用域

全局变量和局部变量

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20220128105339594.png" alt="image-20220128105339594" style="zoom:40%;" />

全局变量可以用修饰符，局部变量不可以

### 构造器（构造方法）

一旦定义了自己的构造器，那么无参构造器（默认构造器）就被覆盖，就不能再使用无参构造器，除非自己重新定义一个无参构造器

构造器并不创建对象，而是对对象进行初始化

### this关键字

this指当前对象自己

简单地说，哪个对象调用，this就代表了哪个对象

#### this的注意事项和使用细节

1. this关键字可以用来访问本类的属性、方法、构造器
2. this用于区分当前类的属性和局部变量
3. 访问成员方法的语法：`this.方法名(参数列表)`
4. 访问构造器的语法：`this(参数列表)`。**注意只能在构造器中使用**（即只能在构造器中访问另外一个构造器，必须放在第一条语句）
5. this不能在类定义的外部使用，只能在类定义的方法中使用

```java
//构造器语法:this(参数列表)；必须放置于第一条语句
public T(){
    this("jack",100);//只能在构造器中使用这种语法，其他成员方法不得使用
    System.out.println()
}
public T(String name,int age){
    ......
}
```

`this.name`访问的一定是类的属性，而`name`则有可能访问的是成员方法的局部变量

### 对象数组

1. 静态初始化

   在定义数组的同时对数组元素进行初始化

   ```java
   BankAccount[] accounts = { new BankAccount(“Zhang", 100.00),
   
   new BankAccount(“Li", 2380.00),}
   ```

2. 动态初始化

   - 使用运算符new为数组分配空间 

     ```java
     type[ ] arrayName=new type[arraySize];
     ```

   - 只是对象数组本身分配空间，并**没有对数组元素进行初始化**，即**数组元素均为空**，因此下列程序会报错

     ```java
     Person[] people = new Person[100];
             people[0].name="yyh";
     ```

## 第9章 面向对象（中级）

### IDEA编译器

#### IDEA快捷键

1. 删除当前行`ctrl+d`
2. 复制当前行`ctrl+alt+向下光标`
3. 补全代码`alt+/`
4. 添加注释或者取消注释`ctrl+/`
5. 导入该行需要的类`alt+enter`
6. 快速格式化代码`ctrl+alt+L`
7. 快速运行程序`alt+R`
8. 生成构造方法`alt+insert`
9. 查看一个类的层级关系`ctrl+H`
10. 定位类的方法，查看某方法的源码`ctrl+B`
11. 自动分配变量名`.var`

#### IDEA模板

`settings->Editor->Live Templates`里面都有

#### IDEA小技巧

点左下角的Structure可以看到一个对象的方法

在IDEA中动态传参数：Edit Configurations -> Program arguments

### 包

作用

1. 区分相同名字的类
2. 当类很多时，可以很好地管理类
3. 控制访问范围

基本语法

```java
package com.xxx.xxx......
    //package打包，后接包名
```

#### 包的本质

创建不同的文件夹来保存类文件

#### 包的命名

只能包含数字、字母、下划线、小圆点。但是不能用数字开头，不能是关键字或保留字

一般是小写字母+小圆点，一般是`com.公司名.项目名.业务名`，例如`com.sina.crm.user`

#### 常用的包

`java.lang.*`默认引入

`java.util.*`系统提供的工具包

`java.net.*`网络包，网络开发

`java.awt.*`java界面开发，GUI

#### 注意事项

package声明当前类所在的包，需要放在类的最上面，一个类最多只有一句package

import指令要求在package下面，可以有多句且没有顺序要求

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20220129223654194.png" alt="image-20220129223654194" style="zoom:40%;" />

### 访问修饰符

| 访问级别 | 访问修饰符 | 同类 | 同包 | 子类 | 不同包 |
| -------- | ---------- | ---- | ---- | ---- | ------ |
| 公开     | public     | ✓    | ✓    | ✓    | ✓      |
| 受保护   | protected  | ✓    | ✓    | ✓    |        |
| 默认     | 没有修饰符 | ✓    | ✓    |      |        |
| 私有     | private    | ✓    |      |      |        |

### 封装（encapsulation）

将抽象出的数据[属性]和对数据的操作[方法]封装在一起，数据被保护在内部，程序的其他部分只有通过被授权的操作[方法]，才能对数据进行操作

#### 封装的好处

1. 隐藏实现的细节
2. 对数据进行验证，保证安全合理

#### 封装的实现步骤（三部曲）

1. 将属性进行私有化private【外部不能直接修改属性】
2. 提供一个公共的(public)set方法，用于对属性判断并赋值

```java
public void setXxx(类型 参数名){
    //加入数据验证的业务逻辑
    属性 = 参数名；
}
```
 可以使用快捷键处理

3. 提供一个公共的(public)get方法，用于获取属性的值

```java
public XX getXxx(){//权限判断，Xxx某个属性
    return xx;
}
```

#### 封装与构造器

将set方法写在构造器中，仍然可以起到防护的机制

```java
public Person(String name, int age){
    //this.age = age;
    //this.name = name; 这样不好
    setName(name);//这样更好
    setAge(age);
} 
```

### 继承（extends）

用于解决代码复用问题。多个类存在相同的属性（变量）和方法时，可以从这些类中抽象出父类（超类），在父类中定义这些相同的属性和方法，所有的子类不需要再重新定义这些属性和方法，只需要通过extends来声明继承父类即可。

**继承设计的基本思想**：父类构造器完成父类属性初始化，子类构造器完成子类属性初始化

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20220130165109518.png" alt="image-20220130165109518" style="zoom:45%;" />

#### 继承的基本语法

```java
class 子类 extends 父类{
    
}
```

1. 子类自动拥有父类定义的属性和方法
2. 父类又叫超类、基类
3. 子类又叫派生类

#### 继承的细节

1. 子类继承了所有的属性和方法，**非私有的属性和方法可以直接访问在子类直接访问**，**但是私有属性和方法不能在子类直接访问**，要通过公共的方法来访问

2. 子类必须调用父类的构造器，完成父类的初始化。子类里面默认调用了父类的无参构造器`super()`。

3. 当创造子类对象时，不管使用子类的哪个构造器，**默认情况下总会去调用父类的无参构造器**，如果父类没有提供无参构造器，则必须在子类的构造器中用super去**指定使用父类的哪个构造器完成对父类的初始化工作**，否则编译不会通过。

4. 如果希望指定去调用父类的某个构造器，则显示的调用一下。

   如果不是默认的无参构造器，那么需要显示调用父类的该构造器`super(对应实参)`

5. `super`在使用时，必须放在构造器第一行

6. `super()`和`this()`都只能放在构造器第一行，因此这两个方法不能共存于同一个构造器，二者不能同时存在。

7. java所有类都是Object类的子类，Object是所有类的基类（`ctrl+H`可以看到类的继承层次）

8. 父类构造器的调用不限于直接父类，将一直追溯直到Object类（顶级父类）。调用了就会从祖宗到子类一路下来，全部调。

9. 子类最多只能继承一个父类（直接继承），即java是单继承机制。若想让A继承B和C，则可以让B继承C后，再让A继承B。

10. 不能滥用继承，子类和父类必须满足**is-a**的逻辑关系

#### 继承的本质分析（重要）

当子类对象创建好后，建立查找的关系

示例代码：

```java
public class ExtendsTheory {
    public static void main(String[] args) {
        Son son = new Son();
    }
}

class GrandPa {//爷类
    String name = "大头爷爷";
    String hobby = "旅游";
}

class Father extends GrandPa {//父类
    String name = "大头爸爸";
    int age = 39;
}

class Son extends Father {//子类
    String name = "大头儿子";
}
```

内存布局

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20220130214915549.png" alt="image-20220130214915549" style="zoom:40%;" />

1. 首先看子类是否有该属性
2. 如果子类有这个属性，且可以访问，则返回信息
3. 如果子类没有这个属性，就看父类有没有这个属性（如果父类有该属性。且可以访问，就返回信息。。。。）
4. 如果父类没有，就按照（3）的规则，继续找上级父类，直到Object。。。

private的属性也存在于堆中，但是不能直接访问，使用方法间接访问即可。如果上级有public的，直接访问的话其会访问第一个public。

#### 例题

```java
public class ExtendsTheory {
    public static void main(String[] args) {
        B b = new B();
    }
}

class A{
    A(){
        System.out.println("a的无参构造器");
    }

    A(String name){
        System.out.println("a的有参构造器");
    }
}

class B extends A{
    B(){
        //由于下面有this，此处原本隐藏的super()被撤销
        this("abs");
        System.out.println("b的无参构造器");
    }

    B(String name){
        //此处隐藏了一个super()
        System.out.println("b的有参构造器");
    }
}
```

输出的是

```bash
a的无参构造器
b的有参构造器
b的无参构造器
```

首先从进入B无参构造器的this中，**注意此处由于B的无参构造器有`this`，因此此处的默认的`super()`也消失了,所以他直接进入了`this`而没有处理`super()`**。然后进入B的有参构造器，此处有一个默认的`super()`，所以进入A的无参构造器，而后输出B的有参构造器中的内容。最后回到B的无参构造器，输出其内容。记住，**所有构造器在没有this存在的时候都有一个默认的super()**

### super关键字

super代表父类的引用，用于访问父类的属性、方法、构造器

#### 基本语法

1. 访问父类的属性，但不能访问父类的private属性

   `super.属性名`

2. 访问父类的方法，但不能访问父类的private方法

   `super.方法名(参数列表)`

3. 访问父类的构造器（这点前面用过）：

   `super(参数列表);`只能用在构造器的第一句，只能出现一句。

#### super给编程带来的便利/细节

1. 调用父类构造器的好处（分工明确）
2. 当子类有和父类中的成员（属性和方法）重名时，为了访问父类的成员，必须通过super。若没有重名，使用super、this、直接访问都是一样的结果。直接访问的顺序：先找本类，如果有，则调用，如果没有，则找父类，直到Object。super顺序：直接查找父类，跳过本类，其他逻辑一致。

3. super的访问不限于直接父类，如果爷爷类和本类都有同名的成员，也可以使用super去访问爷爷类的成员；如多个基类都有同名成员，使用super方法遵循就近原则。

#### super和this的比较

| No.  | 区别点     | this                                   | super                                  |
| ---- | ---------- | -------------------------------------- | -------------------------------------- |
| 1    | 访问成员   | 访问本类的成员，如果本类没有就去找父类 | 直接访问父类中的成员，跳过本类         |
| 2    | 调用构造器 | 调用本类构造器，必须放在构造器首行     | 调用父类构造器，必须放在子类构造器首行 |
| 3    | 特殊       | 表示当前对象                           | 子类中访问父类对象                     |

### 方法重写/覆盖（override）

子类有一个方法，和父类的某个方法的名称、返回类型、参数一样，那么我们说这个子类的方法覆盖了父类的方法

#### 注意事项和使用细节

1. 子类的方法的参数、方法名称，要和父类方法的参数、方法名称完全一致
2. 子类的返回类型和父类方法返回类型一样，或者是父类返回类型的子类。例如父类返回Object，子类返回String
3. 子类方法不能缩小父类方法的权限

### 多态（polymorphic）

问题：代码复用性不高，不利于代码维护

多态：方法或对象具有多种形态，是面向对象的第三大特征，多态是建立在封装和继承基础之上的。

Java有两种引用类型，分别是编译时类型和运行时类型。编译型类型在变量声明时决定，运行时类型取决于变量具体指向的类型，如果两种类型不一致，就会出现多态。

规则：对象调用编译时类型的属性和运行时类型的方法。

#### 具体体现

1. 方法的多态：重载和重写体现多态

2. 对象的多态（**核心、困难、重点**）

   - 一个对象的编译类型和运行类型可以不一致

     ```java
     Animal animal = new Dog();//【animal编译类型是Animal，运行类型是Dog】
     animal = new Cat();//【animal的运行类型变成了Cat，编译类型仍然是Animal】
     ```

   - 编译类型在定义对象时就确定了，**不能改变**。（编译器可以认为是编译器看到的类型）（直接把编译类型看成指针类型就好了）

     ```java
     //编译类型Animal确定了，不能改变
     ```

   - 运行类型是**可以变化**的。（运行类型则是运行时真正起作用的类型）。可以通过`getClass()`来查看运行类型。

     ```java
     //运行类型Dog可以变成Cat
     ```

   - **编译类型**看定义时 `=` 号的左边，**运行类型**看 `=` 号的右边。

   ``` java
   //使用多态可以统一管理主人喂食的问题
   //animal编译类型是Animal，可以指向（接收）Animal子类的对象
   //food编译类型是Food，可以指向（接收）Food子类的对象
   public void feed(Animal animal,Food food){
       System.out.println("主人给" + name + "给" + animal.getName() + "吃" + food.getName);
   }
   ```

#### 多态注意事项和细节讨论

1. 前提：两个对象（类）存在继承关系

2. 多态的向上转型
   - 本质：父类的引用指向了子类的对象（继承图里面父类在上面，子类在下面，所以叫向上转型）
   
   - 语法：`父类类型 引用名 = new 子类类型();`
   
     ```java
     Animal animal = new Cat();
     ```
   
   - 特点：
     - **编译类型**看左边，**运行类型**看右边。
     
     - 可以调用父类中的所有成员（遵循访问权限（也就是public，private这种））
     
     - 不能调用子类中的特有成员（因为在编译阶段，能调用哪些成员是由编译类型决定）
     
     - 最终运行效果看子类的具体表现
     
       ```java
       animal.eat()//先去cat中找eat，再去animal找。。。与方法的调用规则一致
       ```
   
3. 多态的向下转型
   - 语法：`子类类型 应用名 = （子类类型）父类引用;`
   
     ```java
     Cat cat = (Cat) animal;//cat的编译类型是Cat，运行类型是Cat
     ```
   
   - 只能强转父类的**引用**，不能强转父类的**对象**（小明这个人就是这个人，他可以改名，但是他不能不是他自己）
   
   - 要求父类的引用必须指向的是当前目标类型的对象
   
   - 当向下转型后，可以调用子类类型中所有的成员

   <img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230913210624732.png" alt="向上转型和向下转型的示意图" style="zoom:80%;" />
   
   需要注意的是：图中指向方向是从 **对象** 指向其对应的 **引用**
   
   ```java
   Object o = string; //此处为一个向上转型。解释为对一个string对象赋予Object，父类的引用指向了子类的对象。
   String s = (String) o;//此处为一个向下转型
   ```
   
4. 属性没有重写之说，属性的值看**编译类型**，编译器通过编译类型去寻找属性（成员变量）。而方法则是通过**运行类型**，然后根据相应的**继承顺序**来访问（前面写过这个顺序）。

5. `instanceOf` 比较操作符，用于判断对象的**运行类型**是否为XX类型或XX类型的子类型

```java
public class Main {
    public static void main(String[] args) {
        Sub s = new Sub();
        System.out.println(s.count);//20
        s.display();//20
        Base b = s;
        System.out.println(b == s);//true
        System.out.println(b.count);//这里要注意了，属性的值看编译类型，此处b的编译类型是Base，因此count=10
        b.display();//与上面不同，方法从子类开始找起，看的是运行类型，所以其取20
    }
}
```

```java
public class Base {
    int count = 10;
    public void display(){
        System.out.println(this.count);
    }
}

class Sub extends Base{
    int count = 20;
    public void display(){
        System.out.println(this.count);
    }
}
```

[多态示例代码](https://github.com/Bixlku/JavaStudyCode/tree/main/PolyMorphism)

#### Java的动态绑定机制（非常非常重要）

1. 当调用对象方法的时候，该方法会和该对象的内存地址（**运行类型**）绑定
2. 当调用对象属性时，**没有动态绑定机制**，哪里声明，哪里使用

```java
public class DynamicBinding {
    public static void main(String[] args) {
        A a = new B();
        System.out.println(a.sum());//40->30
        System.out.println(a.sum1());//30->20
    }
}
```

```java
public class A {
    public int i = 10;

    public int sum() {
        return getI() + 10;
    }
    public int getI() {
        return i + 10;
    }
    public int sum1() {
        return i + 10;
    }
}

class B extends A {
    public int i = 20;

    // public int sum() {
    //     return i + 20;
    // }
    public int getI() {
        return i;
    }
    // public int sum1() {
    //     return i + 10;
    // }
}
```

[Java动态绑定机制示例代码](https://github.com/Bixlku/JavaStudyCode/tree/main/DynamicBinding)

#### 多态应用

1. 多态数组

   数组的定义类型为父类类型，里面保存的实际元素类型为子类类型

2. 多态参数

   方法定义的形参类型为父类类型，实参类型允许为子类类型

### Object类详解

#### equals和==

==：

1. 如果判断基本类型，则判断的是值是否相等
2. 如果判断引用类型，则判断地址是否相等，即判定是不是同一个对象
3. 如果不是同一个类型，且其无继承关系，会报错

equals：

只能判断引用类型，默认判断的是地址是否相相等，子类中往往重写该方法，用于判断内容是否相等

#### hashCode

1. 提高具有哈希结构的容器的效率
2. 两个引用，如果指向的是同一个对象，则哈希值肯定是一样的
3. 两个引用，如果指向的是不同对象，则哈希值是不一样的
4. 哈希值主要是根据地址号来的，不能将哈希值等价于地址
5. 后面在集合中，hashCode如果需要的话，也会重写

#### toString

1. 默认返回：全类名+@+哈希值的十六进制，子类往往重写toString方法，用于返回对象的属性信息

2. 重写toString方法，打印对象或拼接对象时，都会自动调用该对象的toString形式

3. 输出一个对象时，toString方法会被默认调用

```java
System.out.println(a)会默认调用a.toString()
```

#### finalize

1. 当对象被回收时，系统自动调用该对象的finalize方法。子类可以重写该方法。finalize本身是空的，可以重写该方法来实现自己的业务逻辑。
2. 什么时候被回收：当某个对象没有被任何引用时，则JVM就认为这个对象是一个垃圾对象，就会使用垃圾回收机制来销毁该对象。在销毁该对象前，会先调用finalize方法
3. 垃圾回收机制的调用，是由系统来决定（即有自己的GC算法），也可以通过`System.gc()`主动触发垃圾回收机制

### 零钱通

[零钱通示例代码](https://github.com/Bixlku/JavaStudyCode/tree/main/SmallChange)

### 房屋出租

[房屋出租示例代码](https://github.com/Bixlku/JavaStudyCode/tree/main/HouseRent)

## 第10章 面向对象（高级）

### 类变量（静态变量）

1. 类变量由同一个类所有对象共享
2. 类变量在类加载的时候就生成了

#### 类变量使用细节

1. 什么时候使用类变量

   当我们需要让某个类的所有对象都共享一个变量时，就可以考虑使用类变量（静态变量）

2. 加上`static`称为类变量或静态变量，否则成为实例变量/普通变量/非静态变量

3. 类变量可以通过 `类名.类变量名` 访问或者 `对象名.类变量名` 来直接访问，推荐使用 `类名.类变量名` 访问

   ```java
   Person.id//更好
   Jack.id//不推荐
   ```

### 类方法

#### 类方法基本介绍

类方法也叫静态方法，形式如下：

```java
访问修饰符 static 数据返回类型 方法名(){ } 【推荐】
```

调用方式：

```
类名.类方法名/对象名.类方法名
```

#### 类方法经典的使用场景

当方法中**不涉及到任何和对象相关的成员**，则可以将方法设计成静态方法，提高开发效率。（例如工具类(`utils类`)、Math类、Arrays类。。。。。）即把方法当作工具使用，无需创建对象。

#### 类方法注意事项和细节讨论

1. 类方法和普通方法都是随着类的加载而加载，将结构信息存储在方法区：

   类方法无`this`参数，普通方法隐含`this`参数

2. 类方法可以通过类名调用，也可以通过对象名调用；普通方法和对象有关，需要通过对象名调用

3. 类方法不允许使用和**对象**有关的关键字（fff！！），比如`this`和`super`。普通方法（成员方法）则可以

4. 类方法（静态方法）**只能访问** 静态变量 或 静态方法

5. 普通成员方法，既可以非静态成员，也可以访问静态成员

6. 静态方法只会运行一次

### main方法

1. main方法由虚拟机调用
2. java虚拟机需要调用类的main()方法，所以该方法的访问权限必须是public（虚拟机和main不在同一个类）
3. java虚拟机在调用main()方法时不必创建对象，所以该方法必须是static
4. 该方法接收String()类型的数组参数，该数组中保存执行java命令时传递给所运行的类的参数
5. `java执行的程序 参数1 参数2 参数3` 命令行运行
6. main方法是静态方法，可以直接调用main方法所在的类的静态方法，但是不能访问该类中的非静态成员（必须在创建一个实例后才能访问）

### 代码块

#### 基本介绍

代码块又称为代码块，属于类中的成员，类似于方法，将逻辑语句封装在方法体中，通过{}包围起来。但其和方法不同，没有方法名，没有返回，没有参数，只有方法体，而且不用通过对象或类显式调用，而是加载类时，或创建对象时隐式调用。

#### 基本语法

```java
static (optional) {
	code
};
```

- 修饰符（static）可选，分为静态代码块和普通代码块（非静态代码块）
- 分号（；）可以省略

#### 代码块的好处

- 相当于另一种形式的构造器（对构造器的补充机制），可以做初始化的操作
- 场景：如果多个构造器中都有重复的语句，可以抽取到初始化块中，提高代码的复用性。这样不管用哪个构造器创建任意一个对象，都会调用代码块的内容

#### 代码块使用注意事项和细节讨论

1. static代码块也叫做静态代码块，作用是对类进行初始化，而且它随着类的**加载**而执行，并且只会**执行一次**。

   如果是普通代码块，每**创建**一次实例（new）就执行一次。

   如果只是使用类调用静态成员，普通代码块并不会被执行（可以理解为构造器未被调用）。

2. **类什么时候被加载**【重要！必备】【**加载**不等于**创建**，类加载早于对象创建，类加载不一定创建了对象】

   - 创建对象实例时（new）

   - 创建子类对象实例，父类也会被加载

     ```java
     public class CodeBlock {
         public static void main(String[] args) {
             //类被加载的情况举例
             //1.创建对象(new)
             AA aa = new AA();
             //2.创建子类对象那实例，父类也会被加载，而且父类先被加载，子类后被加载
             BB bb = new BB();
             //3.使用类的静态成员时(静态方法、静态成员)
             int c = Cat.n1;  
         }
     }
     
     class AA{
         static {
             System.out.println("AA的静态代码块1被执行");
         }
     }
     
     class BB extends AA{
         static {
             System.out.println("BB的静态代码块1被执行");
         }
     }
     
     class Cat {
         public static int n1 = 999;
         static {
             System.out.println("Cat的静态代码块1被执行");
         }
     }
     ```

   - 使用类的静态成员时（静态属性，静态方法）

     ```java
     public class CodeBlock {
         public static void main(String[] args) {
             //静态代码块在类加载时执行，而且只会被执行一次
             //下列语句只会输出一次"DD的静态方法被代码块1执行"
             //普通代码块在每创建一次类就会执行一次
             //下列两行会输出两次"DD普通代码块被代码块1执行"
             DD dd = new DD();
             DD dd1 = new DD();
             
             //如果只是使用类调用静态成员，普通代码块并不会被执行
             System.out.println(DD.n1);///输出888，静态代码块会执行，普通代码块不会执行
             
         }
     }
     
     class DD{
         public static int n1 = 888;
         static {
             System.out.println("DD的静态方法被代码块1执行");
         }
         {
             System.out.println("DD普通代码块被代码块1执行")
         }
     }
     ```

3. 创建一个对象时，在一个类调用顺序是【重点，难点】：

   1. 调用静态代码块和静态属性初始化（类加载早于对象创建）

      （注意：静态代码和静态属性初始化调用的优先级一样，如果有多个静态代码和多个静态变量初始化，则按他们定义的顺序调用）

   2. 调用普通代码块和普通属性的初始化

      （注意：普通代码块和普通属性初始化调用的优先级一样，如果有多个普通代码块和多个普通属性初始化，则按定义顺序调用）

   3. 调用构造方法

      ```java
      public class CodeBlock2 {
          public static void main(String[] args) {
              A a = new A();//(1)getN1被调用 (2)A的静态代码块 (3)getN2被调用 (4)A的普通代码块 (5)A无参构造器被调用
          }
      }
      
      class A{
          private static int n1 = getN1();
          private int n2 = getN2();
          {
              System.out.println("A的普通代码块01");//(4)
          }
          static{
              System.out.println("A静态代码块01");//(2)
          }
          
          public static int getN1(){
              System.out.println("getN1被调用");//(1)
              return 100;
          }
          public int getN2(){
              System.out.println("getN2被调用");//(3)
              return 100;
          }
          public A(){
              System.out.println("A无参构造器被调用");//(5)
          }
      }
      ```

   4. 构造方法（构造器）的最前面其实隐含了**super()**和 **调用普通代码块** ，静态相关的代码块，属性初始化，在类加载时就已经执行完毕。

      ```java
      class A{
          public A(){
              //这里有隐藏的执行要求
              //1)super();
              //2)调用本类普通代码块
              System.out.println();
          }
      }
      ```

   5. 创建子类时（有继承关系），他们的静态代码块，静态属性初始化，普通代码块，普通属性初始化，构造方法的调用顺序如下：

      1. 父类的静态代码块和静态属性初始化（优先级一致，按定义顺序执行）
      2. 子类的静态代码块和静态属性初始化（优先级一致，按定义顺序执行）
      3. 父类的普通代码块和普通属性初始化（优先级一致，按定义顺序执行）
      4. 父类构造方法
      5. 子类的普通代码块和普通属性初始化（优先级一致，按定义顺序执行）
      6. 子类构造方法

      [代码块综合测试源代码](https://github.com/Bixlku/JavaStudyCode/blob/main/CodeBlock/CodeBlockExam.java)

   6. 静态代码块只能直接调用静态成员（静态属性和静态方法），普通代码块可以调用任意成员

### 单例设计模式

#### 什么是设计模式

1. 静态方法和属性的经典应用
2. 设计模式是在大量的实践中总结和理论化之后优选的代码结构、编程风格、以及解决问题的思考方式。设计模式就像是经典的棋谱，不同的棋局，我们用不同的棋谱，免去我们自己再思考和摸索

### 什么是单例模式

单例（单个实例）

1. 所谓类的单例设计模式，就是采取一定的方法保证在整个的软件系统中，对某个类只能存在一个对象实例，并且该类只提供一个取得其对象实例的方法
2. 单例模式有两种方式：1） 饿汉式    2）懒汉式

 #### 饿汉式

即使未使用对象，对象也可能被创建了。饿汉式在加载时就创建了对象，有可能并不使用而造成资源浪费。

1. 构造器私有化 =》防止直接new

2. 类的内部创建对象

3. 向外暴露一个静态的公共方法。 `getInstance`（instance：实例）

4. 代码实现

   ```java
   class SingleTon01(){
       private SingleTon01(){}
       //为了能在静态方法中，返回instance对象，因此将其修饰为static
       private static SingleTon01 instance = new SingleTon01();
       
       public static SingleTon01 getInstance(){
           return instance;
       }
   }
   ```

#### 懒汉式

只有使用了getInstance时，才会返回对象，后面再调用时，会返回上次创建的对象，从而保证单例。即使加载类，也不会创建对象

1. 构造器私有化
2. 定义一个static静态属性对象
3. 提供一个public的static方法，返回一个实例

```java
class SingleTon02(){
    private SingleTon01(){}
    
    private static SingleTon02 instance; 
    
    public static SingleTon02 getInstance(){
        if(instance == null) {//如果没创建对象，就进行创建
            instance = new SingleTon02();
        }
        return instance;
    }
}
```

#### 饿汉式VS懒汉式

1. 二者最主要的区别在于创建对象的**时机**不同 ：饿汉式在类加载时就创建了对象实例，而懒汉式是在使用时才创建
2. 饿汉式不存在线程安全问题，懒汉式存在线程安全问题
3. 饿汉式有浪费资源的可能。
4. Java SE中，java.lang.Runtime就是经典单例模式

### final关键字

#### 基本介绍

final可以修饰 类、属性、方法和局部变量

在某些情况下，程序员可能有以下需求，就会用到final：

- 但不希望父类被继承时，可以用final修饰
- 但不希望父类的某个方法被子类覆盖/重写(override)时，可以用final关键字修饰【访问修饰符 final 返回类型 方法名】
- 但不希望类的某个属性的值被修改，可以用final修饰
- 但不希望某个局部变量被修改，可以用final修饰

#### final使用注意事项和细节讨论

1. final修饰的属性又叫常量，一般用`XX_XX_XX`来命名

2. final修饰的属性在**定义**时，必须赋初值，并且以后不能再修改，赋初值可以在如下位置之一：

   1. 定义时：如`public final double TAX_RATE  = 0.08;`
   2. 在构造器中
   3. 在代码块中

3. 如果final修饰的属性是**静态**的，则赋初值的位置只能是

   1. 定义时

   2. 在静态代码块

      不能在构造器中赋值

4. final类不能继承，但是可以实例化对象

5. 如果类不是final类，但是含有final方法，则虽然该方法不能重写，但是可以被继承

6. 一般来说，如果一个类已经是final类了，那么其方法就没必要修饰成final了（继承都不行怎么可能重写）

7. final不能修饰构造方法

8. final和static往往搭配使用，效率更高，**不会导致类加载**，底层编译器做了优化处理。

9. 包装类（Integer，Double，Float，Boolean等）都是final，String也是final类

### 抽象类

当父类的某些方法，需要声明，但是又不确定如何实现时，可以将其声明为抽象方法，那么这个类就是抽象类。

所谓抽象方法就是没有实现的方法，所谓没有实现就是指没有方法体，当父类的一些方法不能确定时，可以用abstract关键字来修饰该方法，这个方法就是抽象方法，用abstract来修饰该类就是抽象类。一般来说，抽象类会被继承，由其子类来实现抽象方法。

#### 抽象类的介绍

1. 用abstract关键字来修饰一个类时，这个类就叫抽象类

   ```java
   访问修饰符 abstract 类名{
   
   }
   ```

2. 用abstract关键字来修饰一个方法时，这个方法就是抽象方法

   ```java
   访问修饰符 abstract 返回类型 方法名(参数列表);//没有方法体
   ```

3. 抽象类的价值更多作用是在于设计，是设计者设计好后，让子类继承并实现抽象类你

   

#### 抽象类的注意事项和细节讨论

1. 抽象类不能被实例化

2. 抽象类不一定要包含abstract方法。也就是说，抽象类可以没有抽象方法

3. 一旦包含了abstract方法，则这个类必须声明为abstract

4. abstract只能修饰 类 和 方法 ，不能修饰属性和其他的

5. 抽象类可以有任意成员【抽象类还是类】，比如：非抽象方法、构造器、静态属性等

6. 抽象方法不能有主体，即下面这种写法是错误的

   ```java
   abstract void a() {}//不能写大括号
   ```

7. 如果一个类继承了抽象类，则它必须实现抽象类的所有抽象方法，除非它本身也声明为抽象类

8. 抽象类不能使用private、final、static，因为这些关键字和重写相违背

### 接口（interface）

接口就是给出一些没有实现的方法，封装到一起，到某个类要使用的时候，再根据具体情况把这些方法写出来。语法（implements：实施/实现)

```java
interface 接口名{
    //属性
    //方法（1.抽象方法 2.默认实现方法 3.静态方法）
}
class 类名 implements 接口 {
    自己属性;
    自己方法;
    必须实现的接口的全部抽象方法;
}
```


小结：

1. 在JDK7.0之前 接口里的所有方法都没有方法体，即都是抽象方法
2. JDK8.0之后接口类可以有静态方法，默认方法，也就是说接口中可以有方法的具体实现
3. 在接口中， 抽象方法可以省略abstract关键字
4. 默认方法需要在方法前加default

#### 注意事项和细节

1. 接口不能被实例化

2. 接口中所有的方法都是**public**方法，接口中抽象方法，可以不用abstract修饰

3. 一个普通类实现接口，就必须将该接口的所有方法都实现（IDEA中可以用`alt`+`enter`解决）

4. **抽象类**实现接口，可以不用实现接口的方法

5. 一个类同时可以实现多个接口

6. **接口中的属性**只能是final的，而且是**public static final** 修饰符。比如

   ```java
   在接口中
   int a = 1;//实际上是public static final int a =1;（必须初始化）
   ```

7. 接口中属性的访问形式：接口名.属性名

8. 一个接口不能继承其它的类，但是可以继承多个别的接口。接口和接口之间可以继承

   ```java
   interface A extends B,C{}
   ```

9. 接口的修饰符只能是public或者默认，这点和类的修饰符是一样的

#### 实现接口 vs 继承类

接口和继承解决的问题不同

- 接口的价值主要在于：解决代码的复用性和可维护性
- 接口的价值主要在于：设计，设计好各种规范（方法），让其他类去实现这些方法

接口比继承更加灵活，继承需要满足 is-a 的关系，而接口只需满足 like-a 的关系

接口在一定程度上实现代码解耦 [即： 接口规范性+动态绑定]

#### 接口的多态性

1. 多态参数

2. 多态数组

3. 多态接口传递

   ```java
   public class InterfacePoly {
       public static void main(String[] args) {
           IF if01 = new IH();
           //如果IG 继承了 IH接口，而Teacher类实现了 IG接口
           //那么，实际上就相当于Teacher类也实现了 IH接口
           //这就是所谓的接口多态传递现象
           IG ig01 = new IH();
       }
   }
   
   interface IG{}
   interface IF extends IG{}
   class IH implements IF{}
   ```

#### 类定义的进一步完善

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20220225110910353.png" alt="image-20220225110910353" style="zoom: 33%;" />

### 内部类

一个类的内部又完整的嵌套了另一个类结构。被嵌套的类称为内部类（inner class），嵌套其他类的类称为外部类（outer class）。是我们类的第五大成员[属性、方法、构造器、代码块、内部类]，内部类的最大特点是可以直接访问私有属性，并且可以体现类与类之间的包含关系。

#### 基本语法

```java
class Outer{//外部类
    class inner{//内部类
    }
}
class Other{//外部其他类 
}
```

#### 内部类的分类

定义在外部类局部位置上：

- 局部内部类（有类名）
- **匿名内部类**（没有内部类，重点！）

定义在外部类的成员位置上：

- 成员内部类（没有static修饰）
- 静态内部类（使用static修饰）

#### 局部内部类

局部内部类是定义在外部类的局部位置，比如方法中，并且有类名

1. 可以直接访问外部类的所有成员，包括私有的
2. 不能添加访问修饰符，但是可以使用`final`修饰。因为它的地位就是一个局部变量，局部变量不能使用修饰符
3. **作用域**：仅仅在定义它的方法或代码块中
4. 局部内部类---访问--->外部类的成员 【访问方式：直接访问】
5. 外部类---访问--->局部内部类的成员 【访问方式：创建对象，再访问（注意：必须在作用域内）】
6. 外部其他类---**不能访问**--->局部内部类（因为局部内部类地位是一个**局部变量**）
7. 如果外部类和局部内部类的成员重名时，默认遵循**就近原则**，如果想访问外部类的成员，则可以使用`外部类名.this.成员`去访问

```java
public class LocalInnerClass {
    public static void main(String[] args) {
    }
}

class Outer02 {
    private int n1 = 100;
    private void m2(){};
    public void m1() {//方法
        class Inner02 {//局部内部类，本质还是一个类
            //可以直接访问外部类的所有成员，包含私有的
            int n1 = 100;
            public void f1() {
                //局部内部类可以直接访问外部类的成员，例如n1和m2
                //如果外部类和局部内部类的成员重名时，默认遵循就近原则
                //如果想访问外部类的成员，则可以使用`外部类名.this.成员`去访问
                //Outer02.this 表示的本质就是外部类的对象，即哪个对象调用了m1，Outer02.this就是哪个对象
                System.out.println("n1=" + n1 + "外部类的n1=" +Outer02.this.n1);
                m2();
            }
        }
        //需要创建Outer类的对象再进行访问
        Outer02 outer02 = new Outer02();
        System.out.println(outer02.n1);
    }
}
```

#### 匿名内部类(重点)

（1）本质是类 （2）内部类 （3）该类没有名字 （4）同时还是一个对象

匿名内部类是定义在外部类的局部位置，比如方法中，并且没有类名的内部类

1. 匿名内部类的基本语法

   ```java
   new 类或接口(参数列表){
       类体
   };
   ```

2. 匿名内部类的语法比较奇特，请注意，因为匿名内部类既是一个类的定义，同时它本身也是一个对象，因此从语法上看，它既有定义类的特征，也有创建对象的特征，对前面代码的分析可以看出这个特点，因此可以调用匿名内部类的的方法。

3. 可以直接访问外部类的所有成员，包含私有的

4. 不能添加访问修饰符，因为他的地位就是一个局部变量

5. 作用域：仅仅在定义它的方法或者代码块中

6. 匿名内部类---访问---->外部类成员[访问方式：直接访问]

7. 外部其他类---不能访问---->匿名内部类（因为 匿名内部类地位是一个局部变量）

8. 如果外部类和内部类的成员重名时，内部类访问的话，默认遵循**就近原则**，如果想访问外部类的成员，则可以使用（`外部类名.this.成员`）去访问

9. 匿名内部类包含了继承、多态、动态绑定、内部类的知识

##### 匿名内部类实践

当作实参传递，简洁高效

[匿名内部类示例代码](https://github.com/Bixlku/JavaStudyCode/tree/main/AnonymousInnerClass)

#### 成员内部类

成员内部类是定义在外部类的成员位置，并且没有static修饰

1. 可以**直接访问**外部类的所有成员，包括私有的

   ```java
   class Outer01{
       private int n1 = 100;
       
       class Inner01{
           public void cry(){
               System.out.println("调用cry方法");
           }
       }
   }
   ```

2. 可以添加任意访问修饰符(public、private、默认、protected)，因为它的定义也是一个成员

3. 作用域和外部类的其他成员一样，为**整个类体**。在外部类的成员方法中创建成员内部类对象，再调用方法

4. 成员内部类---访问---->外部类成员[访问方式：直接访问]

5. 外部类---访问---->内部类（访问方式：创建对象，再访问）

6. 外部其他类---访问---->成员内部类

   ```java
   public class InnerClass {
       Outer08 outer = new Outer08();
       //第一种方式，相当于把new Inner()当作是outer的成员，只是语法，不需要太仔细想
       Outer08.Inner inner1 = outer.new Inner();//第一种外部其他类访问成员内部类的方方式
       //第二种方式，在外部类中，编写一个方法，可以返回Inner对象
       Outer08.Inner inner2 = outer.getInnerInstance();//第二种外部其他类访问成员内部类的方方式
   }
   
   class Outer08{//外部类
       private String name;
       private int n1 = 99;
   
       public class Inner{//内部类
           private int in = 2;
           public void say(){
               System.out.println("调用Inner的say方法");
           }
       }
       
       public Inner getInnerInstance(){
           return new Inner();
       }
   }
   ```

7. 如果外部类和内部类的成员重名时，内部类访问的话，默认遵循**就近原则**，如果想访问外部类的成员，则可以使用（`外部类名.this.成员`）去访问

#### 静态内部类

静态内部类是定义在外部类的成员位置，并且有static修饰

1. 可以直接访问外部类的所有静态成员，包括私有的，但不能直接访问**非静态成员**

2. 可以添加任意访问修饰符，因为它的地位就是一个成员

3. 作用域：同其他的成员，为整个整体

4. 静态内部类---访问---->外部类成员[访问方式：直接访问]

5. 外部类---访问---->静态内部类（访问方式：创建对象，再访问）

6. 外部其他类---访问---->静态内部类（因为 匿名内部类地位是一个局部变量）

   ```java
   public class StaticInnerClass {
       public static void main(String[] args) {
           Outer01 outer = new Outer01();
           //方式1. 因为静态内部类，是可以通过类名直接访问（前提是满足访问权限）
           Outer01.Inner01 inner01 = new Outer01.Inner01();
           inner01.say();
           //方式2. 编写一个方法，可以返回静态内部类的实例
           Outer01.Inner01 inner01Instance = outer.getInner01Instance();//使用非静态方法
           Outer01.Inner01 inner01Instance_ = Outer01.getInner01Instance_();//使用静态方法（如果不想创建一个外部对象可以直接这样用类名）
       }
   }
   
   class Outer01{
       private int n1 = 9;
   
       static class Inner01{
           public void say(){
               System.out.println("调用静态内部类的say方法");
           }
       }
       public Inner01 getInner01Instance(){
           return new Inner01();
       }
       static public Inner01 getInner01Instance_(){
           return new Inner01();
       }
   }
   ```

7. 如果外部类和内部类的成员重名时，内部类访问的话，默认遵循**就近原则**，如果想访问外部类的成员，则可以使用（`外部类名.成员`）去访问

## 第11章 枚举和注解

### 枚举(enumeration)

#### 自定义类实现枚举 

1. 构造器私有化
2. 本类内部创建一组对象
3. 对外暴露对象（通过为对象添加访问修饰符）
4. 可以提供get方法，但是不提供set方法

```java
public class Enumeration01 {
    public static void main(String[] args) {
        System.out.println(Season.AUTUMN.toString());
    }
}

class Season{
    private String name;
    private String desc;

    final public static Season SPRING = new Season("春天","温暖");
    final public static Season SUMMER = new Season("夏天","炎热");
    final public static Season AUTUMN = new Season("秋天","萧瑟");
    final public static Season WINTER = new Season("冬天","寒冷");

    private Season(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }

    private String getName() {
        return name;
    }

    private String getDesc() {
        return desc;
    }
}
```

#### enum关键字实现枚举

1. 使用关键字enum替代class
2. SPRING("春天","温暖") 解读 常量名(实参列表)
3.  如果使用enum来实现枚举，枚举对象应当写在前面

```java
public class Enumeration02 {
    public static void main(String[] args) {
        System.out.println(Season.AUTUMN.toString());
    }
}

enum Season1 {
    SPRING("春天","温暖"),
    SUMMER("夏天","炎热"),
    AUTUMN("秋天","萧瑟"),
    WINTER("冬天","寒冷");
    
    private String name;
    private String desc;
    
    private Season1(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }

    private String getName() {
        return name;
    }

    private String getDesc() {
        return desc;
    }
}
```

##### enum关键字实现枚举的注意事项

1. 当使用enum关键字开发一个枚举类时，默认会**继承Enum类**（使用javap验证）
2. `final public static Season SPRING = new Season("春天","温暖");`   简化为  `SPRING("春天","温暖"),`这里必须理解其调用的是哪个构造器
3. 如果使用无参构造器 创建 枚举对象，则实参列表的小括号可以省略

##### enum常用方法说明

| 方法名            | 详细描述                                                     |
| ----------------- | ------------------------------------------------------------ |
| valueOf           | 传递枚举类型的Class对象和枚举常量名称给**静态方法**valueOf，会得到与参数匹配的枚举常量。将字符串转换成枚举对象，要求字符串必须为已有的常量名，否则报异常。 |
| toString          | 得到当前枚举常量的名称。你可以通过重写这个方法来使得到的结果更易读 |
| equals            | 在枚举类型中可以直接使用`==`来比较两个枚举常量是否相等。Enum提供的这个`equals0`方法，也是直接使用`==`实现的。它的存在是为了在Set、List和Map中使用。注意，equals()是不可变的。 |
| hashCode          | Enum实现了hashCode()来和equals0保持一致。它也是不可变的。    |
| getDeclaringClass | 得到枚举常量所属枚举类型的Class对象。可以用它来判断两个枚举常量是否属于同一个枚举类型。 |
| name              | 得到当前枚举常量的名称。建议优先使用toString()。             |
| ordinal           | 得到当前枚举常量的次序。                                     |
| compareTo         | 枚举类型实现了Comparable接口，这样可以比较两个枚举常量，比较的是编号（按照声明的顺序排列） |
| clone             | 枚举类型不能被Clone.。为了防止子类实现克隆方法，Enum实现了一个仅抛出CloneNotSupportedException异常的不变Clone()。 |
| values            | 含有定义的所有枚举对象（是个数组）                           |

### 注解(annotation)

注解也被称为元数据，用于修饰解释 包、类、方法、属性、构造器、局部变量等数据信息。和注释一样，注解不影响程序逻辑，但注解可以被编译或者运行，相当于前在代码中的补充信息。

1. @Override： 限定某个方法，是重写父类方法，该注解只能用于方法
2. @Deprecated： 用于表示某个程序元素（类，方法等）已过时
   1. 过时不代表不能使用，只是不推荐使用
   2. 可以修饰方法、类、字段、包、参数 等等
   3. @Deprecated可以用于版本升级，过渡使用
3. @SuppressWarnings： 抑制编译器警告
   1. 在大括号中可以抑制不希望看到的警告信息，例如 `@SuppressWarnings({"all"})`，警告类型很多，具体的话查文档
   2. @SuppressWarnings 作用范围和放置位置有关，可以放在类上，也可以放在方法上

#### 元注解

元注解本身作用不大，在看源代码的时候看得懂就行

## 第12章 异常

创建try-catch代码块的快捷键：`ctrl`+`alt`+`windows`+`T`

### 基本概念

Java语言中，将程序中出现的不正常情况称为异常 。

### 异常事件分类

1. **Error**（错误）：Java虚拟机无法解决的严重问题

2. **Exception**（异常）：其他因编程错误或偶然的外在因素导致的一般性问题，可以使用针对性的代码进行处理。Exception也分为两个大类
   1. **运行时异常**RuntimeException（运行时发生的异常）有默认处理机制
   2. **编译时异常**（编译时，编译器检查出的异常）

### 🚩异常体系图

![异常体系图（实线继承，虚线实现）](https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/image-20230907164845191.png)

运行时异常是编译器检查不出来的异常，可不做处理，因为这类异常很普遍

编译时异常是编译器要求必须处置的异常

### 异常处理

1. **try-catch-finally**

   程序员在代码中捕获发生的异常，自行处理

   ```java
   try{
       
   }catch(Exception e){
       //检测到异常
       //系统将异常封装成Exception对象e，传递给catch
       //得到异常对象后，程序员自己处理
       //没有发生异常时，catch代码块不执行
   }finally{
       //不管try代码是否有异常发生，是重要执行finally
   }
   ```

2. **throws**

   将发生的异常抛出，交给调用者（方法）来处理，最顶级的处理者就是JVM

try-catch-finally和throws二选一，若未写明，则默认使用throws

#### try-catch-finally

可以有多个catch语句，捕获不同的异常（进行不同的业务处理），要求**父类异常在后，子类异常在前**，例如：Exception在后，NullPionterException在前。如果发生异常，只会匹配其中的一个catch

try-finally可以配合使用，这种用法相当于没有捕获异常，因此程序会直接崩掉。应用场景：执行一段代码，不管是否发生异常，都必须执行某个业务逻辑

   #### throws

1. 如果一个方法可能产生某种异常，但是并不能确定如何处理这种异常，则此方法应显示地声明抛出异常，表明该方法将不对这些异常进行处理，而由该方法的调用者负责处理
2. 在方法声明中用throws语句可以声明抛出异常的列表，throws后面的异常类型可以是方法中产生的异常类型，也可以是它的父类


```java
public static void f1(){
	f2();//此处f2()在写代码时会报错，因为FileNotFoundException是编译时异常
    //此处有两种选择，1.对f2()代码块进行try-catch。
    //2.继续向上抛出FileNotFoundExecption异常
}
public static void f2() throws FileNotFoundException {}
```

```java
public static void f3(){
	f4();//此处在写代码时不会报错，因为ArithmeticException是运行时异常，不要求程序员显示处理，其有默认处理机制
}
public static void f4() throws ArithmeticException{} 
```

### 自定义异常

#### 基本概念

当程序中出现了某些“错误”，但该错误信息并没有在Throwable子类中描述处理，这是可以自己设计异常类，用于描述该错误信息。

#### 步骤

1. 定义类：自定义异常类名（程序员自己写）继承Exception或RuntimeException
2. 如果继承Exception，属于编译异常
3. 如果继承RuntimeException，属于运行异常

#### 细节

1. 自定义异常一般都是继承RuntimException
2. 把自定义异常做成RuntimeException（运行时异常），可以使用默认的处理机制

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230909225418378.png" alt="image-20230909225418378" style="zoom:80%;" />

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230909225459100.png" alt="image-20230909225459100" style="zoom:80%;" />

### throw和throws的区别

#### 一览表

![image-20230910163535454](http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230910163535454.png)

## 第13章 常用类

### 包装类 Wrapper

1. 针对八种基本数据类型相应的引用类型——包装类

2. 有了类的特点，就可以调用类中的方法

| 基本数据类型 | 包装类    |
| ------------ | --------- |
| boolean      | Boolean   |
| char         | Character |
| byte         | Byte      |
| short        | Short     |
| int          | Integer   |
| long         | Long      |
| float        | Float     |
| double       | Double    |

<img src="https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/image-20230915142938465.png" alt="继承关系" style="zoom: 67%;" />

<img src="https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/image-20230915143030998.png" alt="继承关系" style="zoom:80%;" />

<img src="https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/image-20230915143048622.png" alt="继承关系" style="zoom:80%;" />

#### 包装类和基本数据的转换

1. **装箱**：基本类型->包装类型；**拆箱**：包装类型->基本类型

2. jdk5（包括jdk5）以后都是自动装箱和自动拆箱

3. 自动装箱底层调用的是valueOf()方法，例如：Integer.valueOf()

```java
//手动装箱
int n1 = 100;
Integer integer = new Integer(n1);//手动装箱方法1
Integer integer = Integer.valueOf(n1);//手动装箱方法2
//手动拆箱
int i = integer.intValue();

//jdk5后，自动装箱和自动拆箱
int n2 = 200;
//自动装箱
Integer integer2 = n2;//底层是用的是Integer.valueOf(n2)
//自动拆箱
int n3 = integer2;//底层是用的是intValue()方法
```

#### 包装类型和String类型的相互转换

```java
//包装类(Integer)->String
Integer i = 12;
//方式1
String str = i +"";
//方式2
String str1 = i.toString();
//方式3
String str2 = String.valueOf(i);

//String->包装类(Integer)
String str4 = "114514";
//方式1
Integer integer1 = Integer.parseInt(str4);//自动装箱
//方式2
Integer integer2 = new Integer(str4);//构造器。已经是Deprecated了，官方不建议
```

#### 常用的方法

```java
Integer.MIN_VALUE;//返回最小值
Integer.MAX_VALUE;//返回最大值
Character.isDigit('a');//判断是不是数字
Character.isLetter('a');//判断是不是字母
Character.isUpperCase('a');//判断是不是大写
Character.isLowerCase('a');//判断是不是小写
Character.isWhitespace('a');//判断是不是空格
Character.toUpperCase('a');//转换成大写
Character.toLowerCase('a');//转换成小写
```

#### Integer类的范围提示

```java
Integer n1 = 127;
Integer n2 = 127;
System.out.println(n1==n2);//这里输出true

Integer n3 = 128;
Integer n4 = 128;
System.out.println(n3==n4);//这里输出false
//因为Integer类的源码中，-127-127范围内返回的都是int，而范围外返回的都是new的对象
//只要==的两边有基本数据类型，那么对比的一定是值
```

### 🚩String

1. String类用于存储字符串，即一组字符序列

2. 字符串的字符使用Unicode字符编码，一个字符（不区分字母还是汉字）占两个字节

3. String类常用构造器

   ```java
   String s1 = new String;
   String s2 = new String(String original);
   String s3 = new String(char[] a);
   String s4 = new String(char[] a,int startIndex,int count);
   ```

4. String是final类，**不能**被其他的类**继承**

5. String有属性 private final char value[];用于存放字符串内容

6. 一定要注意：value是一个final类型，不可以修改（final不可修改指的是**不能指向新的地址**，但是**单个字符**的内容是可以变化的）

   ```java
   String name = "jack";
   final char[] value = {'a','b','c'};
   char[] v2 = {'t','o','m'};
   value[0] = 'H';//此处不报错
   value = v2;//此处报错
   ```

7. Serializable：实现了串行化，说明可以在网络中传输

   Comparable：实现了Comparable接口，说明String对象可以比较大小

   ![image-20230916101911861](https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/image-20230916101911861.png)

#### 创建String对象的两种方式

1. 直接赋值 `String s = "hspedu"`。只要不是new出来的，就都会直接放在方法区里面，不会进堆

   ![image-20230916112013487](https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/image-20230916112013487.png)

2. 调用构造器 `String s2 = new String("hsp");`

   ![image-20230916112030189](https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/image-20230916112030189.png)

![image-20230917143702258](https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/image-20230917143702258.png)

```java
s.equals(s2);//equals方法比较的是值，所以这里是True
s == s2;//这里是False，因为s2指向堆，而s指向常量池
//.intern()方法返回的是常量池的地址
//It follows that for any two strings s and t, s.intern() == t.intern() is true if and only if s.equals(t) is true.
s == s2.intern();//此处返回的是True
s == s.intern();//此处返回的是False
```

##### 关于`.intern()`方法的小练习：

```java
public static void main(String[] args) {
	String str1 = "Runoob";
	String str2 = new String("Runoob");
	String str3 = str2.intern();
    System.out.println(str1 == str2);  // false
	System.out.println(str1 == str3);  // true
}
```

其文字解释为：

以上实例中，str1 是直接赋值的字符串常量，它会被自动添加到字符串池中。str2 是通 过new String() 创建的新字符串对象，它不会自动添加到字符串池中。然后，通过调用 intern() 方法，将 str2 添加到字符串池中，并返回字符串池中的引用，保存在 str3 中。

注意，== 运算符用于比较引用是否相等。在上面的示例中，str1 == str3 返回 true，这是因为它们都引用字符串池中的同一个对象。

其图像解释为：

<img src="https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/e717021464726c7b4b32792e037889e.png" alt="e717021464726c7b4b32792e037889e" style="zoom:80%;" />

```java
public class WrapperType02 {
    public static void main(String[] args) {
        Person p1 = new Person();
        p1.name = "test";
        Person p2 = new Person();
        p2.name = "test";

        System.out.println(p1.name.equals(p2.name));//True
        System.out.println(p1.name == p2.name);//True
        System.out.println(p1.name == "test");//True

        String s1 = new String("test2");
        String s2 = new String("test2");
        System.out.println(s1==s2);//False
        //搞清楚内存分布图
    }
}
```



#### 字符串特性

1. 编译器会自动进行优化，判断创建的常量池对象，例如`String a = "hello" + "world"`会直接优化成`String a = "helloworld"`。所以只会创建一个对象。

```java
String a = "hello";
String b = "abc";
String c = a + b;//c=a+b这里是有讲究的
//1. 先创建一个StringBuilder sb = StringBuilder()
//2. 执行 sb.append("hello");
//3. sb.append("abc")
//4. 相当于String c = sb.toString()
//最后其实是c指向堆中的对象
String d = "helloabc"
d == c//返回False，因为c指向的是堆中的对象，而d直接指向池中的对象
String e = "hello" + "abc"
e == d//返回True
```

##### 练习题：画出下图的内存布局图

```java
public class Test1 {
    String str = new String("hsp");
    char[] ch = {'j','a','v','a'};
    public static void main(String[] args) {
        Test1 ex = new Test1();  
        ex.change(ex.str,ex.ch);
        System.out.println(ex.str+" and ");
        System.out.println(ex.ch);
    }
    
    public void change(String str,char[] ch){
        str = "java";
        ch[0] = 'h';
    }
}
```

<img src="https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/image-20230918114742030.png" alt="内存布局图" style="zoom:80%;" />

### String类的常用方法

String类是保存字符串常量的。每次更新都需要重新开辟空间，效率较低。因此开发者设计了StringBuilder和StringBuffer来增强String的功能
* `equals` 区分大小写判断内容是否相等
* `equalsIgnoreCase` 忽略大小写判断内容是否相等
* `length` 获取字符的个数
* `indexOf` 获取字符串中第一次出现的索引，索引从0开始，如果找不到，就返回-1
* `lastIndexOf` 获取字符在字符串中最后一次出现的索引，索引从0开始，如果找不到，就返回-1
* `substring` 截取指定范围内的子串
* `trim` 去掉前后空格
* `charAt` 获取某索引处的字符，注意没有`str[index]`这种形式 
* `toUpperCase` 变成大写字符
* `toLowerCase` 变成小写字符
* `concat` 拼接字符串
* `replace` 替换字符串中的字符
* `split` 分割字符串
* `compareTo` 比较两个字符串的大小（不懂的话建议去看源码）

  1.  返回值为0时，说面两个字符串相等
  2.  若前者大，则返回正数；若后者大，则返回负数
* `toCharArray` 转换成字符数组
* `format` 格式字符串（类c写法）

### StringBuffer类
![20230919141502](https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/vscode/20230919141502.png)

1. StringBuffer的直接父类是AbstractBuilder
2. StringBuffer实现了Serialzable，即StringBuffer的对象可以串行化
3. 在父类中 AbstractStringBuilder有属性byte[] value，不是final。该value数组存放字符内容，引出存放在堆中。
   
   从jdk9开始char[]被改为了byte[]，因为AbstractStringBuilder大部分情况下存放的都是拉丁字母，使用Byte的话一个拉丁字母只需要占据1个字节的容量，所以现在使用非拉丁字母时，使用UTF-16编码，需要使用isLatin1进行区分
4. StringBuilder类是一个final类，不能被继承
5. StringBuilder的字符内容存放在byte[] value中，所以更改后不是每次都要变换地址（如果空间不够还是会改变地址的）

### String VS StringBuffer
1. String保存的是字符串常量，里面的值不能更改，每次String类的更新实际上就是更改地址，效率较低//`private final char value[]`
2. StringBuffer保存的是字符串变量，里面的值可以更改，每次StringBuffer的更新实际上可以更新内容，不用每次都更新地址，所以效率比较高。//`char[] value` //这个放在堆中


### StringBuffer构造器
1. `StringBuffer()` 构造一个其中不带字符的字符串缓冲区，其初始容量为16个字符。
2. `StringBuffer(CharSequence seq)` 构造一个字符串缓冲区，它包含与指定的 CharSequence 相同的字符。
3. `StringBuffer(int capacity)` 构造一个不带字符，但具有指定初始容量的字符串缓冲区。即对char［］大小进行指定
4. `StringBuffer(String str)` 构造一个字符串缓冲区，并将其内容初始化为指定的字符串内容。其cahr[]大小为str.length()+16

### StringBuffer转换
String -> StringBuffer

1. 使用构造器

    ```
    StringBuffer sb = new StringBuffer(str);
    ```

2. 使用append()方法
    
    ```
    StringBuffer sb = new StringBuffer(); 
    sb = sb.append(str);
    ```
StringBuffer -> String

1. 使用StringBuffer提供的toString方法
    ```
   String s = sb.toString()
   ```

2. 使用构造器

    ```
    String s1 = new String(sb);
    ```

### StringBuffer的常用方法

1. `append` 增
2. `delete(start,end)` 删
3. `replace(start,end,string)` 改。将start-end区间的内容替换为string
4. `indexOf` 查
5. `insert(index,str)` 在index处插入str，原来索引为index的元素自动后移
6. `length` 获取长度

### StringBuilder类
1. 可变字符序列，使用与StringBuffer兼容的API ，但是不保证同步（不是线程安全的）。用作StringBuffer的简易替换，用在字符串缓冲区被单线程使用的时候。如果可能建议使用该类，因为其比StringBuffer要快
2. StringBuilder的主要操作是append和insert方法，可重载这些方法，以接受任意类型的数据

### String、StingBuffer、StringBuilder比较
1. StringBuilder和StringBuffer非常类似，均代表可变的字符序列，而且方法也一样
2. String：不可变字符序列，效率低，但是复用率高。如果需要对String进行大量修改，就不要使用String，应当选择StringBuffer和StringBuilder
3. StringBuffer：可变字符序列、效率较高（增删）、线程安全
4. StringBuilder：可变字符序列、效率最高、线程不安全

### Math类
常用的**静态方法**
1. `abs()` 求绝对值
2. `pow()` 求幂
3. `ceil()` 向上取整
4. `floor()` 向上取整
5. `round()` 四舍五入
6. `sqrt()` 求开方
7. `random()` 求随机数，返回一个[0,1)的随机小数
8. `max()` 求两个数的最大值
9. `min()` 求两个数的最小值

### Arrays类
1. `Arrays.toString()` 显示数组
2. `Arrays.sort()` 排序 

    * 因为数组是引用类型，所以通过sort排序后，会直接影响到实参arr。

    * sort是重载的，也可以通过传入一个接口 Comparator 实现定制排序
    
    * 定制排序：
    
    ```java
    Arrays.sort(arr,new Comparator(){
        @Override
        public int compare(Object o1, Object o2){
            相关程序代码
            return int1-int2;
        }
    })
    ```
    
    传入两个参数：

    （1） 排序的数组`arr`
    
    （2）实现了Comparator接口的匿名内部类，要求其实现compare方法。其底层调用了**二叉排序**，compare方法返回的int值的正负会直接影响排序是从小到大还是从大到小。如果i1-i2则为从小到大，反之则为从大到小

3. `Arrays.binarySearch()` 通过二分搜索法进行查找，要求数组有序

4. `copyOf` 数组元素的复制 
    
    `Integer[] newArr = Arrays.copyOf(arr,arr.length)` 从arr数组中，拷贝arr.length个元素到newArr数组中。如果拷贝的长度大于arr.length，就在新数组后面加null；如果拷贝的长度小于0，就抛出异常NegativeArraySizeException。

5. `Arrays.fill(arr,element)` 数组填充。将所有元素替换为element

6. `Arrays.equals(arr1,arr2)` 比较两个数组的内容是否完全一致

7. `Arrays.asList()` 将一组值，转换为list。

### System类

1. `exit` 退出当前程序
2. `arraycopy()` 复制数组元素，适合底层调用。一般选用`Arrays.copyOf()`，其底层调用的就是`arraycopy()`
3. `currentTimeMillens()` 返回当前时间距离1970-1-1的毫秒数
4. `gc` 运行垃圾回收机制 System.gc()

### BigInteger和BigDecimal类

应用场景：

1. BigInteger适合保存比较大的整型

2. BigDecimal适合保存进度更高的浮点型（小数）


对BigInteger或BigDecimal进行加减乘除时，不能直接写运算符号，而应该调用对应的方法

在调用devide方法时，需要指定精度，即BigDecimal.ROUND_CEILING。其精度与分子相同

## 第14章 集合

### 集合的理解和好处

数组：
1. 长度开始时必须指定，而且一旦指定，不能修改
2. 保存的必须为同一类型的元素
3. 使用数组进行增加/删除元素 比较麻烦

集合：
1. 可以动态保存任意多个对象，使用方便
2. 提供一系列方便的操作对象的方法：add、remove、set、get
3. 使用集合添加、删除元素比较间接

### 集合框架体系

必背

#### Collection：单列集合
![20230924170531](https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/vscode/20230924170531.png)

#### Map：双列集合
![20230924170449](https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/vscode/20230924170449.png)

### Collection接口和常用方法

#### Collection接口实现类的特点

```java
public interface Collection<E> extends Iterable<E>
```

1. Collection实现子类可以存放多个元素，每个元素可以是Object
2. 有些Collection实现类可以存放重复元素，有些不能；有些是有序的（List），有些是无序的（Set）
3. Collection接口**没有直接**的子类，是通过它的子接口Set和List来实现的

#### 使用ArrayList演示常用方法

1. `add`： 添加单个元素（有自动装箱功能）
2. `remove`： 删除指定元素
    `list.remove(int index)`删除序列为index的元素
    `list.remove(Object o)`删除指定元素的第一个出现。 如果不包含该元素，则不会更改。
3. `contains`：查找元素是否存在
4. `size`：获取元素个数
5. `isEmpty`：判断是否为空
6. `clear`：清空
7. `addAll`：添加多个元素
8. `containsAll`：查找多个元素是否都存在
9. `removeAll`：删除多个元素

#### Collection接口遍历元素方式
1. 使用Iterator（迭代器）
    
    * Iterator对象称为迭代器，主要用于遍历Collection集合中的元素
    * 所有实现了Collection接口的集合类都有一个iterator()方法，用以返回一个实现了Iterator()接口的对象，即可以返回一个迭代器
    * 使用Iterator.next()之前，必须先用hasNext()，否则可能会抛出NoSuchElementException
    * 如果希望再次遍历，需要重置迭代器

2. 增强for循环
    
    增强for循环，可以代替iterator迭代器，特点：增强for就是简化版的iterator，本质一样。只能用于遍历集合或数组。

    基本语法
    ```java
    for(元素类型 元素名:集合名或数组名)
        访问元素
    ```

    实际上增强for循环底层就是调用了iterator的next()。可以理解为简化版本的迭代器遍历。

    快捷方式：`I`

### List接口

1. List集合类中元素有序（即添加顺序和去除顺序一致）、且可重复
2. List集合中的每个元素都有其对应的顺序索引，即支持索引
3. List容器中的元素都对应一个整数型的序号记载其在容器中的位置，可以根据序号存取容器中的元素
4. 实现List接口的实现类有很多，常见的有ArrayList、LinkedList和Vector

#### 常用方法

1. `void add(int index, Object ele)` 在index位置插入ele元素

2. `boolean addAll(int index, Collection eles)` 从index位置开始将eles中的所有元素都添加进来

3. `Object get(int index)` 获取指定index位置的元素

4. `int indexOf(Object obj)` 返回obj在集合中首次出现的位置

5. `int lastIndexOf(Object obj)` 返回obj在当前集合中末次出现的位置

6. `Object remove(int index)` 移除指定index位置的元素，并返回此元素

7. `Object set(int index, Object ele)` 设置指定index位置的元素为ele，相当于是替换

8. `List subList(int fromIndex, int toIndex)` 返回从fromIndex到toIndex位置的子集

### ArrayList的注意事项

1. permits all elements, including null, ArrayList 可以加入null，并且支持加入多个null

2. ArrayList底层是基于数组实现的

3. ArrayList基本等同于Vector，但是ArrayList是线程不安全的；在多线程情况下不建议使用ArrayList

### ArrayList底层结构和源码分析

1. ArrayList中维护了一个Object[]类型的数组elementData。transient Object[] elementData; transient 短暂的、瞬间；表示该属性不会被序列化

2. 创建ArrayList对象时，如果使用无参构造器，则初始化elementData容量为0，第一次添加，则扩容elementData为10，如需要再次扩容，则扩容elementData为1.5倍大小

3. 如果使用的是指定大小的构造器，则初始elementData容量为指定大小，如果需要扩容，则直接扩容elementData为1.5倍

### ArrayList和LinkedList的比较

![20230926191930](https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/vscode/20230926191930.png)

一般来说，在程序中，80%-90%是改查，所以大部分情况下使用的是ArrayList

### Set接口和常用方法

#### Set接口基本介绍

1. 无序（添加和取出的顺序不一样），虽然无序，但是**固定顺序**没有索引
2. 不允许重复元素，所以最多包含一个null
3. JDK API中Set接口的实现类

#### Set接口常用方法

和List接口一样，Set接口是Collection的子接口，所以常用方法和Collection接口一样

#### Set接口的遍历方式

和Collection遍历方式一致

1. 使用迭代器
2. 增强for
3. **不能**使用索引的方式

### HashSet的全面说明

1. HashSet实现了Set接口
2. HashSet底层实际上是HashMap
3. 可以存放null值，但是只能有一个null
4. HashSet不保证元素是有序的，取决于hash后，再确定索引的结果。即不保证取出和存入顺序一致
5. 不能有重复元素/对象


示例代码：
```java
public class Main {
    public static void main(String[] args) {
        Emploee yyh = new Emploee("yyh", "1");
        Emploee yyh1 = new Emploee("yyh", "1");
        HashSet a = new HashSet();
        System.out.println(a.add(yyh));//返回true
        System.out.println(a.add(yyh1));//此处会返回false，因为hashcode和equals返回的值都是一样的
    }
}

class Emploee {
    private String name;
    private String age;

    public Emploee(String name, String age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object o) {//重写equals接口
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Emploee emploee = (Emploee) o;
        return Objects.equals(name, emploee.name) && Objects.equals(age, emploee.age);
    }

    @Override
    public int hashCode() {//重写每一个Object类都有的hashCode接口
        return Objects.hash(name, age);
    }
}
```

   
### Map接口和常用方法
1. Map和Collection并列存在。用于保存具有映射关系的数据：Key-Value，即键值对
2. Map中的Key和value可以是任何引用类型的数据，会封装到HashMap$Node对象中
3. Map中的Key不允许重复，原因和HashSet一样，当`put()`的Key重复时，等价于替换
4. Map中的value可以重复
5. Map中的key可以为null，value也可以为null，注意key为null只能有一个，而value为null可以有多个
6. **常用**String类作为Map的key
7. key和value之间存在单向一对一关系，即通过指定的key总能找到对应的value
8. Map存放数据的key-value示意图，一对k-v是放在一个HashMap$Node中的，又因为Node实现了Entry接口，有些书上也说一对k-v就是一个Entry
```java
Map map = new HashMap();
map.put("no1","yyh");
map.put("no2","yyh2");

Set set = map.entrySet();
System.out.println(set.getClass());

//static class Node<K,V> implements Map.Entry<K,V>
//k-v存放于HashMap$Node中，其代码格式为HashMap$Node node = newNode(hash,key,value,null)
//entrySet集合的存在是为了方便程序员进行遍历，该集合存放的元素类型是Entry，一个Entry对象有k和v，即EntrySet<Entry<K,V>>
//entrySet中存放的是Entry，但是其实际上存放的是HashMap$Node。因为HashMap$Node实现了Entry接口，static class Node<K,V> implements Map.Entry<K,V>
//实际上Entry中的k和v都是指向Node的。只是指向！

for(Object obj : set){
    System.out.println(obj.getClass());//输出class java.util.HashMap$Node
}
```

### Map接口和常用方法

1. `put` 添加
2. `remove` 根据键删除映射关系
3. `get` 根据键获取值
4. `size` 获取元素个数
5. `isEmpty` 判断各户是否为0
6. `clear` 清除
7. `containsKey` 查找键是否存在

#### Map接口的遍历方法
1. `containsKey` 查找键是否存在
2. `keySet` 获取所有的键
3. `entrySet` 获取所有关系
4. `values` 获取所有的值
   
第一组：先取出所有的key，通过key取出对应的Value
```java
Set keyset = map.keySet();
//(1)增强for循环
for(Object key : keyset){
    System.out.println(key + "-" +map.get(key));
}
//(2)迭代器
Iterator iterator keyset.iterator();
while(iterator.hasNext()){//快捷键itit
    Object next = iterator.next()
}
```
第二组：把所有的values取出来
```java
Collection values = map.values();
//这里可以使用所有的Collections使用的遍历方法
//(1)增强for循环
for(Object value : values){
    System.out.println(value);
}
//(2)迭代器
Iterator iterator = values.iterator();
while(iterator.hasNext()){//快捷键itit
    Object next = iterator.next()
}
```
第三组：通过EntrySet来获取k-v
```java
//(1)增强for循环
for(Object entry : entrySet) {
    Map.Entry m = (Map.Entry) entry;
    System.out.println(m.getKey() + "-" + m.getValue());
}
//(2)迭代器
Iterator iterator = entrySet.iterator();
while(iterator.hasNext()){
    Object next = iterator.next();//HashMap$Node -实现-> Map.Entry(getKey,getValue)，HashMapNode没提供相应的方法。。。
    Map.Entry m = (Map.Entry) entry;
    System.out.println(m.getKey() + "-" + m.getValue());
}
```

## 反射
Java的反射是指程序在运行期可以得到一个对象的所有信息
![20231012161513](https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/vscode/20231012161513.png)

1. Class也是类，因此也继承Object类 ［类图］
2. Class类对象不是new出来的，而是**系统创建**的［演示］
3. 对于某个类的Class类对象，在内存中只有一份，因为类**只加载一次** ［演示］
4. 对于每个类的实例都会记得自己是由哪个Class实例所生成的
5. 通过Class对象可以完整地得到一个类的**完整结构**，通过一系列API
6. Class对象是存放在**堆**的
7. 类的字节码二进制数据，是放在**方法区**的，有的地方称为类的元数据（包括 方法代码变量名，方法名，访问权限等等）

### Class类常用方法
包中的Animal.java文件，其中包含了一个Animal Class，文件位置在com.yyh.class_
```java
package com.yyh.class_;

public class Animal {
    public String name = "cat";
    public int age = 10;
    public String home = "Asia";

    @Override
    public String toString() {
        return "Animal{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", home='" + home + '\'' +
                '}';
    }
}
```
Class类常用方法如下
```java
String classAllPath = "com.yyh.class_.Animal";
//1. 获取到Animal类 对应的 Class对象
//<?>表示不确定的Java类型
Class<?> cls = Class.forName(classAllPath);
//2. 输出cls
System.out.println(cls);//显示cls的对象，是哪个Class对象 class com.yyh.class_.Animal
System.out.println(cls.getClass());//输出cls的运行类型 class java.lang.Class
//3. 得到包名
System.out.println(cls.getPackage().getName());
//4. 得到全类名
System.out.println(cls.getName());
//5. 通过clas创建对象实例
Animal cat = (Animal)cls.newInstance();
System.out.println(cat);
//6. 通过反射获取属性name
Field name = cls.getField("name");
//如果name这里是私有属性或者default属性都会报错NoSuchFileException
System.out.println(name.get(cat));
//7. 通过反射给属性赋值
name.set(cat,"HelloKitty");
System.out.println(name.get(cat));//输出HelloKitty
//8. 遍历得到所有的字段属性
Field[] fields = cls.getFields();
for(Field f : fields){
    System.out.println(f.getName());//此处只输出public属性
    /*输出:
    name
    age
    home*/
```

### 获取Class对象的六种方法
```java
//1. Class.forName
        String classAllPath = "com.yyh.class_.Animal";
        Class<?> cls1 = Class.forName(classAllPath);
        //2. 类名.class，应用场景：参数传递
        Class cls2 = Animal.class;
        System.out.println(cls2);
        //3. 对象.getClass()，应用场景：有对象实例
        //反映了 反射 的本质，即程序在运行期可以得到一个对象的所有信息，这个信息里面就包括了对象对应的类的信息
        Animal cat = new Animal();
        Class<? extends Animal> cls3 = cat.getClass();
        System.out.println(cls3);
        //4. 通过类加载器来获取到类的Class对象
        //(1)先得到类加载器 car
        ClassLoader classLoader = cat.getClass().getClassLoader();
        //(2)通过类加载器得到Class对象
        Class cls4 = classLoader.loadClass(classAllPath);
        System.out.println(cls4);
        //cls1,cls2,cls3,cls4其实是同一个对象
        //5. 基本数据类型（int,char,bytr,long......）按如下方式得到Class类对象
        Class<Integer> integerClass = int.class;
        System.out.println(integerClass);//int
        //6. 基本数据类型对应的包装类，可以通过.TYPE得到Class类对象
        Class type = Integer.TYPE;
        System.out.println(type);//int

        //integerClass和type实际上也是同样的类，底层会进行自动装箱和自动拆箱
```