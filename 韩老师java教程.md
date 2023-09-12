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

## 面向对象（初级）

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

## 面向对象（中级）

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
     - 编译类型看左边，运行类型看右边。
     
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

## 面向对象（高级）

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

## 枚举和注解

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

1. Error（错误）：Java虚拟机无法解决的严重问题

2. Exception：其他因编程错误或偶然的外在因素导致的一般性问题，可以使用针对性的代码进行处理。Exception也分为两个大类
   1. 运行时异常RuntimeException（运行时发生的异常）有默认处理机制
   2. 编译时异常（编译时，编译器检查出的异常）

### 🚩异常体系图

![异常体系图（实线继承，虚线实现）](https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/image-20230907164845191.png)

运行时异常是编译器检查不出来的异常，可不做处理，因为这类异常很普遍

编译时异常是编译器要求必须处置的异常

### 异常处理

1. try-catch-finally

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

2. throws

   将发生的异常抛出，交给调用者（方法）来处理，最顶级的处理者就是JVM

try-catch-finally和throws二选一，若未写明，则默认使用throws

#### try-catch-finally

可以有多个catch语句，捕获不同的异常（进行不同的业务处理），要求父类异常在后，子类异常在前，例如：Exception在后，NullPionterException在前。如果发生异常，只会匹配其中的一个catch

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
2. 把自定义异常做成运行时异常，可以使用默认的处理机制

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230909225418378.png" alt="image-20230909225418378" style="zoom:80%;" />

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230909225459100.png" alt="image-20230909225459100" style="zoom:80%;" />

### throw和throws的区别

#### 一览表

![image-20230910163535454](http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230910163535454.png)
