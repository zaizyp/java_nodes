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
　　在Java5以前stitch(expr)中，expr只能是byte、short、char、int。从Java5开始，Java中引入了枚举类型，expr也可以是enum类型。从Java7开始，expr也可以是String对象。
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
#### 3.1、构造函数
　　1) 空构造
```java
public ArrayList() {
    array = EmptArray.OOJECT;
}
```
　　array是一个Object[]类型，当我们new一个空参构造时系统调用了EmptArray.OBJECT属tfyfsghhgg性，EmptArray仅仅是一个系统的类库，该类源码如下：
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
　　2) 带参构造函数1
```java
public ArrayList(int capacity) {
    if (capatity < 0) {
        throw new IllegalArgumentException("capatity < 0 :" + capatity);
    }
    array = (capatity ==0 ? EmptyArrar.OBJECT : new Object[capacity]);
}
```
　　该构造函数传入一个int值，该值作为数组的长度值。如果该值小于0，则抛出一个运行时异常。如果等于0，则使用一个空数组，如果大于0，则创建一个长度为该值的新数组。  
　　3) 带参构造2
```java
public ArrayList(Collection<? extends E> collection) {
    if (collection == null) {
        throw new NullPointerException("collection == null");
    }
    Object[] a = collection.toArray();
    if(a.getClass() != Object.clsaa) {
        Object[] newArray = new Object[a.length];
        System.arraycopy(a, 0, newArray, 0, a.length);
        a = newAray;
    }
    array = a;
    size = a.length;
}
```
　　如果调用构造函数的时候传入了一个Collection的子类，那么先判断该集合是否为null，为null则抛出空指针异常。如果不是则将该集合转换为数组a，然后将该数组赋值给成员变量array，将该数组的长度作为成员变量size。这里面它先判断a.getClass是否等于Object[].class,其实一般是相等的，我也暂时没有想明白为什么多加了这个判断，toArray方法时Collection接口定义的，因此其所有的子类都有这样的方法，list集合的toArray和Set集合的toArray返回都是Object[]数组。
　　这里讲些题外话，其实在看Java源码的时候，作者的很多意图都很费人心思，我能知道他的目标是啥，但是不知道他为何这样写。比如对于ArrayList，array是他的成员变量，但是每次在方法中使用该成员变量的时候作者都会重新在方法中开辟一个局部变量，然后给局部变量赋值为array，然后再使用，有人可能会说这是为了防止并发修改array，毕竟array是成员变量，大家都可以使用因此需要将array变为局部变量，然后再使用。这样的说法并不是都成立的，也许有时候就是老外们写代码的一个习惯而已。
#### 3.2、add方法
　　add方法有两个重载，这里只研究最简单的那个。
```java
@Override
public boolean add(E object) {
    Object[] a = array;
    int s = size;
    if (s == a.length) {
        Object[] newArray = new Object[s + (s < (MIN_CAPATITY_INCREMENT / 2) ? MIN_CAPATITY_INCREMENT : s >> 1)];
        System.arraycopy(a, 0, newAray, 0, s);
        array = a = newAray;
    }
    a[s] = object;
    size = s + 1；
    modCount++;
    return true;
}
```
　　1、首先将成员变量array赋值给局部变量a，将成员变量的size赋值给局部变量s  
　　2、判断集合的长度是否等于数组的长度（如果集合的长度已经等于数组的长度了，说明数组已经满了，该重新分配新数组的），重新分配数组的时候需要计算新分配内存的空间大小，如果当前的长度小于MIN_CAPACITY_INCREMENT/2（这个常量值是12，除以2也就是6，如果当前集合的长度小于6）则分配12个长度，如果集合长度大于6则分配当前长度的一半长度，这里面用到了三元运算符和位运算符，s >> 1，意思就是将s往右移动一位，相当于s=s/2，只不过位运算是效率最高的运算  
　　3、将新添加的Object对象作为数组的a[s]个元素  
　　4、修改集合的长度size为s+1  
　　5、modCount++，该变量是父类中声明的，用于记录集合修改的次数，记录集合修改的次数是为了防止在用迭代器迭代集合时避免发生修改异常，或者说用于判断是否出现并发修改异常的  
　　6、return true，这个返回值意义不大，因为一直返回true，除非报了一个运行时异常。
#### 3.3、remove方法
　　　　remove方法有两个重载，我们只研究remove(int index)方法。
```java
@Override
public E remove (int index) {
    Object[] a = array;
    int s = size;
    if (index >= s) {
        throw IndexOutOfBoundsException(index, s);
    }
    E result = (E) a[index];
    System.arraycopy(a, index + 1, a, index, --s -index);
    a[s] = null; //Prevent memory leak
    size = s;
    modCount++;
    return result;
}
```
　　1、先将成员变量array和size赋值给局部变量a和s  
　　2、判断形参index是否大于等于集合的长度，如果长了则抛出运行时异常  
　　3、获取数组中角标为index的对象result，该对象作为方法的返回值  
　　4、调用System的arraycopy函数，拷贝原理：只需要将该数组后面区域整体往前移动一个位置即可  
　　5、接下来就是很重要的一个工作，因为删除了一个元素，而集合整体向前移动了一位，因此需要将集合最后一个元素设置为null，否则就可能内存泄漏  
　　6、重新给成员变量array和size赋值  
　　7、记录修改次数  
　　8、返回删除的元素（让用户再看最后一眼）
#### 3.4、clear方法
```java
@override
public void clear() {
    if (size != 0) {
        Arrays.fill(array, 0, size, null);
        size = 0;
        modCount++;
    }
}
```
　　如果集合长度不等于0，则将数组的所有值都设置为null，然后将成员变量size设置为0即可，最后让修改记录加1
### 4、并发集合和普通集合如何区别？
　　并发集合常见的有ConcurrentHashMap,ConcurrentLinkedQueue,ConcurrentLinkedDeque等。并发集合位于java.util.concurrent包下，是JDK1.5之后才有的，主要作者是Doug Lea完成的  
　　在java中有普通集合、同步（线程安全）的集合、并发集合，普通集合通常性能最该，但是不保证多线程的安全性和并发的可靠性。线程安全集合仅仅是给集合添加了synchronized同步锁，严重牺牲了性能，而且对并发的效率就更低了，并发集合则通过复杂的策略不仅保证了多线程的安全又提高了并发时的效率。
　　参考阅读：  
　　ConcurrentHashMap是线程安全的HashMap的实现，默认构造同样有initialCapatity和loadFactor属性，不过还多了一个concurrentLevel属性，三属性默认值分别为16、0.75、16。其内部使用锁分段技术，维持着锁Segment的数组，在Segment数组中又存放着Entity[]数组，内部hash算法将数据较均匀分布在不同锁中。  
　　**put操作**：并没有在此方法加上synchronized，首先对key.hashCode进行hash操作，得到key的hash值。hash操作的算法和map的也不同，根据此hash值计算并获取其对应的数组中Segment对象(继承自ReentrantLock)，接着调用此Segment对象的put方法来完成当前操作。  
　　CurrentHashMap基于concurrentLevel划分出了多个Segment来对key-value进行存储，从而避免每次put操作都得锁住整个数组。在默认的情况下，最佳情况可允许16个线程并发无阻塞的操作集合对象，尽可能地减少并发时的阻塞现象。  
　　**get(key)**：首先对key.hashCode进行hash操作，基于其值找到对应的Segment对象，调用其get方法完成当前操作。而Segment的get操作首先通过hash值和对象数组大小减1的值进行按位与来获得操作数组上对应位置的HashEntry。而在这个步骤中，可能会因为对象数组大小的改变，以及数组上对应位置的HashEntry产生不一致性，那么ConcurrentHashMap是如何保证的呢？  
　　对象数组大小的改变只有在put操作时有可能发生，由于HashEntry对象数组对应的变量是volatile类型的，因此可以保证如HashEntry对象数组大小发生改变，读操作可以看到最新的对象数组大小。  
　　在获取到了HashEntry对象之后，怎么能保证它及其next属性构成的链表上的对象不会改变呢？这点ConcurrentHashMap采用了一个简单的方式，即HashEntry对象中的hash,key,next属性都是final的，这也就意味着没办法插入一个HashEntry对象到基于next属性构成的链表中间或者末尾。这样就可以保证当获取到HashEntry对象后，其基于next属性构建的链表是不会发生变化的。
　　ConcurrentHashMap默认情况下采用将数据分为16个段进行存储，并且16个段分别各自持有不同的锁Segment，锁仅用于put和remove等改变集合对象的操作，基于volatile及HashEntry链表的不变性实现了读取的不加锁。这些方式使得ConcurrentHashMap能够保持极好的并发支持，尤其是其对于读远比插入和删除频繁的Map而言，而采用这些也可谓是对于Java内存模型、并发深刻掌握的体现。
### 5、List的三个子类的特点
ArrayList 底层结构是数组，查询快、增删慢  
LinkedList 底层结构的链表型的，增删快，查询慢  
Vector 底层是数组结构 线程安全的，增删慢，查询慢
### 6、List和Map、Set的区别

#### 6.1 结构特点
　　List和Set是存储单列数据的集合，Map是存储键和值的双列数据的集合；List中存储的数据是有顺序的，并且允许重；Map中存储的数据是没有顺序的，其键是不能重复的，它的值是可以重复的；Set中存储是数据是无序的，且不允许有重复，但元素在集合中的位置由元素的hashCode决定，位置是固定的（Set集合根据hashCode来进行存储，所以位置是固定的，但是位置不是用户可以控制的，所以对用户来说Set中的元素还是无序是）
#### 6.2 实现类
　　List接口有三个实现类（**LinkedList：** 基于链表实现，链表内存是散乱的，每一个元素存储本身内存地址的同时还存储下一个元素的地址。链表增删快，查找慢；**ArryList：** 基于数组实现，非线程安全，效率高，便于索引，但不便于插入删除；**Vector：** 基于数组实现，线程安全，效率低）  
　　Map接口有三个实现类（**HashMap:** 基于hash表的Map接口实现，非线程安全，高效，支持null值和null键；**HashTable** 线程安全，低效，不支持null值和null键；**LinkedHashMap** 是HashMap是一个子类，保存了记录的插入顺序  
　　SortMap接口：**TreeMap：** 能够把它保存的记录根据键排序，默认是键值的升序排序）  
　　Set接口：有两个实现类（**HashSet：** 底层是由HashMap实现，不允许集合中有重复的值，使用该方式时需要重写equals()和hashCode()方法；**LinkedHashSet：** 继承自HashSet，同时又基于LinkedHashMap来进行实现，底层使用的是LinkedHashMap）
#### 6.3 区别
　　List集合中对象按照索引位置排序，可以有重复对象，允许按照对象在集合中索引位置检索对象，例如通过`list.get(i)`来获取集合中的元素；Map中的每一个元素包含一个键和一个值，成对出现，键对象不可重复，值对象可以重复；Set集合中的对象不按照特定的方式排序，并且没有重复对象，但它的实现类能对集合中的对象按照特定的方式排序，例如TreeSet类，可以按照默认顺序，也可以通过实现java.util.Comparator<Type>接口来自定义排序方式
### 7、HashMap和HashTable有什么区别
　　HashTable是线程安全的一个集合，不允许有null值作为一个key值或者Value值；  
　　HashTable是synchronized，多个线程访问时不需要自己为它的方法实现同步，而HashMap在被多个线程访问时需要自己为它的方法实现同步
### 8、数组和链表分别比较适合用于什么场景，为什么
#### 8.1、 数组和链表简介
　　在计算机中要对给定的数据集进行若干处理，首要任务就是把数据集的一部分（当数据量非常大时，可能只能一部分一部分地读取到内存中来处理）或全部存储到内存中，然后再对内存中的数据进行各种处理  
　　例如，对于数据集S{1,2,3,4,5,6}，要求吧数据连续的存放在内存中，各种编程语言中的数组一般都是按照这种方式进行存储的（当然也可能有例外），如图1(b)；当内存中只有一些离散的可用空间时，想继续存储数据就非常困难了，这时能想到的一种解决方式是移动内存中的数据，把离散的空间聚聚成连续的一大块空间，如图1（c）所示，这样做当然也可以，但是这种情况可能要移动别人的数据，所以会存在一些困难，移动的过程中也有可能会把一些别人的重要数据给丢失。另外一种，不影响别人的数据存储方式是把数据集中是数据分开离散地存储到这些不连续空间中，如图（d）。这时为了把数据集中的所有数据联系起来，需要在前一块数据的空间中记录下一块数据的地址，这样只要知道第一块内存空间的地址就能环环相扣把数据集整体联系在一起了。C/C++中用指针实现的链表就是这种存储形式。  
<img alt="数组内存分配" src="/pic/数组内存分配.png" />  
　　由上图可知，内存中的存储形式可以分为连续存储和离散存储两种形式。因此，数据的物理存储结构就有连续存储和离散存储两种，它们对应了我们通常所说的数组和链表
#### 8.2、数组和链表的区别
　　数组是将元素在内存中连续存储的；它的优点：因为数据是连续存储的，内存地址是连续的，所以在查找数据的时候效率比较高；它的缺点：它在存储之前，我们需要申请一块连续的内存空间，并且在编译的时候就必须确定好它的空间的大小。在运行的时候空间的大小是无法随着你的需要进行增加和减少而改变的，当数据比较大的时候，有可能会出现越界的情况，数据比较小的时候，有可能会浪费掉内存空间。在改变数据个数时，增加、插入、删除数据效率比较低  
　　链表是动态申请内存空间，不需要像数组提前申请好内存的大小，链表只需要在用的时候申请就可以，根据需要来动态申请或删除内存空间，对于数据增加和删除以及插入比数组灵活。还有就是链表中数据在内存中可以在任意的位置，通过应用来关联数据（就是通过存在元素指针来联系）
#### 8.3、链表和数组使用场景
　　数组应用场景：数据比较少；经常做的运算是按序号访问数据元素；数组更容易实现，任何高级语言都支持；构建的线性表较稳定。  
　　链表应用场景：对线性表的长度或者规模难以估计；或频繁做插入删除操作；构建动态性比较强的线性表。  
#### 8.4、跟数组相关的面试题
　　用面向对象的方法求出数组中重复value的个数，按如下个数输出：
　　　　1 出现：1次  
　　　　3 出现：2次  
　　　　8 出现：3次  
　　　　2 出现：4次  
　　　　int[] arr = {1,4,1,2,5,8,7,8,77,88,5,4,9,6,2,4,1,5};
### 9、Java中ArryList和LinkedList区别
　　ArryList和Vector使用了数组的实现，可以认为ArryList或者Vector封装了对内部数组的操作，比如先数组中添加，删除，插入新的元素或者数据的扩展和重定向。  
　　LinkedList使用了循环双向链表数据结构。与基于数组的ArryList相比，这是两种截然不同的实现技术，这也决定了它们将适用于完全不同的工作场景。  
　　LinkedList链表由一系列表项连接而成。一个表项总是包含3个部分：元素内容，前驱表和后驱表，如图所示：  
![链表结构](/pic/链表结构.png)
　　在下图展示了一个包含3个元素的LinkedList的各个表项间的连接关系。在JDK的实现中，无论LinkedList是否为空，链表内部都有一个header表项，它既表示链表的开始，也表示链表的结尾。表项header的后驱表项表便是链表中第一个元素，表项header的前驱表项便是链表中最后一个元素。  
![双向链表结构](/pic/双向链表结构.png)
### 10、`List a = new ArrayList()`和`ArrayList a = new ArrayList()`的区别
　　`List a = new Arraylist()`这句创建了一个ArrayList对象后把引用指向了List。此时它是一个List对象了，有些ArrayList有但是List没有的属性和方法，它就不能再用了。而`ArrayList a = new ArrayList()`创建对象则保留了ArrayList的所有属性。所以需要用到ArrayList独有的方法的时候不能用前者。
### 11、要对集合进行更新操作时，ArrayList和LinkedList哪个更合适
　　1) ArrayList是实现了基于动态数组的数据结构，LinkedList基于链表的数据结构  
　　2) 如果集合数据是对于集合随机访问get和set，ArrayList绝对由于LinkedList，因为LinkedList要移动指针  
　　3) 如果集合数据是对于集合新增和删除操作add和remove，LinkedList比较占优势，因为ArrayList要移动数据  
　　ArrayList和LinkedList是两个集合类，用于存储一系列的对象引用（references）。例如我们可以用可以用ArrayList来存储一系列的String或者Integer。那么ArrayList和LinkedList在性能上有什么差别呢？什么时候应该用ArrayList什么时候又应该用LinkedList呢？  
　　一.时间复杂度
　　首先一点关键的是，ArrayList的内部实现是基于基础的对象数组的，因此，它使用get方法访问列表中的任意一个元素时（random access），它的速度要比LinkedList快。LinkedList中的get方法时按照顺序从列表的一端开始检查，直到另外一端。对于LinkedList而言，访问列表中的某个指定元素没有更快的方法了  
　　假设我们又一个很大的列表，它里面的元素已经排序好了，这个列表可能是ArrayList类型的也可能是LinkedList类型的，现在我们对这个列表来进行二分查找（binary search），比较列表是ArrayList和LinkedList时的查询速度，看下面的程序：  
```java
public class TestList{
    public static final int N = 5000;      //5000个数
    public static List values;            //要查找的集合
    //放入5000个数给value
    static{
        Integer vals[] = new Integer[N];
        Random r = new Random();
        for(int i = 0, currval = 0; i < N; i++){
            vals[i] = new Integer(currval);
            currval += r.nextInt(100)+1;
        }
        values = Arrays.asList(vals);
    }
    //通过二分法查找
    static Long timeList(List lst){
        long start = System.currentTimeMillis();
        for(int i = 0; i < N; i++) {
            int index = Collections.binarySearch(lst, values.get(i));
            if(index != i){
                System.out.println("***错误***");
            }
        }
        return System.currentTimeMillis() - start;
    }
    public static void main(String[] args) {
        System.out.println("ArrayList消耗时间："+timeList(new ArrayList(values)));
        System.out.println("LinkedList消耗时间："+timeList(new LinkedList(values)));
    }
}
```
　　得到的输出是：
```
Arraylist消耗时间：5
LinkedList消耗时间：83
```
　　这个结果不是固定的，但基本上ArrayList的时间要明显小于LinkedList的时间。因此在这种情况下不宜用LinkedList。二分查找法使用的随机访问（random access）策略，而LinkedList是不支持快速的随机访问的。对一个LinkedList做随机访问所消耗的时间与这个list的大小是成比例的。而相应的，在ArrayList中进行随机访问所消耗的时间是固定的。  
　　这是否表明ArrayList总是比LinkedList性能要好呢？这并不一定，在某些情况下LinkedList的表现要优于ArrayList，有些算法在LinkedList中实现时效率更高。比方说，利用`Collections.reverse()`方法对列表进行反转时，其性能就要好些。看这样一个例子，假如我们有一个列表，要对其进行大量的插入和删除操作，在这种情况下LinkedList就是一个较好的选择。请看如下一个极端的例子，我们重复在一个列表的开端插入一个元素：
```java
public class TestList2 {
    static final int N = 50000;
    static long timeList(List list) {
        long start = System.currentTimeMillis();
        Object o = new Object();
        for(int i = 0; i < N; i++)
            list.add(0, o);
        return System.currentTimeMillis()-start;
    }
    public static void main(String[] args) {
        System.out.println("ArrayList消耗时间："+timeList(new ArrayList()));
        System.out.println("LinkedList消耗时间："+timeList(new LinkedList()));
    }
}
```
　　这时我们的输出结果是：
```
ArrayList消耗时间：213
LinkedList消耗时间：5
```
　　二、空间复杂度
　　在LinkedList中有一个私有的内部类，定义如下：
```java
private static class Entry {
    Object element;
    Entry next;
    Entry previous;
}
```
　　每个Entry对象reference列表中的一个元素，同时还有在LinkedList中它的上一个元素和下一个元素。一个有1000个元素的LinkedList对象将有1000个链接在一起的Entry对象，每个对象都对应于列表中的一个元素。这样的话，在一个LinkedList结构中将有一个很大的空间开销，因为它要存储这1000个Entry对象的相关信息。  
　　ArrayList使用一个内置的数组来存储元素，这个数组的起始容量时10，当数组需要增长时，新的容量按如下公式获得：新容量=（旧容量*3）/2+1,也就是说每一次容量大概会增长50%。这就意味着，如果你有一个包含大量元素的ArrayList对象，那么最终将会有很大的空间会被浪费掉，这个浪费是由ArrayList的工作方式本身造成的。如果没有足够的空间来存放新的元素，数组将不得不被重新进行分配以便能够增加新的元素。对数组进行重新分配，将会导致性能急剧下降。如果我们知道一个ArrayList将会有多少个元素，我们可以通过构造方法来指定容量。我们还可以通过trimToSize方法在ArrayList分配完毕之后去掉浪费的空间    
　　三、总结  
　　ArrayList和LinkedList在性能上各有各的优缺点，都有各自所试用的地方，总的来说可以描述如下：  
　　1、对ArrayList和LinkedList而言，在列表末尾增加一个元素所花的开销都是固定的。对ArrayList而言，这要是在内部数组中增加一项，指向所添加的元素，偶尔可能会导致对数组重新进行分配；而对LinkedList而言，这个开销是统一的，分配一个内部Entry对象  
　　2、在ArrayList的中间插入一个或删除一个有元素意味着这个列表中剩余的元素都会被移动；而在LinkedList的中间插入或删除一个元素的开销是固定的  
　　3、LinkedList不支持高效的随机元素访问  
　　4、ArrayList的空间浪费主要体现在在list列表的结尾预留一定的容量空间，而LinkedList的空间花费则体现在它的每一个元素都需要消耗相当的空间  
　　可以这样说：当操作是在一列数据的后面添加数据而且不是在前面或中间，并且需要随机地访问其中的元素时，使用ArrayList会提供比较好的性能；当你的操作是在一列数据的前面或中间添加或删除数据，并且按照顺序访问其中的元素时，就应该使用LinkedList了。
### 12、请用两个队列模拟堆栈结构
　　两个队列模拟一个堆栈，队列是先进先出，而堆栈是后进后出。模拟如下：  
　　队列a和b  
　　(1) 入栈：a队列为空，b为空。例：则将“a,b,c,d,e”需要入栈的元素先放入a中，a进栈为“a,b,c,d,e”
　　(2) 出栈：a队列目前的元素为“a,b,c,d,e”。将a队列依次加入到ArrayList集合a中。以倒序方法，将a中的集合取出，放入b队列中，再将b队列出列。代码如下：
```java
public static void main(String[] args) {
    Queue<String> queue = new LinkedList<String>();   //a队列
    Queue<Stirng> queue2 = new LinkedList<String>();  //b队列
    Arraylist<String> a = new ArrayList<Sting>();     //arralist集合是中间参数
    queue.offer("a");
    queue.offer("b");
    queue.offer("c");
    queue.offer("d");
    queue.offer("e");
    System.out.print("进栈：");
    //a队列依次加入list集合之中
    for(String q : queue) {
        a.add(q);
        System.out.print(q);
    }
    //以倒序的方法取出（a队列依次加入list集合）之中的值，加入b队列
    for(int i = a.size()-1; i > 0; i--) {
        queue2.offer(a.get(i));
    }
    //打印出栈队列
    System.out.println("");
    System.out.print("出栈：");
    for(String q : queue2) {
        System.out.print(q);
    }
}
```
　　打印结果为（遵循栈模式先进后出）：
```
进栈：a b c d e
出栈：e d c b a
```
### 13、Collection和Map的集成体系
　　Collection：
![Java集合Collection](/pic/Java集合Collection.png)
　　Map:
![Java集合Collection](/pic/Java集合Collection.png)
### 14、Map中的key可以和value可以为null么
　　HashMap对象的key和value值均可以为null  
　　HashTable对象的key和value值均不可以为null  
　　且两者的key值均不能重复，若添加key相同的的键值对，后面的value会自动覆盖前面的value，但不会报错  
## 九、Java的多线程并发库
　　对于Java程序员来说，多线程在工作中的使用场景还是比较常见的，而仅仅掌握了Java中的传统多线程机制，还是不够的。在JDK1.5之后，Java增加的并发库中提供了很多优秀的API，在实际开发中用的比较多。因此在看具体的面试题之前我们有必要对这部分知识做一个全面的了解  
　　（一）多线程基础知识--传统线程机制的回顾  
　　（1）传统使用类Thread和接口Runnable实现   
　　1、在Thread子类覆盖的run方法中编写运行代码  
　　方式一：  
```java
new Thread() {
    @override
    public void run() {
        while(true) {
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
               e.printStackTrace();
            }
        }
    }
}.start();
```
　　2、在传递给Thread对象的Runnable对象的run方法中编写代码
```java
new Thread(new Runnable(){
    @Override
    public void run(){
        while(true) {
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName());
        }
    }
}).start();
```
　　3、总结  
　　查看Thread类的run()方法的源代码，可以看到其实这两种方式都是在调用Thread对象的run方法，如果Thread类的run方法没有被覆盖，并且为该Thread对象设置了一个Runnable对象，该run方法会调用Runnable对象的run方法  
　　（3）线程互斥与同步  
　　在引入了多线程之后，由于线程执行的异步性，会给系统造成混乱，特别是在急用临界资源时，如多个线程急用同一台打印机，会使打印结果交织在一起，难于区分。当多个线程急用共享变量、表格、链表时，可能会导致数据处理出错，因此线程同步的主要任务是使并发执行的各线程之间能够有效的共享资源和合作，从而使程序的执行具有可在观性。  
　　当线程并发执行时，由于资源共享和线程协作，使用线程之间会存在以下两种制约关系。  
　　1、间接相互制约。一个系统中的多个线程必然要共享某种系统资源，如共享CPU，共享I/O设备，所谓间接相互制约即源于这种资源共享，打印机就是最好的例子，线程A在使用打印机时，其他线程都要等待。  
　　2、直接相互制约。这种制约主要是因为主线程之间的合作，如有线程A将计算结果提供给B作进一步处理，那么线程B在A将数据传达之前都将处于阻塞状态  
　　间接相互制约可以称为**互斥**，直接相互制约可以称为**同步**，对于互斥可以理解成这样，线程A和线程B互斥访问某个资源则它们之间就会产生一个顺序问题--要么线程A等待线程B操作完毕，要么线程B等待线程A操作完毕，这其实就是线程同步了。因此，同步包括互斥，互斥其实是一种特殊的同步。  
　　下面我们通过一道面试题来体会线程的交互  
　　要求：子线程运行执行10次后，主线程在运行5次。这样交替执行三遍   
```java
public class ThreadTest {

	public static void main(String[] args) {
	    final Bussiness bussiness = new Bussiness();
	    //子线程
	    new Thread(new Thread() {
	        @Override
	        public void run(){
	            for(int i = 0; i < 3; i++)
	                bussiness.subMethod();
	        }
	    }).start();
	    //主线程
	    for(int i = 0; i < 3; i++)
	        bussiness.mainMethod();
	}
}
class Bussiness{
    private boolean flag = false;

    public synchronized void subMethod() {
        while(flag) {
            try {wait();} catch(InterruptedException e) {e.printStackTrace();}
        }
        for (int i = 0; i < 10; i++) {
            System.out.println(Thread.currentThread().getName()+"----"+i);
        }
        flag = true;
        notify();
    }
    public synchronized void mainMethod() {
        while(!flag){
            try {wait();} catch(InterruptedException e) {e.printStackTrace();}
        }
        for (int i = 0; i < 5; i++)
            System.out.println(Thread.currentThread().getName() + "----" +i);
        flag = false;
        notify();
    }
}
```
　　（4）线程局部变量ThreadLocal  
　　- ThreadLocal 的作用和目的：用于实现线程内的数据共享，即对于相同的程序代码，多个模块在同一个线程中运行时要共享一份数据，而在另外线程中运行时又共享另外一份数据。  
　　- 每个线程调用全局ThreadLocal对象的Set方法，在Set方法中，首先根据当前线程获取当前线程的ThreadLocalMap对象，然后往这个map中插入一条记录，key其实就是ThreadLocal对象，value是各自set方法传进去的值。也就是每个线程其实都有一份自己独享的ThreadLocal对象，该对象的key是ThreadLocal对象，值是用户设置的具体值。在线程结束时可以调用`ThreadLocal.remove()`方法，这样会更快释放内存，不调用也可以，因为线程结束后也可以自动释放相关的ThreadLocal变量  
　　- ThreadLocal的应用场景：  
　　　　- 订单处理包含一系列操作：减少库存量、增加一条流水台账、修改总账，这几个操作要在同一个事务中完成，通常也即同一个线程中进行处理，如果累加公司应收账款的操作失败了，则应该把前面的操作回滚，否则，提交所有操作，这些操作使用相同的数据库连接对象，而这些操作的代码分别位于不用的模块中  
　　　　- 银行转账包含一系列操作：把转出账户的余额减少，把转入账户的余额增加，这两个操作要在同一个事务中完成，它们必须使用相同的数据库连接对象，转入和转出操作的代码分别是两个不同的账户对象的方法。
　　　　- 例如Strus2的ActionContext，同一段代码被不同的线程调用运行时，该代码操作的数据是每一个线程各自的状态和数据，对于不同的线程来说，getContext方法拿到的对象都不相同，对同一个线程来说，不管调用getContext方法多少次和在哪个模块中调用getContext方法，拿到的都是同一个。  
　　1、ThreadLocal的使用方式  
　　（1）在关联数据类中创建`private static ThreadLocal`  
　　　　在下面的类中，私有静态ThreadLocal实例（serialNum）为调用该类的静态`SerialNum.get()`方法的每个线程维护了一个“序列号”，该方法将返回当前线程的序列号。（线程的序列号是在第一次调用`SerialNum.get()`时分配的，并在后续调用中不会更改。）  
```java
public class SerialNum {
    //The next serial num to be assigned
    private static int nextSerialNum = 0;
    private static ThreadLocal serial = new ThreadLocal() {
        protected synchronized Object initialValue() {
            return new Integer(nextSerialNum++);
        }
    };
    public static int get() {
        return ((Integer) (serialNum.get())).intValue;
    }
}
```
　　另一个例子，也是私有静态ThreadLocal实例：
```java
public class ThreadContext {
    private String userId;
    private Long transactionId;

    private static ThreadLocal threadLocal = new ThreadLocal() {
        @override
        protected ThreadContext initialValue() {
            return new ThreadContext();
        }
    };
    public static ThreadContext get() {
        return threadLocal.get();
    }

    public String getUserId() {
        return userId;
    }
    public Long getTransactionId() {
        return transactionId;
    }
    public void setTransactionId(Long transactionId) {
        this.transactionId = transactionId;
    }
}
```
