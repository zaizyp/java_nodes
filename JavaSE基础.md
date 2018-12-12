# JavaSE基础
## 一、面向对象
### 1、面向对象的特性
1)、继承：继承是从已有类得到继承信息创建新类的过程。提供继承信息的类被称为父类（超类、基类）；得到继承信息的类被称为子类（派生类）。继承让变化中的软件系统有了一定的延续性，同时继承也是封装程序中可变因素的重要手段。  
2)、封装：通常认为封装是把数据和操作数据的方法绑定起来，对数据的访问只能通过已定义的接口。面向对象的本质就是将现实世界描绘成一系列完全自治、封闭的对象。我们在类中编写的方法就是对实现细节的一种封装；我们编写一个类就是对数据和数据操作的封装。可以说，封装就是隐藏一切可隐藏的东西，只向外界提供最简单的编程接口。  
3)、多态性：多态性指允许不同子类型的对象对同一消息作出不同的响应。简单的说就是用同样的对象引用调用同样的方法但是做了不同的事情。多态性分为编译时的多态性和运行时的多态性。如果将对象的方法视为对象向外界提供的服务，那么运行时的多态性可以解释为：当A系统访问B系统提供服务时，B系统有多种提供服务的方式，但一切对A系统来说都是透明的。方法重载（overload）实现的是编译时的多态性（也称为前绑定），而方法重写（override）实现的是运行时的多态性（也称为后绑定）。运行时的多态性是面向对象最精髓的东西，要实现多态需要做两件事：1、方法重写（子类继承父类并重写父类中已有的或者抽象的方法）；2、对象造型（用父类型引用子类型对象，这样同样的引用调用同样的方法就会根据子类对象的不同而表现出不同的行为）。
4)、抽象：抽象是将一类对象的共同特征总结出来构造类的过程，包括数据抽象和行为抽象两方面。抽象只关注对象有哪些属性和行为，并不关注这些行为的细节是什么。
### 2、访问修饰符
|修饰符|当前类|同包|子类|不同包|
|-----|-----|----|----|-----|
|public| ✔ | ✔  | ✔ |  ✔  |
|protected|✔| ✔ | ✔ |  ✖  |
|default|✔ | ✔  | ✖ |  ✖  |
|private|✔ | ✖  | ✖ |  ✖  |
### 3、clone对象
#### 3.1、为什么要用clone？
　　在实际编程过程中，我们常常要遇到这种情况：有一个对象A，在某一时刻A中已经包含了一些有效值，此时可能会需要一个和A完全相同的新对象B，并且此后对B任何改动都不会影响带A中的值，也就是说，A与B是两个独立的对象，但B的初始值是由A对象确定的。在Java语言中，用简单的赋值语句是不能满足这种需求的，要满足这种需求有很多种途径，但实现clone()方法时其中最简单，也是最高效的手段。
#### 3.2、new一个对象的过程和clone一个对象的过程的区别
　　new操作符的本意是分配内存。程序执行到new操作符时，首先去看new操作符后面的类型，因为知道了类型，才能知道要分配多大的内存空间。分配完内存之后，再调用构造函数，填充对象的各个域，这一步叫做对象的初始化，构造方法返回后，一个对象创建完毕，可以把他的引用（地址）发布到外部，在外部就可以使用这个引用操纵这个对象。  
　　clone在第一步是和new相似，都是分配内存，调用clone方法时，分配的内存和原对象（即调用clone方法的对象）相同，然后再使用原对象中对应的各个域，填充新对象的域，填充完成后，clone方法返回，一个新的相同的对象被创建，同样可以把这个新对象的引用发布到外部。
## 二、JavaSE语法
### 1、Java有没有goto语句？
　　goto是Java中的保留关键字，在目前版本的Java中没有使用。根据James Gosling（Java之父）编写的《The Java Programming Language》一书的附录中给出了一个Java关键字列表，其中有共同和const，但这两个是目前无法使用的关键字，因此有些地方将其称为保留字，其实保留字这个词应该有更广泛的意义，因为熟悉C语言的程序员都知道，在系统类库中使用过的有特殊意义的单词或单词的组合都被视为保留字。
### 2、&和&&的区别
　　&运算符有两种用法：（1）按位与；（2）逻辑与。  
　　&&运算符是短路运算符，逻辑与跟短路与的差别是非常巨大的，虽然二者都要求运算符左右两端的布尔值都是true整个表达式的值才是true。  
　　&&之所以被称为短路与是因为，如果&&左边的表达式的值时false，幼斌的表达式会被直接短路掉，不会进行运算。很多时候我们可能都需要用&&而不是&，例如在验证用户登录时判定用户名是不是null而且不是空字符串，应该写为`username != null && !username.equals("")`，二者的位置不能交换，更不能使用&运算符，因为第一个条件如果不成立，根本不能进行字符串的equals比较，否则会产生NullPointerException异常。注意：逻辑或运算符（|）和短路或运算符（||）的差别也是如此。
### 3、两个对象值相同（`x.equals(y) == true`），但却可以有不同的hashCode，这句话对不对？
　　不对，如果两个对象x和y满足`x.equals(y) == true`，它们的hashCode应当相同。  
　　java对于equals方法和hashCode方法是这样规定的：（1）如果两个对象相同（equals方法返回true），那么他们的hashCode值一定要相同；（2）如果两个对象的hashCode相同，它们不一定相同。当然，你未必要按照要求去做。
### 4、重载（overload）和重写（override）的区别？重载的方法能否根据返回类型进行区分？
　　方法的重载和重写都是实现多态的方式，区别在于前者实现的是编译时的多态性，而后者实现的是运行时的多态性。重载发生在一个类中，同名的方法如果有不同的参数列表（参数类型不同，参数个数不同或者二者都不同）则视为重载；重写发生在子类和父类之间，重写要求子类被重写方法与父类之间有相同的方法名、以及参数，而且子类重写方法的返回值类型必须为父类方法返回值类型的派生类。  
　　方法重载的规则：  
　　　　1、方法名一致，参数类表中参数的顺序、类型、个数不同。  
　　　　2、重载与方法的返回值无关，存在于父类和子类、同类中。  
　　　　3、可以抛出不同的异常，可以有不同的修饰符。  
　　方法重写的规则：  
　　　　1、参数列表必须完全与被重写方法的一致，返回类型一致或为被重写方法返回值类型的子类。  
　　　　2、构造方法不能重写，声明为final的方法不能被重写，声明为static的方法不能被重写，但是能够被再次声明。  
　　　　3、访问权限不能比父类中被重写方法的访问权更低。  
　　　　4、重写的方法能够抛出任何非强制异常（UncheckedException，也叫非运行时异常），无论被重写的方法时否抛出异常。但是重写的方法不能抛出新的强制性异常，或者比被重写方法声明的更广泛的强制性异常，反之则可以。
### 5、char型变量中能不能存储一个中文汉字，为什么？
　　　　char类型可以存储一个中文汉字，因为Java中使用的编码是Unicode（不选择任何特定的编码，直接使用字符集中的编号，这是统一的唯一方法），一个char类型占用2个字节（16比特），所以放一个中文是没问题的。  
**补充**:使用Unicode意味着字符在JVM内部和外部有着不同的表现形式，在JVM内部都是Unicode，当这个字符被从JVM内部转移到外部时（例如存入文件系统中），需要进行编码转换。所以Java中有字节流和字符流，以及在字节流和字符流之间进行转换的转换流，如InputStreamReader和OutStreamReader，这两个类是字节流和字符流之间的适配器类，承担了编码转换的任务；对于C程序员来说，要完成这样的编码转换恐怕要依赖于union（联合体/共用体）共享内存的特征来实现了。
### 6、抽象类（abstract class）和接口（interface）有什么异同？
不同：
　　抽象类：
　　　　1、抽象类中可以定义构造器  
　　　　2、可以有抽象方法和具体方法  
　　　　3、接口中的成员全都是public的  
　　　　4、抽象类中可以定义成员变量  
　　　　5、有抽象方法的类必须被声明为抽象类，而抽象类未必要有抽象方法  
　　　　6、抽象类中可以包含静态方法  
　　　　7、一个类只能继承一个抽象类  
　　接口：  
　　　　1、接口中不能定义构造器  
　　　　2、方法全部都是抽象方法  
　　　　3、抽象类中的成员可以是private、默认、protected、public  
　　　　4、接口中定义的成员变量只能是常量（即为final static修饰），且默认为常量  
　　　　5、接口中不能有静态方法  
　　　　6、一个类可以实现多个接口  
　　相同：  
　　　　1、不能够实例化  
　　　　2、可以将抽象类和接口类型作为引用类型  
　　　　3、一个类如果继承了某个抽象类或者实现了某个接口都需要对其中的抽查方法全部进行实现，否则该类仍需要被声明为抽象类或者接口。
### 7、静态变量和实例变量的区别？
**静态变量**：是被static修饰符修饰的变量，也称为类变量，它属于类，不属于任何一个对象，一个类不管创建多少个对象，静态变量在内存中有且仅有一个拷贝。  
**实例变量**：必须依赖于某一实例，需要先创建对象然后通过对象才能访问到它。静态变量可以实现让多个对象共存。
### 8、==和equals的区别？
equals和==最大的区别是一个是方法一个是运算符。  
==：如果比较的是基本数据类型，则比较的是数值是否相等；如果比较的是引用数据类型，则比较的是对象的地址值是否相等。
equals()：用来比较两个对象的内容是否相等。  
注意：equals方法不能用于基础数据类型的变量，如果没有对equals方法进行重写，则比较的是引用类型的变量所指的对象的地址。
### 9、`String s = "Hello";s = s + "world!";`这两行代码执行后，原始的String对象中的内容到底变了没有？
没有，因为String被设计成不可变类，所以它的所有对象都是不可变对象。String底层实际上是一个final修饰的字符串数组。
## 三、Java中的多态
### 1、Java中实现多态的机制是什么？
　　靠的是父类或接口定义的引用变量可以指向子类或具体实现类的实例对象，而程序调用的方法在运行期才动态绑定，就是引用变量所指向的具体实例对象的方法，也就是内存里正在运行的那个对象的方法，而不是引用变量的类型中定义的方法。
## 四、Java的异常处理
### 1、Java中异常分为哪些种类
　　1)按照异常需要处理的时机分为编译时异常（也叫强制性异常）也叫CheckedException和运行时异常（也叫非强制性异常）也叫RuntimeException。只有Java语言提供了CheckedException，Java认为CheckedException都是可以被处理的异常，所以Java程序必须显式处理CheckedException。如果程序没有处理CheckedException，该程序在编译时就会发生错误而无法编译。这体现了Java的设计哲学：没有完善错误处理的代码根本没有机会被执行。对CheckedException处理有两种方法：  
　　　　1、当前方法知道该如何处理该异常，则用try...catch块来处理该异常。  
　　　　2、当前方法不知道该如何处理，则在定义该方法时声明抛出该异常。  
　　2）运行时异常只有当代码在运行时才发生的异常，编译时不需要try...catch。Runtime如除数是0和数组下标越界等，其产生频繁，处理麻烦，若显示声明或捕获将会对程序的可读性和运行效率影响很大。所以由系统自动检测并将它们交给缺省的异常处理程序。当然如果你有处理要求也可以显示地捕获它们。
### 2、调用下面的方法，得到的返回值是什么？
```java
public int getNum(){
    try {
      int a = 1/0;
      return 1;
    } catch (Exception e) {
      return 2;
    } finally {
      return 3;
    }
}
```
返回值为3
### 3、error和exception的区别？
　　Error类和Exception类都是继承与Throwable类，它们的区别如下：  
　　Error类一般指与虚拟机相关的问题，如系统崩溃，虚拟机错误，内存空间不足，方法调用栈溢出等，对于这类错误导致的应用程序中断，仅靠程序本身无法恢复和预防，遇到这样的错误，建议让程序终止  
　　Exception类表示程序可以处理的异常，可以捕获且可能恢复。遇到这类异常，应该尽可能处理异常，使程序恢复运行，而不应该随意终止异常  
　　Exception类又分为运行时异常（Runtime Exception）和受检查的异常（Checked Exception），运行时异常：ArithmaticException，IllegalArgumentException，编译能通过，但是一运行就终止了，程序不会处理运行时异常，出现这类异常，程序会终止。而受检查的异常，要么用try...catch捕获，要么用throws语句声明抛出，交给调用者处理，否则编译不会通过。
### 4、Java异常处理机制
　　Java对异常进行了分类，不同类型的异常分别用不用的Java类表示，所有异常的基类为java.lang.Throwable，Throwable下面又派生了两个子类：Error和Exception，Error表示应用程序本身无法克服和恢复的一种严重问题，Exception便是程序还能够克服和恢复的问题，其中又分为系统异常和普通异常，系统异常是软件本身缺陷所导致的问题，也就是软件开发人员考虑不周所导致的问题，软件使用者无法克服和恢复这种问题，但在这种问题下还可以让软件系统继续运行或者让软件死掉，例如，数组角标越界（ArrayIndexOfBoundsException），空指针异常（NullPointerException），类转换异常（ClassCastException）；普通异常就是运行环境变化或异常所导致的问题，是用户能够克服的问题，例如，网络断线，硬盘空间不足，发生这样的异常后，程序不应该死掉  
　　Java为系统异常和普通异常提供了不同的解决方案，编译器强制普通异常必须try...catch处理或者用throws声明继续抛给上层调用方法处理，所以普通异常也称为Checked异常，而系统异常可以处理也可以不处理，所以，编译器不强制用try...catch处理或用throws声明，所以系统异常也称为UnChecked异常。
### 5、写出最常见的5个RuntimeException
1) java.lang.NullPointerException 空指针异常；出现原因：调用了未经初始化的对象或者不存在的对象  
2) java.lang.ClassNotFoundException 指定的类找不到异常；出现原因：类的名称或者路径加载错误；通常都是程序试图通过字符串来加载某各类时可能引起的异常  
3) java.lang.NumberFormatException 字符串转换为数字异常；出现原因：字符串数据中包含非数字型字符  
4) java.lang.ArrayIndexOfBoundsException 数组角标越界异常；出现原因：操作数组时角标大于数组的实际长度  
5) java.lang.IllegalArgumentException 方法传递参数错误  
6) java.lang.ClassCastException 数据类型转换异常  
7) java.lang.NoClassDefFoundException 未找到类定义异常  
8) java.lang.InstantiationException 实例化异常  
9) java.lang.NoSuchMethodException 方法不存在异常  
10) SQLException SQL异常，常见于操作数据库时的SQL语句错误  
### 6、throw和throws的区别
**throw**：  
　　1) throw语句用在方法体内，表示抛出异常，由方法体内的语句处理  
　　2) throw是具体向外抛出一的动作，所以它抛出的是一个异常实例，执行throw一定是抛出了某种异常  
**throws**：  
　　1) throws语句是用在方法声明后面，表示如果抛出异常，由该方法的调用者来进行异常的处理  
　　2) throws主要声明这个方法会抛出某种类型的异常，然后让它的调用者知道需要捕获的异常的类型  
　　3) throws表示出现异常的一种可能性，并不一定会发生这种异常  
### 7、final、finally、finalize的区别
　　1) final：用于声明属性、方法和类，分别表示属性不可变、方法不可覆盖和被其修饰的类不可继承
　　2) finally：异常处理语句结构的一部分，表示总是执行
　　3) finalize：Object类的一个方法，在垃圾回收器执行的时候会调用被回收对象的此方法，可以覆盖此方法提供垃圾收集时的其它资源回收，例如关闭文件等。该方法更像是一个对生命周期的临终方法，当该方法被系统调用则代表该对象即将“死亡”，但需要注意的是，我们主动行为上去调用该方法并不会导致该对象“死亡”，这是一个被动得分（其实就是回调方法），不需要我们调用。
## 五、JavaSE常用API
### 1、`Math.round(11.5)`等于多少？`Math.round(-15)`又等于多少
　　`Math.round(11.5)`的返回值是12，`Math.round(-11.5)`的返回值是-11.四舍五入的原理是在参数上加上0.5然后进行取整
### 2、switch是否能作用在byte上，是否能作用在long上，是否能作用在String上？
　　在Java5以前stitch(expr)中，expr只能是byte、short、char、int。从Java5开始，Java中引入了枚举类型，expr也可以是enum类型
### 3、数组有没有length()方法？String有没有length()方法？
　　数组没有length()方法，而是又length属性。String有length()方法。在JavaScript中，获得字符串的长度时通过length属性得到的，这一点容易和Java混淆
### 4、String、StringBuilder、StringBuffer的区别？
　　Java平台提供了两种类型的字符串：String和StringBuilder/StringBuffer，它们都可以存储和操作字符串，区别如下：  
　　1) String是只读字符串，也就意味着String引用的字符串内容是不能被改变的。初学者可能会有这样的误解：  
    ```java
    String str = "abc";
    str = "bcd";
    ```
　　如上，字符串str明明是可以改变的呀！其实不然，str仅仅是一个引用对象，它指向一个字符串对象"abc"。第二行代码的含义是让str重新指向了一个新的字符串"bcd"对象，而"abc"对象并没有任何改变，只不过该对象已经成为一个不可及对象罢了  
    2) StringBuilder/StringBuffer表示的字符串对象可以直接进行修改。
    3) StringBuilder是Java5中引入进来的，它和StringBuffer的方法完全相同，区别在于它使在单线程环境下使用的，因为它的所有方法都没有被synchronized修饰，因此它的效率理论上也比StringBuffer要高
### 5、什么情况下用“+”运算符进行字符串链接比调用StringBuffer/StringBuilder对象的append方法链接字符串性能要好？
　　字符串是Java程序中最常用的数据结构之一。在Java中String类已经重载了“+”。也就是说，字符串可以直接使用“+”进行连接，如下面代码所示：
```java
String s = "abc" + "ddd";
```
　　但这样做真的好吗？当然，这个问题不能简单地回答yes or no。要根据具体情况来定。在Java中提供了一个StringBuilder类（这个类在J2SE5及以上版本提供，以前的版本使用Stringbuffer），这个类也可以起到“+”的作用。那么我们应该用哪个呢？
　　下面让我们先看看如下的代码：
```java
package string;

public class TestSimplePlus {
    public static void main(String[] args) {
        String s = "abc";
        String ss = "ok" + s + "xyz" + 5;
        System.out.println(ss);
    }
}
```
　　上面的代码将对输出正确的结构。从表面上看，对字符串和整型使用“+”运算符并没有上面区别，但事实真的如此吗？下面让我们来看看这段代码的本质  
　　我们首先使用反编译工具（如JDK自带的javap、jad）将TestSimplePlus反编译成Java Byte Code，其中的奥秘就一目了然了。在本文中使用jad来反编译，命令如下：  
jad -o -a -s d.java TestSimplePlus.class  
　　反编译后的代码如下：  
```java
package string;
import java.io.PrintStream;

public class TestSimplePlus {
    pulic TestSimplePlus() {
    //    0    0:aload_0
    //    1    1:invokespecial   #8    <Method void Object()>
    //    2    4:return
    }

    public static void main(String args[]) {
        String a = "abc";
        //    0   0:ldc1            #16 <String "abc">
        //    1   2:astore_1
            String ss = (new StringBuilder("ok")).append(s).append("xyz").append(5).toString;
        //    2   3:new             #18  <Class StringBuilder>
        //    3   6:dup
        //    4   7:ldc1            #20 <String "ok">
        //    5   9:invokespecial   #22 <Method void StringBuilder(String)>
        //    7   13:invokespecial  #25 <Method StringBuilder StringBuilder.append(String)>
        //    6   12:aload_1
        //    8   16:ldc1           #29 <String "xyz">
        //    9   18:invokespecial  #25 <Method StringBuilder StringBuilder.append(String)>
        //    10  21:iconst_5
        //    11  22:invokespecial  #31 <Method StringBuilder StringBuilder.append(int)>
        //    12  25:invokespecial  #34 <Method StringBuilder StringBuilder.toString()>
        //    13  28:astore_2
            System.out.println(ss);
        //    14  29:getstatic      #38 <Field PrintStream System.out>
        //    15  32:aload_2
        //    16  33:invokespecial  #44 <Method void PrintStream.println(String)>
        //    17  36:return
    }
}
```
　　读者可能看到上面的Java字节码感到迷惑，不过大家不必担心，本文的目的并不是讲解Java Byte Code，因此，并不用了解具体的字节码含义。  
　　使用jad反编译的好处之一就是可以同时生成字节码和源代码。这样可以进行对照研究。从上面的代码很容易看出，虽然在源程序中使用了“+”，但在编译时仍将“+”转换成StringBuilder。因此，我们可以得出结论，**在Java中无论以何种方式进行字符串的连接，实际上都是使用的是StringBuilder。**  
　　那么是不是可以根据这个结论推出使用“+”和StringBuilder的效果是一样的呢？这个要从两个方面解释。如果从运行结果来解释，那么“+”和StringBuilder是完全等效的。但如果从运行效率和资源消耗来看，那他们将存在很大的区别。  
　　当然，如果连接字符串表达式很简单（如上面的顺序结构），那么“+”和StringBuilder基本是一样的，但如果结构比较复杂，如使用循环来连接字符串，那么产生的Java Byte Code就会有很大的区别。先让我们看看如下的代码：
```java
import java.util.*;
public class TestComplexPlus {
    public static void main(String[] args) {
        String s = "";
        Random rand = new Random();
        for (int i = 0; i < 10; i++) {
            s = s + rand.nextInt(1000) + " ";
        }
        System.out.println(s);
    }
}
```
　　这里，编译器会将“+”转换成StringBuilder，但是创建StringBuilder对象的位置却在for语句内部。这就意味着每执行一次循环，就会创建一个StringBuilder对象（对于本例来说，是创建了10个StringBuilder对象），虽然Java有垃圾回收器，但这个回收器的工作时间是不定的。如果不断产生这样的垃圾，那么仍然会占用大量的资源。解决这个问题的方法就是在程序中直接使用StringBuilder来连接字符串，代码如下：
```java
import java.util.*;

public class TestStringBuilder {
    public static void main(String[] args) {
        String s = "";
        Random rand = new Random();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i > 10; i++) {
            sb.append(rand.nextInt(1000));
            sb.append(" ");
        }
        System.out.println(sb.toString());
    }
}
```
　　上面的代码反编译之后，创建StringBuilder的代码被放在了for语句之外。虽然这样处理在源程序中看起来复杂，但却换来了更高的效率，同时消耗的资源也更少了。  
　　在使用StringBuilder时要注意，尽量不要“+”和StringBuilder混着用，否则会创建更多的StringBuilder对象，如下面的代码：
```java
for (int i = 0; i < 10; i++) {
    sb.append(rand.nextInt(1000));
    sb.append(" ");
}
```
改成如下形式：
```java
for (int i = 0; i < 10; i++) {
    sb.append(rand.nextInt(1000) + " ");
}
```
　　从上面的代码可以看出，Java编译器将“+”编译成StringBuilder，这样for语句每循环一次，又创建了一个StringBuilder对象。
　　如果将上面的代码在JDK1.4下编译，必须将StringBuilder改为StringBuffer，而JDK1.4将“+”转换为StringBuffer（因为JDK1.4并没有提供StringBuilder类）。StringBuffer和StringBuilder的功能基本一样，只是StringBuffer是线程安全的，而StringBuilder不是线程安全的。因此，StringBuilder的效率会更高。
### 6、Java中的日期和时间
#### 6.1、如何获得年月、小时分钟秒？
```java
public class DataTimeTest {
    pulic static void main(String[] args) {
        Calendar cal = new Calender.getInstance();
        System.out.println(cal.get(Calender.YEAR));
        System.out.println(cal.get(Calender.MONTH));// 0 - 11
        System.out.println(cal.get(Calender.DATE));
        System.out.println(cal.get(Calender.HOUR_OF_DAY));
        System.out.println(cal.get(Calender.MINUTE));
        System.out.println(cal.get(Calender.SECOND));
        // Java 8
        LocalDataTime dt = LocalDataTime.now();
        System.out.println(dt.getYear());
        System.out.println(dt.getMonthValue());
        System.out.println(dt.getDayOfMonth());
        System.out.println(dt.getHour());
        System.out.println(dt.getSecond());
    }
}
```
#### 6.2、如何获得从1970年1月1日0时0分0秒到现在的毫秒数
```java
Calender cal = Calender.getInstance().getTimeInMillis();//第一种方式
System.currentTimeMillis();//第二种方式
//Java 8
Clock.systemDefaultZone().millis();
```
#### 6.3、如何格式化日期？
　　1) java.text.DataFormat的子类（如SimpleDateFormat类）中的format(Date)方法可将日期格式化。
　　2) Java8中可以用java.time.format.DateTimeFormatter来格式化时间日期，代码如下所示：
```java
import java.text.SimpleDateFormat;
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.util.Date;
class DateFormatTest {
    public static void mian(String[] args) {
        SimpleDateFormat oldFormatter = new SimpleDateFormat("yyyy/MM/dd");
        Date date1 = new Date();
        System.out.println(oldFormatter.format(date1));
        // Java 8
        DateTimeFormatter newFormatter = DateTimeFormatter.ofPattern("yyyy/MM/dd");
        LocalDate date2 = LocalDate.now();
        System.out.println(date2.format(newFormatter));
    }
}
```
　　补充:Java的时间日期API一直以来都是被诟病的东西，为了解决这一问题，Java8中引入了新的时间日期API，其中包括LocalDate、LocalTime、LocalDateTime、Clock、Instant等类，这些类的设计都使用了不变模式，因此是线程安全的设计。
#### 6.4、Java8的日期特性？


## 六、Java的数据类型
### 1、Java的基本数据类型都有哪些各占几个字节
|  类型  |字节数|    数据表示范围    |
|--------|-----|-------------------|
|byte   |1     |-128~127|
|short  |2     |-32768~32767|
|int    |4     |-2147483648~2147483647|
|long   |8     |-2*63 ~ 2*63-1|
|float  |4     |-3.403E38~3.403E38|
|double |8     |-1.798E308~1.798E308|
|char   |2     |表示一个字符，如('a','A','0','家')|
|boolean|1     |只有两个值true与false|
### 2、`short s1 = 1; s1 = s1 +1;`有错吗？`short s1 = 1; s +=1;`有错吗？
　　前者不正确，后者正确。对于`short s1 = 1; s1 = s1 +1;`由于1是int类型，因此s1+1运算结果也是int型，需要强制转换类型才能赋值给short型。而`short s1 = 1; s +=1;`可以正常编译，因为`s1 += 1;`相当于`s1 = (short)(s1 + 1);`其中有隐含的强制类型转换
### 3、数据类型之间的转换
#### 3.1、字符串如何转基本数据类型？
　　调用基本数据类型对应的包装类中的方法parseXXX(String)或valueOf(String)即可返回相应的基本类型。
#### 3.2、基本数据类型如何转字符串？
　　一种方法时将基本数据类型与空字符串("")连接(+)即可获得其对应的字符串；另一种方法时调用String类中的valueOf()方法犯规相应的字符串
## 七、Java的IO
### 1、java中有几种类型的流
　　按照流的方向分：输入流（inputStream）和输出流（outputStream）  
　　按照事先功能分：字节流（可以从或向一个特定的地方（节点）读写数据。如FileReader）和处理流（是对一个已存在的流的连接和封装，通过所封装的流的功能调用实现数据读写。如BufferReader。处理流的构造方法总是要带一个其他的流对象做参数。一个流对象经过其他流的多次包装，称为流的链接）  
　　按照处理数据的单位：字节流和字符流。字节流继承于InputStream和OutputStream，字符流继承于Reader和Writer  
### 2、字节流如何转换为字符流
字节输入流转字符输入流通过InputStreamReader实现，该类的构造函数可以传入InputStream对象  
字节输出流转字符输出流通过OutputStreamWriter实现，该类的构造函数可以传入OutputStream对象。
### 3、如何将一个java对象序列化到文件里
在Java中能够被序列化的类必须先实现Serializable接口，该接口没有任何抽象方法只是起到一个标记作用。  
```java
//对象输出流
    ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream(new File("D://obj")));
    objectOutputStream.writeObject(new User("zhangsan",100));
    objectOutputStream.close();
//对象输入流
    ObjectInputStream objectInputStream = new ObjectInputStream(new FileInputStream(new File("D://obj")));
    User user = (User)objectInputStream.readObject();
    objectInputStream.close();
```
### 4、字节流和字符流的区别
　　字节流读取的时候，读到一个字节就返回一个字节；字符流使用了字节流读到一个或多个字节（中文对应的字节是两个，在UTF-8码表中是3个字节）时，先去查指定的码表，将查到的字符返回。字节流可以处理所有类型数据，如：图片，MP3，AVI视频文件，而字符流只能处理字符数据。只要是处理纯文本数据，就要优先考虑使用字符流，除此之外都用字节流。字节流主要是操作byte类型数据，以byte数组为准，主要操作类就是OutputStream、InputStream  
　　字符流处理的单元为2个字节的Unicode字符，分别操作字符、字符数组或字符串，而字节流处理单元为1个字节，操作字节和字符数组。所以字符流是由Java虚拟机将字节转化为2个字节的Unicode字符为单位的字符而成的，所以它对多国语言支持性比较好！如果是音频文件、图片、歌曲，就用字节流好点，如果关系到中文（文本）的，用字符流好点。在程序中一个字符等于两个字节，java提供了Reader、Writer两个专门操作字符流的类。
### 5、如何实现克隆对象
　　有两种方式：  
1) 实现Cloneable接口并重写Object类中的clone()方法；  
2) 实心Serializable接口，通过对象的序列化和反序列化实现克隆，可以实现真正的深度克隆  
　　注意：基于序列化和反序列化实现的克隆不仅仅是深度克隆，更重要的是通过泛型限定，可以检查出要克隆的对象是否支持序列化，这项检测是编译器完成的，不是在运行时抛出异常，这种方案明显优于使用Object类的clone方法克隆对象。让问题在编译的时候暴露出来总是好过把问题留到运行时。
### 6、什么是Java序列化，如何实现Java序列化
　　序列化就是一种用来处理对象流的机制，所谓对象流也就是将对象的内容进行流化。可以对流化后的对象进行读写操作，也可以将流化后的对象传输于网络之间。序列化是为了解决在对象进行读写操作时引发的问题。  
　　序列化的实现：将需要被序列化的类实现 Serializable 接口，该接口没有需要实现的方法，`implments Serializable`只是为了标注该对象时可被序列化的，然后使用一个输出流（如：FileOutputStream）来构造一个ObjectOutputStream（对象流）对象，接着，使用ObjectOutputStream对象的writerObject(Object obj)方法就可以将参数为obj的对象写出（即保存其状态），要恢复的话则用输入流。
## 八、Java的集合
### 1、HashMap排序题，上机题。（这题很重要）
　　已知一个HashMap<Integer, User>集合，User有name(String)和age(int)属性。请写一个方法实现对HashMap的排序功能，该方法接收HashMap<INteger, User>为形参，返回类型为HashMap<Integer, User>，要求HashMap中的User的age倒序进行排序。排序时key=value键值对不得拆散。  
注意：要做这道题必须对集合体系结构非常的熟悉。HashMap本身就是不可排序的，但是该道题偏偏让给HashMap排序，那我们就得想在API中有没有这样的Map结构是有序的，LinkedHashMap，对的，就会它，它是Map结构，也是链表结构，有序的，更可喜的是它是HashMap的子类，我们返回LinkedHashMap<Integer,User>即可，还符合面向接口（父类编程思想）。  
　　但凡是对集合的操作，我们应该保持一个原则就是能用JDK中的API就用JDK中的API，比如排序算法我们不应该去用冒泡或者选择，而是首先想到用Collections集合工具类。
```java
//对HashMap的排序
public class HashMapTest {
	public static void main(String[] args) {
		HashMap<Integer, User> map = new HashMap<>();
		map.put(1, new User("张三", 33));
		map.put(2, new User("李四", 28));
		map.put(3, new User("王五", 45));
		map.put(4, new User("大明", 45));
		map.put(5, new User("小李", 21));
		map.put(6, new User("小红", 89));
		System.out.println(map);
		HashMap<Integer,User> mapSort = hashMapSort(map);
		System.out.println(mapSort);
	}

	public static HashMap<Integer, User> hashMapSort(HashMap<Integer, User> map) {
		Set<Entry<Integer,User>> entrySet = map.entrySet();
		List<Entry<Integer,User>> list = new ArrayList(entrySet);
		Collections.sort(list, new Comparator<Entry<Integer,User>>() {
			@Override
			public int compare(Entry<Integer, User> o1, Entry<Integer, User> o2) {
				return o2.getValue().getAge() - o1.getValue().getAge();
			}
		});
		LinkedHashMap<Integer, User> result = new LinkedHashMap<>();
		for (Entry<Integer, User> entry : list) {
			result.put(entry.getKey(), entry.getValue());
		}
		return result;
	}
}
```
### 2、集合的安全性问题
　　请问ArrayList、HashSet、HashMap是线程安全的吗？如果不是我想要线程安全的集合怎么办？   
　　我们都看过上面那些集合的源码，每个方法否没有加锁，显然都不是线程安全的。话又说过来如果他们安全了也就没有第二问了。  
　　在集合Vector和HashTable倒是线程安全的。你打开源码会发现其实就是把各自核心方法添加上了`synchronized`关键字  
　　Collections工具类提供了相关的API，可以让上面那3个不安全的集合变为安全的。  
```java
Collections.synchronizedCollection(c);
Collections.synchronizedList(list);
Collections.synchronizedMap(m);
Collections.synchronizedSet(s);
```
　　上面几个函数都有对应的返回值类型，传入上面类型返回什么类型。打开源码其实实现原理非常简单，就是将集合的核心方法加上了`synchronized`关键字。  
### 3、ArrayList内部是用什么实现的？
　　（回答这样的问题，不要只答个皮毛，可以再介绍一下ArrayList内部是如何实现数组的增加和删除的，因为数组在创建的时候长度是固定的，那么久有个问题我们往ArrayList中不断的添加对象，它是如何管理这些数组的呢？）
　　ArrayList内部是用Object[]实现的。接下来我们分别分析ArrayList的构造、add、remove、clear方法的实现原理。
　　一、构造函数
　　1) 空构造
```java
public ArrayList() {
    array = EmptArray.OOJECT;
}
```
　　array是一个Object[]类型，当我们new一个空参构造时系统调用了EmptArray.OBJECT属性，EmptArray仅仅是一个系统的类库，该类源码如下：
```java
public final class EmptArray {

    public static final boolean[] BOOLEAN = new boolean[0];
    public static final byte[] BYTE = new byte[0];
    public static final char[] CHAR = new char[0];
    public static final double[] DOUBLE = new double[0];
    public static final int[] INT = new int[0];

    public static final Class<?>[] CLASS = new Class[0];
    public static final Object[] OBJECT = new Object[0];
    public static final String[] STRING = new String[0];
    public static final Throwable[] THROWABLE = new Throwable[0];
    public static final StackTraceElement[] STACK_TRACE_ELEMENT = new StackTraceElement[0];
}
```
　　也就是说当我们new一个空参ArrayList的时候，系统内部使用了一个new Object[0]数组。
