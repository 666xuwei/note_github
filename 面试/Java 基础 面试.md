[TOC]

### 一、基础

#### 1、Java 的跨平台原理

由于各操作系统支持的指令集不是完全一致的，就会让程序在不同的操作系统上执行不同的代码。java 通过不同的系统、不同版本、不同位数的 java 虚拟机来屏蔽不同的系统指令集差异而对外提供统一的接口，对于我们普通的 java 开发者而言，只需要按照接口开发即可。如果我们的程序需要部署在不同的环境时，只需在对应环境下下载安装对应的 java 虚拟机即可。

#### 2、JVM、JRE、JDK 之间的关系

- JDK：包括了 Java 运行环境 JRE、Java 开发工具和 Java 基础类库。

- JRE：:是 java 的运行环境，光有 JVM 还不能让class文件执行，因为在解释 class 的时候 JVM 需要调用解释所需要的类库 lib。在 jre 目录下有两个文件夹 bin 和 lib, 可以简单的认为 bin 里的就是 jvm，lib 中则是 jvm 工作所需要的类库，而 jvm 和 lib 和起来就称为 jre。所以，在你写完 java 程序编译成 .class之后，你可以把这个 .class 文件和 jre 一起打包发给朋友，这样你的朋友就可以运行你写程序了（jre 里有运行 .class 的 java.exe）。 
- JVM：java 虚拟机，java 文件编译成.class 文件，在虚拟机上执行。是 java 能够跨平台使用的基础。

#### 3、Java 的基本数据类型

| 序号 | 数据类型        | 位数 | 默认值 | 取值范围       | 举例说明          |
| ---- | --------------- | ---- | ------ | -------------- | ----------------- |
| 1    | byte(位)        | 8    | 0      | -2^7 - 2^7-1   | byte b = 10;      |
| 2    | short(短整数)   | 16   | 0      | -2^15 - 2^15-1 | short s = 10;     |
| 3    | int(整数)       | 32   | 0      | -2^31 - 2^31-1 | int i = 10;       |
| 4    | long(长整数)    | 64   | 0      | -2^63 - 2^63-1 | long l = 10l;     |
| 5    | float(单精度)   | 32   | 0.0    | -2^31 - 2^31-1 | float f = 10.0f;  |
| 6    | double(双精度)  | 64   | 0.0    | -2^63 - 2^63-1 | double d = 10.0d; |
| 7    | char(字符)      | 16   | 空     | 0 - 2^16-1     | char c = 'c';     |
| 8    | boolean(布尔值) | 8    | false  | true、false    | boolean b = true; |

#### 4、一个".java"源文件中是否可以包括多个类（不是内部类）？有什么限制？

可以有多个类，但只能有一个 public 的类，并且 public 的类名必须与文件名相一致。

扩展：

因为 java 程序是从一个public类的main( )函数开始执行的，编译器在编译时，针对一个java源代码文件（编译单元）只会接受一个public类。

每个编译单元只有单一的公共接口，用public类来表现，如果很多public 类，那程序从何运行？这个单一的公共接口可以包含多个支持包访问权限的类。如果有一个以上的public 类，编译器就会报错。同时，public类的名称必须完全与含有该编译单元的文件名完全一致。如果不一致，也会导致将编译错误。

#### 5、Java 有没有 goto？

- Java中有两个未使用的保留字：goto、const
- **Java**中有三个特殊含义的单词：null、ture、alse
- **JDK1.4 **后追加了 **assert** **关键字；**
- **JDK1.5 **以后追加了 **enum** **关键字**。

#### 6、说说&和&&的区别

先说两者共同点，再分开说。

- &和&&都可以用作逻辑与的运算符，表示逻辑与（and），当运算符两边的表达式的结果都为 true 时，整个运算结果才为 true，否则，只要有一方为 false，则结果为 false。
- &&还具有短路的功能，即如果第一个表达式为 false，则不再计算第二个表达式，例如，对于 if(str != null && !str.equals(“”))表达式，当 str 为 null 时，后面的表达式不会执行，所以不会出现 NullPointerException，如果将&&改为&，则会抛出 NullPointerException 异常。
- &还可以用作位运算符，当&操作符两边的表达式不是 boolean 类型时，&表示按位与操作，我们通常使用 0x0f 来与一个整数进行&运算，来获取该整数的最低 4 个 bit 位，例如，0x31 & 0x0f 的结果为 0x01。

#### 7、在 JAVA 中如何跳出当前的多重嵌套循环？

- 可以在外面的循环语句前定义一个标号，然后在里层循环体的代码中使用带有标号的 break 语句，即可跳出外层循环。例如，

```java
ok:
for(int i=0;i<10;i++){
    for(int j=0;j<10;j++){
        System.out.println(“i=” + i + “,j=” + j);
        if(j == 5) break ok;
    }
}
```

- 让外层的循环条件表达式的结果可以受到里层循环体代码的控制，例如，要在二维数组中查找到某个数字。

```java
int arr[][] = {{1,2,3},{4,5,6,7},{9}};
boolean found = false;
for(int i=0;i<arr.length && !found;i++){
    for(int j=0;j<arr[i].length;j++){
        System.out.println(“i=” + i + “,j=” + j);
        if(arr[i][j] == 5) {
            found = true;
            break;
        }
    }
}
```

#### 8、switch 语句能够作用在 byte 上，能否作用在 long 上，能否作用在 String 上?

在 switch（expr1）中，expr1 只能是一个整数表达式或者枚举常量（更大字体），整数表达式可以是 int 基本类型或 Integer 包装类型，由于，byte,short,char 都可以隐含转换为 int，所以，这些类型以及这些类型的包装类型也是可以的。显然，long 和 String 类型都不符合 switch 的语法规定，并且不能被隐式转换成 int 类型，所以，它们不能作用于 swtich 语句中。

#### 9、short s1 = 1; s1 = s1 + 1;有什么错? short s1 = 1; s1 += 1;有什么错?

- 对于 short s1 = 1; s1 = s1 + 1; 由于 s1+1 运算时会自动提升表达式的类型，所以结果是 int 型，再赋值给 short 类型 s1 时，编译器将报告需要强制转换类型的错误。
- 对于 short s1 = 1; s1 += 1;由于 += 是 java 语言规定的运算符，java 编译器会对它进行特殊处理，因此可以正确编译

#### 10、char 型变量中能不能存贮一个中文汉字?为什么?

char 型变量是用来存储 Unicode 编码的字符的，unicode 编码字符集中包含了汉字，所以，char 型变量中当然可以存储汉字啦。不过，如果某个特殊的汉字没有被包含在 unicode 编码字符集中，那么，这个 char 型变量中就不能存储这个特殊汉字。补充说明：unicode 编码占用两个字节，所以，char 类型的变量也是占用两个字节。

**扩展：**

> - **ASCII编码：**用来表示英文，它使用1个字节表示，其中第一位规定为0，其他7位存储数据，一共可以表示128个字符。**大写字母（**A~Z): 65(A)~90(Z)， 小写字母（a~z): 97(a)~122(z)。
> - **拓展ASCII编码：**用于表示更多的欧洲文字，用8个位存储数据，一共可以表示256个字符
> - **GBK/GB2312/GB18030：**表示汉字。GBK/GB2312表示简体中文，GB18030表示繁体中文。
> - **Unicode编码：**包含世界上所有的字符，是一个字符集。
> - **UTF-8：**是Unicode字符的实现方式之一，它使用1-4个字符表示一个符号，根据不同的符号而变化字节长度。

#### 11、用最有效率的方法算出 2 乘以 8 等於几?

2 << 3，因为将一个数左移 n 位，就相当于乘以了 2 的 n 次方，那么，一个数乘以 8 只要将其左移 3
位即可，而位运算 cpu 直接支持的，效率最高，所以，2 乘以 8 等於几的最效率的方法是 2 << 3。

#### 12、== 和 equals 的区别是什么?

==操作符专门用来比较两个变量的值是否相等，也就是用于比较变量所对应的内存中所存储的数值是否相同，要比较两个基本类型的数据或两个引用变量是否相等，只能用==操作符。如果一个变量指向的数据是对象类型的，那么，这时候涉及了两块内存，对象本身占用一块内存（堆内存），变量也占用一块内存，例如 Objet obj = new Object();变量 obj 是一个内存，new Object()是另一个内存，此时，变量 obj 所对应的内存中存储的数值就是对象占用的那块内存的首地址。对于指向对象类型的变量，如果要比较两个变量是否指向同一个对象，即要看这两个变量所对应的内存中的数值是否相等，这时候就需要用==操作符进行比较。equals 方法是用于比较两个独立对象的内容是否相同，就好比去比较两个人的长相是否相同，它比较的两个对象是独立的。例如，对于下面的代码：

String a=new String("foo");
String b=new String("foo");

两条 new 语句创建了两个对象，然后用 a,b 这两个变量分别指向了其中一个对象，这是两个不同的对象，它们的首地址是不同的，即 a 和 b 中存储的数值是不相同的，所以，表达式a==b 将返回 false，而这两个对象中的内容是相同的，所以，表达式 a.equals(b)将返回 true。在实际开发中，字符串的比较基本上都
是使用 equals 方法。

如果一个类没有自己定义 equals 方法，那么它将继承 Object 类的 equals 方法，Object 类的equals 方法的实现代码如下：

```java
boolean equals(Object o){
	return this==o;
}
```

这说明，如果一个类没有自己定义 equals 方法，它默认的 equals 方法（从 Object 类继承的）就是使用==操作符，也是在比较两个变量指向的对象是否是同一对象，这时候使用 equals和使用==会得到同样的结果，如果比较的是两个独立的对象则总返回 false。如果你编写的类希望能够比较该类创建的两个实例对象的内容是否相同，那么你必须覆盖 equals 方法，由你自己写代码来决定在什么情况即可认为两个对象的内容是相同的。

#### 13、JDK 中哪些类是不能被继承的？

String，StringBuffer，System

#### 14、数组有没有 length()方法？String 有没有 length()方法？

数组没有 length()方法，有 length 的属性。String 有 length()方法。JavaScript中，获得字符串的长度是通过 length 属性得到的，这一点容易和 Java 混淆。

#### 15、当一个对象被当作参数传递到一个方法后，此方法可改变这个对象的属性，并可返回变化后的结果，那么这里到底是值传递还是引用传递？

是值传递。Java 语言的方法调用只支持参数的值传递。当一个对象实例作为一个参数被传递到方法中时，参数的值就是对该对象的引用。对象的属性可以在被调用过程中被改变，但对对象引用的改变是不会影响到调用者的。C++和 C#中可以通过传引用或传输出参数来改变传入的参数的值。在 C#中可以编写如下所示的代码，但是在 Java 中却做不到。

#### 16、请说出作用域 public，private，protected，以及不写时的区别

| 作用域    | 当前类 | 同一包 | 子孙类 | 其他包 |
| --------- | ------ | ------ | ------ | ------ |
| public    | 1      | 1      | 1      | 1      |
| protected | 1      | 1      | 1      |        |
| friendly  | 1      | 1      |        |        |
| private   | 1      |        |        |        |

#### 17、Overload 和 Override 的区别。Overloaded 的方法是否可以改变返回值的类型?

- Overload：重载，表示同一个类中可以有多个名称相同的方法，但这些方法的参数列表各不相
    同（即参数个数或类型不同），与返回值无关。
    - 用反证法举例：比如有两个方法重名且参数列表都相同，一个有返回值，一个没有。那么我们调用这个方法但不用任何变量接受，Java 无法确定我们是想调用哪个方法。
- Override：重写，表示子类中的方法可以与父类中的某个方法的名称和参数完全相同，通过子类创建的实例对象调用这个方法时，将调用子类中的定义方法，这相当于把父类中定义的那个完全相同的方法给覆盖了，这也是面向对象编程的多态性的一种表现。
    - 子类覆盖父类的方法时，只能比父类抛出更少的异常。
    - 子类的访问权限必须比父类的访问权限更大。

#### 18、比较一下 Java 和 JavaScript

- java主要在服务端运行，javascript主要运行在客户端浏览器中 。
- java语言是动态语言，需要先编译再运行；javascript 是脚本语言，在程序运行中被逐行地解释 。
- java采用强类型变量检查，即所有变量在编译前必须作声明， javascript语言采用弱类型，即在使用前不需要声明，而是浏览器解释器在运行时检查数据类型（console.log(typeof a);中的typeof在浏览器的控制台返回数据类型） 。
- java语言依赖于jdk编程，javascript语言依赖于浏览器，与操作系统无关 。

#### 19、什么时候用断言？

断言检查通常在开发和测试时开启。为了保证程序的执行效率，在软件发布后断言检查通常是关闭的。断言是一个包含布尔表达式的语句，在执行这个语句时假定该表达式为 true；如果表达式的值为 false，那么系统会报告一个 AssertionError。断言的使用如下面的代码所示：

```java
assert(a > 0); // throws an AssertionError if a <= 0
```

断言可以有两种形式：

- assert Expression1;
- assert Expression1 : Expression2 ;

Expression1 应该总是产生一个布尔值，Expression2 可以是得出一个值的任意表达式；这个值用于生成显示更多调试信息的字符串消息。

#### 20、列出自己常用的 JDK 包

- java.lang：这个是系统的基础类，比如 String 等都是这里面的，这个 package 是唯一一个可以不用 import 就可以使用的 Package。
- java.io:这里面是所有输入输出有关的类，比如文件操作等
- java.net: 这里面是与网络有关的类，比如 URL,URLConnection 等。
- java.util :这个是系统辅助类，特别是集合类 Collection,List,Map 等。
- java.sql:这个是数据库操作的类，Connection, Statememt，ResultSet 等 。

### 二、面向对象

#### 1、面向过程与面向对象

- 面向过程就是在我们解决实际问题的时候，首先先考虑清楚解决这个问题的具体步骤，然后按照顺序依次执行即可。传统的面向过程的编程思想总结起来就八个字--**自顶而下，逐步细化**！举个例子：学生到校报到注册。


- 面向对象就是在编程的时候尽可能的去模拟真实的现实世界，按照现实世界中的逻辑去处理一个问题，分析问题中参与其中的有**哪些实体**，这些实体应该**有什么属性和方法**，我们**如何通过调用这些实体的属性和方法**去解决问题。现实世界中，任何一个操作或者是业务逻辑的实现都需要一个实体来完成，也就是说，**实体就是动作的支配者**，没有实体，就肯定没有动作发生！

**区别：**

1.  面向对象的思维方式更加贴近于现实生活，更容易解决大型的复杂的业务逻辑。 
2.  从前期开发角度上来看，面对对象远比面向过程要复杂，但是从维护和扩展功能的角度上来看，面对对象远比面向过程要简单！ 
3.  面对对象的模块化更深，数据更封闭，也更安全！因为面向对象的封装性更强！

#### 2、面向对象的四大特征

**1、封装**

所谓封装，也就是把客观事物封装成抽象的类，并且类可以把自己的数据和方法只让可信的类或者对象操作，对不可信的进行信息隐藏。简而言之就是，内部操作对外部而言不可见(保护性)。 

举例：张三是一个人，他就是个对象，有姓名、年龄这些属性。这些属性要有自己提供的获取或改变的方法来改变，提供 private setName 可以修改姓名。

**2、继承**

 继承是指这样一种能力：它可以使用现有类的所有功能，并在无需重新编写原来的类的情况下对这些功能进行扩展。，或者对原有类的功能进行修改。

举例：遗产的继承。

**3、多态**

 多态是指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编译时并不确定，二十在程序运行期间才确定，即一个引用变量到底会指向哪个类的实例对象，该引用变量发出的方法调用到底是哪个类中实现的方法，必须由程序运行期间才决定。

总结：允许将子类类型的指针赋值给父类类型的指针， 把不同的子类对象都当作父类来看。比如你家有亲属结婚了，让你们家派个人来参加婚礼，邀请函写的是让你爸来，但是实际上你去了，或者你妹妹去了，这都是可以的，因为你们代表的是你爸，但是在你们去之前他们也不知道谁会去，只知道是你们家的人。可能是你爸爸，可能是你们家的其他人代表你爸参加。这就是多态。

多态又分为 编译时多态和运行时多态。

- 编译时多态：比如重载
- 运行时多态：比如重写 

**4、抽象**

 抽象就是找出一些事物的相似和共性之处，然后将这些事物归为一个类，这个类只考虑这些事物的相似和共性之处，并且会忽略与当前主题和目标无关的那些方面，将注意力集中在与当前目标有关的方面。 

举例：柴犬和哈士奇都是狗，都要吃饭，玩耍，睡觉，就可以将其抽象出一个 Dog 类。

#### 3、构造器能否被重写

构造器不能被继承，因此不能被重写，但可以被重载。

#### 4、抽象类（abstract class）和接口（interface）有什么异同？

- 相同点：抽象类和接口都不能够实例化，但是可以定义抽象类和接口的引用。。一个类如果继承了某个抽象类或者实现了某个接口都需要对其中的抽象方法全部进行实现，否则该类仍然需要被声明为抽象类。
- 不同点：从实现、构造函数、实现数量、访问修饰符来看。
    - 抽象类中可以有构造器，可以有抽象方法和具体方法，而接口中不能定义构造器而且其中的方法全部都是抽象方法。（现在接口可以定义默认、静态、私有方法）
    - 抽象类中的成员可以是 private、默认、protected、public 的，而接口中的成员全都是 public 的。
    - 抽象类中可以定义成员变量，而接口中定义的成员变量实际上都是常量。有抽象方法的类必须被声明为抽象类，而抽象类未必要有抽象方法。

#### 5、静态内部类和内部类的不同？

静态内部类与非静态内部类之间存在一个最大的区别，我们知道非静态内部类在编译完成之后会隐含地保存着一个引用，该引用是指向创建它的外围类，但是静态内部类却没有。没有这个引用就意味着： 

1.  静态内部类的创建是不需要依赖于外部类，可以直接创建 ；
2.  **静态内部类不可以使用任何外部类的非 static 成员变量和方法，而内部类则都可以 。**
3.  成员内部类不能有任何 static 变量和方法，创建依赖于外部类。
4.  一个内部类对象直接可以访问创建它的外部类对象的成员，包括私有成员。外部类可以通过内部类引用间接访问内部类元素。

**方法内部类**：

- 局部内类不允许使用访问权限修饰符 public private protected 均不允许 ；
- 局部内部类对外完全隐藏，除了创建这个类的方法可以访问它其他的地方是不允许访问的。 
- 局部内部类要想使用方法形参，该形参必须用final声明(JDK8形参变为隐式final声明)；

**匿名内部类：**

- 匿名内部类是没有访问修饰符的；
- 匿名内部类必须继承一个抽象类或者实现一个接口；

- 匿名内部类中不能存在任何静态成员或方法，也不能有构造方法，因为它没有类名；

- 与局部内部相同匿名内部类也可以引用方法形参。此形参也必须声明为 final

**下面的代码哪些地方会产生编译错误？**

```java
class Outer {
    class Inner {}
    public static void foo() { new Inner(); }
    public void bar() { new Inner(); }
    public static void main(String[] args) {
        new Inner();
    }
}
```

Java 中非静态内部类对象的创建要依赖其外部类对象，上面的中 foo和 main 方法都是静态方法，静态方法中没有 this，也就是说没有所谓的外部类对象，因此无法创建内部类对象，如果要在静态方法中创建内部类对象，可以这样做：

```java
new Outer().new Inner();
```

#### 6、抽象的（abstract）方法是否可同时是静态的（static）,是否可同时是本地方法（native），是否可同时被 synchronized修饰？

都不能。抽象方法需要子类重写，而静态的方法是无法被重写的，因此二者是矛盾的。本地方法是由本地代码（如 C 代码）实现的方法，而抽象方法是没有实现的，也是矛盾的。synchronized 和方法的实现细节有关，抽象方法不涉及实现细节，因此也是相互矛盾的。

#### 7、使用 final 关键字修饰一个变量时，是引用不能变，还是引用的对象不能变？

使用 final 关键字修饰一个变量时，是指引用变量不能变，引用变量所指向的对象中的内容还是可以改变的。比如：

```java
final StringBuffer a = new StringBuffer("immutable");
```

执行如下语句将报告编译期错误：a=new StringBuffer("");

但是在该方法内部仍然可以增加如下代码来修改参数对象：`a.append("hello")`

#### 8、静态变量和实例变量的区别？

- 语法区别：静态变量前要加 static，实例变量不需要；
- 运行时区别：实例变量属于某个对象的属性，必须创建了实例对象，其中的实例变量才会被分配空间，才能使用这个实例变量。静态变量不属于某个实例对象，而是属于类，所以也称为类变量，只要程序加载了类的字节码，不用创建任何实例对象，静态变量就会被分配空间，静态变量就可以被使用了。总之，实例变量必须创建对象后才可以通过这个对象来使用，静态变量则可以直接使用类名来引用。

#### 9、是否可从一个 static 方法内发出对非 static 方法的调用？

不可以。因为非 static 方法是要与对象关联在一起的，必须创建一个对象后，才可以在该对象上进行方法调用，而 static 方法调用时不需要创建对象，可以直接调用。也就是说，当一个 static 方法被调用时，可能还没有创建任何实例对象，如果从一个 static 方法中发出对非static 方法的调用，那个非 static 方法是关联到哪个对象上的呢？这个逻辑无法成立，所以，一个 static 方法内部发出对非 static 方法的调用。

#### 10、Anonymous Inner Class(匿名内部类)是否可以继承其它类？是否可以实现接口？

可以继承其他类或实现其他接口，在 Swing 编程和 Android 开发中常用此方式来实现事件监听和回调。

#### 11、Java 中的 final 关键字有哪些用法？

在 Java 中 final 被称为终结器，可以用来修饰类、方法、属性。

- 使用 final 修改的类不能被继承，比如 String 类。
- 使用 final 修饰的方法不能被字类重写。
    - 一个类被 final 修饰后，所有的方法默认都会加上 final。

- 使用 final 修饰的变量就成为了常量，必须在声明时赋值(全用大写字母)，且不能修改。

    -  **final**成员变量必须在声明的时候初始化或者在构造器中初始化，否则就会报编译错误。

#### 12、指出下面程序的运行结果

```java
class A {
    static {
        System.out.print("1");
    }
    public A() {
        System.out.print("2");
    }
}
class B extends A{
    static {
        System.out.print("a");
    }
    public B() {
        System.out.print("b");
    }
}
public class Hello {
    public static void main(String[] args) {
        A ab = new B();
        ab = new B();
    }
}
```

执行结果：1a2b2b。创建对象时构造器的调用顺序是：先初始化静态成员，然后o调用父类构造器，再初始化非静态成员，最后调用自身构造器。



### 三、常用类

#### 1、String 和 StringBuilder、StringBuffer 的区别？

Java 平台提供了两种类型的字符串：String 和 StringBuffer/StringBuilder，它们可以储存和操作字符串。其中 String 是只读字符串，也就意味着 String 引用的字符串内容是不能被改变的。而 StringBuffer/StringBuilder 类表示的字符串对象可以直接进行修改。StringBuilder 是 Java 5 中引入的，它和 StringBuffer 的方法完全相同，区别在于它是在单线程环境下使用的，因为它的所有方面都没有被 synchronized 修饰，因此它的效率也比 StringBuffer 要高。

**1、什么情况下用+运算符进行字符串连接比调用？**

StringBuffer/StringBuilder 对象的 append 方法连接字符串性能更好。

**2、请说出下面程序的输出。**

```java
class StringEqualTest {
    public static void main(String[] args) {
        String s1 = "Programming";
        String s2 = new String("Programming");
        String s3 = "Program";
        String s4 = "ming";
        String s5 = "Program" + "ming";
        String s6 = s3 + s4;
        System.out.println(s1 == s2); //false
        System.out.println(s1 == s5); //true
        System.out.println(s1 == s6); //false
        System.out.println(s1 == s6.intern()); //true
        System.out.println(s2 == s2.intern()); //false
    }
}
```

1. String 对象的 intern 方法会得到字符串对象在常量池中对应的版本的引用（如果常量池中有一个字符串与 String 对象的 equals 结果是 true），如果常量池中没有对应的字符串，则该字符串将被添加到常量池中，然后返回常量池中字符串的引用；
2. 字符串的+操作其本质是创建了 StringBuilder 对象进行 append 操作，然后将拼接后的 StringBuilder 对象用toString 方法处理成 String 对象，这一点可以用 javap -c StringEqualTest.class命令获得 class 文件对应的 JVM 字节码指令就可以看出来。

#### 2、Math.round(11.5)等於多少? Math.round(-11.5)等於多少?

Math 类中提供了三个与取整有关的方法：ceil、floor、round。

- ceil 表示向上取整，比如 Math.ceil(11.5) 结果为 12；
- floor 表示向下取整，比如 Math.floor(11.5) 结果为 11；
- round 表示四舍五入，比如 Math.foor(-11.5) 结果为 -11。

#### 3、Integer 与 int 的区别

int 是 Java 提供的 8 种基本数据类型之一，Integer 是 Java 为该类型提供的封装类。int 的默认值为 0，而 Integer 的默认值为 null，即 Integer 可以区分出未赋值和值为 0 的区别，int 则无法表达出未赋值的情况。另外，Integer 提供了多个与整数相关的操作方法，例如，将一个字符串转换成整数，Integer 中还定义了表示整数的最大值和最小值的常量。

扩展：

- 字符串与基本类型转换：
    - `int num = Integer.parseInt("123") ;`
    - `Integer.valueOf("123").intValue();`
- 将基本类型变为字符串：
    - `String str = String.valueOf(100) ;`
    - `String str2 =  100 + "";`
- 自动装箱：`Integer i = 55 ;`
- 自动拆箱：`i += 200; // i = i + 200;` i + 200 自动拆箱；i = i + 200; 是自动装箱

#### 4、String s = new String(“xyz”);创建了几个字符串对象？

两个对象，一个是静态区的”xyz”，一个是用 new 创建在堆上的对象。

#### 5、如何实现字符串的反转及替换？

可以自己写实现也可以使用 String 或 StringBuffer/StringBuilder 中 的方法。有一道很常见的面试题是用递归实现字符串反转，代码如下所示：

```java
//1.使用reverse方法
StringBuffer str = new StringBuffer("hello");
StringBuffer str2 = str.reverse();
System.out.println(str2.toString());

//2.使用递归
public static String reverse(String str) {
    if (str == null || str.length() <= 1) {
        return str;
    }
    return reverse(str.substring(1)) + str.charAt(0);
}
```

#### 6、怎样将 GB2312 编码的字符串转换为 ISO-8859-1 编码的字符串？

```java
public static void main(String[] args) throws UnsupportedEncodingException {
    String s1 = "你好";
    String s2 = new String(s1.getBytes("GB2312"), "ISO-8859-1");
    System.out.println(s2); //ÄãºÃ
}
```

#### 7、日期和时间

- 如何取得年月日、小时分钟秒？
- 如何取得从 1970 年 1 月 1 日 0 时 0 分 0 秒到现在的毫秒数？
- 如何取得某月的最后一天？
- 如何格式化日期？

问题一：创建 java.util.Calendar 实例，调用其 get()方法传入不同的参数即可获得参数所对应的值。Java 8 中可以使用java.time.LocalDateTimel 来获取，代码如下所示

```java
public static void main(String[] args) {
    Calendar cal = Calendar.getInstance();
    int year = cal.get(Calendar.YEAR);
    int month = cal.get(Calendar.MONTH);
    int date = cal.get(Calendar.DATE);
    int hour = cal.get(Calendar.HOUR);
    int minute = cal.get(Calendar.MINUTE);
    int second = cal.get(Calendar.SECOND);
    System.out.printf("%d-%d-%d %d:%d:%d",year,month,date,hour,minute,second); //2020-1-4 3:16:38
}

public static void main(String[] args) {
    LocalDateTime dt = LocalDateTime.now();
    System.out.println(dt.getYear()); //2020
    System.out.println(dt.getMonthValue()); //2
    System.out.println(dt.getDayOfMonth()); //4
}
```

问题二：以下方法均可获得该毫秒数。

```java
public static void main(String[] args) {
    System.out.println(Calendar.getInstance().getTimeInMillis());
    System.out.println(System.currentTimeMillis());
    System.out.println(Clock.systemDefaultZone().millis());
}
```

问题三：代码如下所示：

```java
public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入年：");
    System.out.println("请输入月：");
    int year = sc.nextInt();
    int month = sc.nextInt();

    //设置日历对象的年、月、日
    Calendar c = Calendar.getInstance();
    c.set(year, month,1); //月数下标是从0开始，此处获取的是下一个月的第一天
    c.add(Calendar.DATE, -1);

    //获取这一天
    int date = c.get(Calendar.DATE);
    System.out.println(year + "年" + month + "月的最后一天：" + date);
}
```

问题四：利用 java.text.DataFormat 的子类（如 SimpleDateFormat 类）中的 format(Date)方法可将日期格式化。Java 8 中可以用 java.time.format.DateTimeFormatter 来格式化时间日期，代码如下所示。

```java
public static void main(String[] args) throws ParseException {
    DateFormat sdf = new SimpleDateFormat("yyyy/MM/dd");
    Date date = new Date();
    //格式化
    System.out.println(sdf.format(date)); //2020/02/04
    //解析
    String s = sdf.format(date);
    Date date2 = sdf.parse(s);
    System.out.println(date2); //Tue Feb 04 00:00:00 CST 2020
}


public static void main(String[] args) {
    DateTimeFormatter newFormatter = DateTimeFormatter.ofPattern("yyyy/MM/dd");
    LocalDate date2 = LocalDate.now();
    System.out.println(date2.format(newFormatter));
}
```

#### 8、打印昨天的当前时刻

```java
public static void main(String[] args) throws ParseException {
    Calendar cal = Calendar.getInstance();
    cal.add(Calendar.DATE, -1);
    System.out.println(cal.getTime()); //Mon Feb 03 15:32:58 CST 2020
}

public static void main(String[] args) {
    LocalDateTime today = LocalDateTime.now();
    LocalDateTime yesterday = today.minusDays(1);
    System.out.println(yesterday);
}
```

#### 9、String 类的常用方法

- indexOf()：返回指定字符的索引。
- charAt()：返回指定索引处的字符。
- replace()：字符串替换。
- getBytes()：返回字符串的 byte 类型数组。
- substring()：截取字符串。
- split()：分割字符串。

### 四、泛型、枚举、注解

#### 1、你是怎么理解 java 中的泛型的？

泛型是 JDK1.5 的新特性，本质上是参数化类型，也就是说所操作的数据类型被指定为一个参数。这种参数类型可以用在类、接口和方法的创建中，分别称为泛型类、泛型接口、泛型方法。在 JDK1.5之前，没有泛型的情况的下，通过对类型Object的引用来实现参数的“任意化”，“任意化”带来的缺点是要做显式的强制类型转换，而这种转换是要求开发者对实际参数类型可以预知的情况下进行的。对于强制类型转换错误的情况，编译器可能不提示错误，在运行的时候才出现异常，这是一个安全隐患。

泛型的好处是**在编译的时候检查类型安全，并且所有的强制转换都是自动和隐式的，提高代码的重用率。**

### 五、异常

#### 1、Error 和 Exception 有什么区别？

- Error 表示系统级的错误和程序不必处理的异常，是恢复不是不可能但很困难的情况下的一种严重问题；比如内存溢出，不可能指望程序能处理这样的情况；

- Exception 表示需要捕捉或者需要程序进行处理的异常，是一种设计或实现问题；也就是说，它表示如果程序运行正常，从不会发生的情况。

#### 2、try{} 里有一个 return 语句，那么紧跟在这个 try 后的 finally{}里的代码会不会被执行，什么时候被执行，在 return 前还是后?

会执行，在方法返回调用者前执行。

在 finally 中改变返回值的做法是不好的，因为如果存在 finally 代码块，try 中的 return 语句不会立马返回调用者，而是记录下返回值待 finally 代码块执行完毕之后再向调用者返回其值，然后如果在 finally 中修改了返回值，就会返回修改后的值。

#### 3、Java 语言如何进行异常处理，关键字：throws、throw、try、catch、finally 分别如何使用？

在 Java 中，每个异常都是一个对象，它是 Throwable 类或其子类的实例。当一个方法出现异常后便抛出一个异常对象，该对象中包含有异常信息，调用这个对象的方法可以捕获到这个异常并可以对其进行处理。

Java 的异常处理是通过 5 个关键词来实现的：try、catch、throw、throws 和 finally。一般情况下是用 try 来执行一段程序，如果系统会抛出（throw）一个异常对象，可以通过它的类型来捕获（catch）它，或通过总是执行代码块（finally）来处理；try 用来指定一块预防所有异常的程序；catch 子句紧跟在 try 块后面，用来指定你想要捕获的异常的类型；throw 语句用来明确地抛出一个异常；throws 用来声明一个方法可能抛出的各种异常（当然声明异常时允许无病呻吟）；finally 为确保一段代码不管发生什么异常状况都要被执行；try 语句可以嵌套，每当遇到一个 try 语句，异常的结构就会被放入异常栈中，直到所有的 try 语句都完成。如果下一级的try 语句没有对某种异常进行处理，异常栈就会执行出栈操作，直到遇到有处理这种异常的 try 语句或者最终将异常抛给 JVM。

#### 4、运行时异常与受检异常有何异同

运行时异常：编译能通过，但是一运行就终止了，程序不会处理运行时异常，是一种常见运行错误，只要程序设计得没有问题通常就不会发生，比如：IllegalArgumentException。

而受检查的异常，跟程序运行的上下文环境有关，即使程序设计无误，仍然可能因使用的问题而引发。要么用try。。。catch捕获，要么用throws字句声明抛出，交给它的父类处理，否则编译不会通过。

#### 5、列出一些你常见的运行时异常？

1. ClassCastException （类转换异常）
2. IllegalArgumentException （非法参数异常）
3. IndexOutOfBoundsException （下标越界异常）
4. NullPointerException （空指针异常）
5. SecurityException （安全异常）
6. ArithmeticException（算术异常）

#### 6、阐述 final、finally、finalize 的区别。

finally：通常放在 try…catch…的后面构造总是执行代码块，这就意味着程序无论正常执行还是发生异常，这里的代码只要 JVM 不关闭都能执行，可以将释放外部资源的代码写在 finally 块中。

finalize：Object 类中定义的方法，Java 中允许使用 finalize() 方法在垃圾收集器将对象从内存中清除出去之前做必要的清理工作。这个方法是由垃圾收集器在销毁对象时调用的，通过重写 finalize()方法可以整理系统资源或者执行其他清理工作。

#### 7、类 ExampleA 继承 Exception，类 ExampleB 继承ExampleA，输出下列代码执行结果

```java
try {
    throw new ExampleB("b")
} catch（ExampleA e）{
    System.out.println("ExampleA");
} catch（Exception e）{
    System.out.println("Exception");
}
```

输出：ExampleA。（根据里氏代换原则[能使用父类型的地方一定能使用子类型]，抓取 ExampleA 类型异常的 catch 块能够抓住 try 块中抛出的 ExampleB 类型的异常）

#### 8、throw throws Throwable 关联于区别？

- throw 是在方法中抛出一个异常，不能单独使用 。
- throws 是在方法定义处或者类定义处声明该类或方法可能产生的异常，（一个或者多个）.----也可以理解为将方法内部异常不处理直接抛出，让调用者去处理。
- throwable 是所有错误和异常的超类，所以当不知道要产生的异常是什么类型时候，直接 throws Throwable 即可

### 六、Java8、9 新特性