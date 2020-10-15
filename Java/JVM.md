# JVM


>
>* **友情链接：**[常见OMM Error和七大垃圾回收器详解](https://www.jianshu.com/p/ba415aa2330b)
>* **友情链接：**[JVM调参、GCRoots与四大引用浅析](https://www.jianshu.com/p/13afdf841f68)

## JVM介绍

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-bba54463485506eb.png)

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-2dfbe0fa4266a276.png)

1.**方法区和堆区是所有线程共享的内存区域；而java栈、本地方法栈和程序计数器是运行时线程私有的内存区域。**

2.Java栈又叫做jvm虚拟机栈

3.方法区（永久代）在 jdk8 中又叫做**元空间** Metaspace

- **方法区用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器（JIT编译器，英文写作Just-In-Time Compiler）编译后的代码等数据。**虽然Java虚拟机规范把方法区描述为堆的一个逻辑部分，但是它却有一个别名叫做 Non-Heap（非堆），目的应该是与 Java 堆区分开来。
- 在JDK1.7之前运行时常量池逻辑包含字符串常量池存放在方法区, 此时hotspot虚拟机对方法区的实现为永久代
- 在JDK1.7 字符串常量池被从方法区拿到了堆中，这里没有提到运行时常量池,也就是说字符串常量池被单独拿到堆，运行时常量池剩下的东西还在方法区, 也就是hotspot中的永久代
- **在JDK1.8之后JVM 已经将运行时常量池从方法区中移了出来，在 Java 堆（Heap）中开辟了一块区域存放运行时常量池**。同时在 jdk 1.8中移除整个永久代，取而代之的是一个叫元空间（Metaspace）的区域
- 元空间的本质和永久代类似，都是对JVM规范中方法区的实现。不过元空间与永久代之间最大的区别在于：**元空间并不在虚拟机中，而是使用本地内存**。因此，默认情况下，元空间的大小仅受本地内存限制。**默认大概是用了20M的大小**。

4.java代码执行流程：

　　java程序--（编译javac）-->字节码文件.class-->类装载子系统化身为反射类Class--->运行时数据区--->（解释执行）-->操作系统（Win，Linux，Mac JVM）

* **所有的栈和程序计数器并不会产生垃圾**，所以不会有垃圾回收
* 我们所说的JVM调优是在方法区和堆里进行调优，**99%是堆调优**
* 注意：我们平时说的栈是指的Java栈，本地方法栈（native method stack） 里面装的都是native方法。

> **JVM生命周期**

**1.启动**

通过**引导类根加载器（bootstrap class loader）创建一个初始类（initial class）来完成的**，这个类是由虚拟机的具体实现指定的.

**2.执行**

- 一个运行中的java虚拟机有着一个清晰的任务：执行Java程序；
- 程序开始执行的时候他才运行，程序结束时他就停止；
- 执行一个所谓的Java程序的时候，真真正正在执行的是一个叫做**Java虚拟机的进程**。

**3.退出**

- 程序正常执行结束
- 程序异常或错误而异常终止
- 操作系统错误导致终止
- 某线程调用Runtime类或System类的exit方法，或Runtime类的halt方法，并且java安全管理器也允许这次exit或halt操作
- 除此之外，JNI 规范描述了用 JNI Invocation API 来加载或卸载Java虚拟机时，Java虚拟机的退出情况

![img](https://gitee.com/xudongyin/img/raw/master/img/1846149-20200401105306155-3084373.png)

## 类加载子系统

### **作用**

1.类加载子系统负责从文件系统或者网络中加载class文件，class文件在**文件开头有特定的文件标识**，JVM并不是通过检查文件后缀是不是`.class`来判断是否需要加载的，而是通过**文件开头的特定文件标志**即16进制CA TE BA BE；

![文件开头的特殊标识](https://gitee.com/xudongyin/img/raw/master/img/4070621-91c98e0dcc20f0ed.png)

2.**加载后的Class类信息存放于一块成为方法区的内存空间**。除了类信息之外，方法区还会存放运行时常量池信息，可能还包括字符串字面量和数字常量（这部分常量信息是Class文件中常量池部分的内存映射）

来一张经典的JVM内存结构图：其中类加载器的工作范围只限于下图的左半部分，不包含调用构造器实例化对象

![img](https://gitee.com/xudongyin/img/raw/master/img/1846149-20200401105500017-801589677.png)

3.ClassLoader只负责class文件的加载，至于它**是否可以运行，则由Execution Engine决定**

4.如果调用构造器实例化对象，则其**实例存放在堆区**

### 功能细分

![img](https://gitee.com/xudongyin/img/raw/master/img/1846149-20200401105701873-414824729.png)

####  加载模块

1.通过一个类的全限定名获取定义此类的二进制字节流；

2.将这个字节流所代表的的静态存储结构转化为方法区的运行时数据；

3.在内存中生成一个代表这个类的java.lang.Class对象，作为方法区这个类的各种数据的访问入口



#### 链接模块分为三块，即验证、准备、解析

**验证**

1.目的在于确保Class文件的字节流中包含信息符合当前虚拟机要求，保证被加载类的正确性，不会危害虚拟机自身安全。

2.主要包括四种验证，文件格式验证，源数据验证，字节码验证，符号引用验证。

**准备**

1.为类变量分配内存并且设置该类变量的默认初始值，即零值；

2.这里不包含用final修饰的static，因为**final在编译的时候就会分配了**，准备阶段会显式初始化；

3.不会为实例变量分配初始化，类变量会分配在方法区中，而实例变量是会随着对象一起分配到java堆中。

**解析**

1.将常量池内的符号引用转换为直接引用的过程。

2.事实上，解析操作往往会伴随着JVM在执行完初始化之后再执行

3.符号引用就是一组符号来描述所引用的目标。符号应用的字面量形式明确定义在《java虚拟机规范》的class文件格式中。直接引用就是直接指向目标的指针、相对偏移量或一个间接定位到目标的句柄

4.解析动作主要针对类或接口、字段、类方法、接口方法、方法类型等。对应常量池中的CONSTANT_Class_info/CONSTANT_Fieldref_info、CONSTANT_Methodref_info等。



#### 初始化模块，初始化阶段就是执行类构造器方法clinit（）的过程

1.clinit()即“class or interface initialization method”，注意他并不是指构造器init()

2.此方法不需要定义，是javac编译器自动收集类中的**所有类变量的赋值动作和静态代码块中的语句合并而来。** 

3.我们注意到如果没有静态变量c，那么字节码文件中就不会有clinit方法

![img](https://gitee.com/xudongyin/img/raw/master/img/1846149-20200401110130474-1529475451.png)

 

![img](https://gitee.com/xudongyin/img/raw/master/img/1846149-20200401110136134-452502146.png)

 

构造器方法clinit()中指令按语句在源文件中出现的顺序执行

![img](https://gitee.com/xudongyin/img/raw/master/img/1846149-20200401110204405-205185586.png)

 **java程序对类的使用方式分为：主动使用和被动使用，即是否调用了clinit()方法**

主动使用在类加载系统中的第三阶段initialization（初始化模块），即初始化阶段调用了clinit()方法

而被动使用不会去调用

主动使用，分为七种情况

1.创建类的实例

2.访问某各类或接口的静态变量，或者对静态变量赋值

3.调用类的静态方法

4.反射 比如Class.forName(com.dsh.jvm.xxx)

5.初始化一个类的子类

6.java虚拟机启动时被标明为启动类的类

7.JDK 7 开始提供的动态语言支持：java.lang.invoke.MethodHandle实例的解析结果REF_getStatic、REF_putStatic、REF_invokeStatic句柄对应的类没有初始化，则初始化

除了以上七种情况，其他使用java类的方式都被看作是对**类的被动使用，都不会导致类的初始化。**



#### 虚拟机必须保证一个类的clinit()方法在多线程下被同步加锁

即一个类只需被clinit一次，之后该类的内部信息就被存储在方法区。

![img](https://gitee.com/xudongyin/img/raw/master/img/1846149-20200401110344885-1015535384.png)

 可以看到**线程2并不会重复执行初始化操作**



### 类加载器种类


![](https://gitee.com/xudongyin/img/raw/master/img/4070621-026cc3734789ca8f.png)

> **注意：**
>
> * Class loader有多种，可以说三个，也可以说是四个（第四个为**自己定义的加载器，继承 ClassLoader**），系统自带的三个分别为：
> 1. 启动类加载器(Bootstrap) ，C++所写
>
>    - 这个类加载使用**C/C++语言实现的**，嵌套在JVM内部
>    - 它用来加载java的核心库（JAVA_HOME/jre/lib/rt.jar/resources.jar或sun.boot.class.path路径下的内容），用于提供JVM自身需要的类
>    - 并不继承自java.lang.ClassLoader,没有父加载器
>    - 加载拓展类和应用程序类加载器，并指定为他们的父加载器，即ClassLoader
>    - 出于安全考虑，BootStrap启动类加载器只加载包名为java、javax、sun等开头的类
>
> 2. 扩展类加载器(Extension) ，Java所写
>
>    - java语言编写 ，由sun.misc.Launcher$ExtClassLoader实现。
>    - 派生于ClassLoader类，**不是继承**
>    - 从java.ext.dirs系统属性所指定的目录中加载类库，或从JDK的安装目录的jre/lib/ext子目录（扩展目录）下加载类库。**如果用户创建的JAR放在此目录下，也会由拓展类加载器自动加载**
>
> 3. 应用程序类加载器 (AppClassLoader)
>
>    - java语言编写， 由sun.misc.Launcher$AppClassLoader实现。
>    - 派生于ClassLoader类，**不是继承**
>    - 它负责加载环境变量classpath或系统属性 java.class.path指定路径下的类库
>    - **该类加载器是程序中默认的类加载器**，一般来说，java应用的类都是由它来完成加载
>    - 通过ClassLoader#getSystemClassLoader()方法可以获取到该类加载器
>
>    **用户自定义类加载器：**
>
>    在日常Java应用程序开发中，类的加载几乎是由上述3种类加载器相互配合执行的，必要时可以自定义类加载器来定制类的加载方式。
>
>    为什么要自定义类加载器呢？
>
>    - 隔离加载类
>    - 修改类加载方式
>    - 扩展加载源
>    - 防止源码泄露
>
> 我们自己new的时候创建的是应用程序类加载器(AppClassLoader)。

```java
import com.gmail.fxding2019.T;

public class  Test{
    //Test:查看类加载器
    public static void main(String[] args) {

        Object object = new Object();
        //查看是那个“ClassLoader”（快递员把Object加载进来的）
        System.out.println(object.getClass().getClassLoader());
        //查看Object的加载器的上一层
        // error Exception in thread "main" java.lang.NullPointerException（已经是祖先了）
        //System.out.println(object.getClass().getClassLoader().getParent());

        System.out.println();

        Test t = new Test();
        System.out.println(t.getClass().getClassLoader().getParent().getParent());
        System.out.println(t.getClass().getClassLoader().getParent());
        System.out.println(t.getClass().getClassLoader());
    }
}

/*
*output:
* null
* 
* null
* sun.misc.Launcher$ExtClassLoader@4554617c
* sun.misc.Launcher$AppClassLoader@18b4aac2
* */
```

>**注意：**
>* 如果是**JDK自带的类(Object、String、ArrayList等)，其使用的加载器是Bootstrap加载器；如果自己写的类，使用的是AppClassLoader加载器；Extension加载器是负责将把java更新的程序包的类加载进行**
>* 输出中，**sun.misc.Launcher是JVM相关调用的入口程序**
>* Java加载器个数为3+1。前三个是系统自带的，用户可以定制类的加载方式，通过继承Java. lang. ClassLoader

> **JVM中表示两个class对象是否为同一个类**

1.在jvm中表示两个class对象是否为同一个类存在的两个必要条件

- 类的**完整类名必须一致**，包括包名
- 即使类的完整类名一致，同时要求**加载这个类的ClassLoader（指ClassLoader实例对象）必    须相同**；是引导类加载器、还是定义类加载器

2.换句话说，在jvm中，即使这两个类对象（class对象）来源同一个Class文件，被同一个虚拟机所加载，但只要加载它们的ClassLoader实例对象不同，那么这两个类对象也是不相等的.

3.对类加载器的引用，JVM必须知道一个类型是由启动类加载器加载的还是由用户类加载器加载的。如果一个类型由用户类加载器加载的，那么JVM会**将这个类加载器的一个引用作为类型信息的一部分保存在方法区中**。当解析一个类型到另一个类型的引用的时候，JVM需要保证两个类型的加载器是相同的。



### 双亲委派机制


![](https://gitee.com/xudongyin/img/raw/master/img/4070621-2cf08ab9b6da3c81.png)
>**注意：**
>* 双亲委派机制：“我爸是李刚，有事找我爹”。
>例如：**需要用一个A.java这个类，首先去顶部Bootstrap根加载器去找，找得到你就用，找不到再下降一层，去Extension加载器去找，找得到就用，找不到再将一层，去AppClassLoader加载器去找，找得到就用，找不到就会报"CLASS NOT FOUND EXCEPTION"。**
```java
//测试加载器的加载顺序
package java.lang;

public class String {

    public static void main(String[] args) {

        System.out.println("hello world!");

    }
}

/*
* output:
* 错误: 在类 java.lang.String 中找不到 main 方法
* */
```
上面代码是为了测试加载器的顺序：首先加载的是Bootstrap加载器，由于JVM中有java.lang.String这个类，所以会首先加载这个类，而不是自己写的类，而这个类中并无main方法，所以会报“在类 java.lang.String 中找不到 main 方法”。

这个问题就涉及到，如果有两个相同的类，那么java到底会用哪一个？如果使用用户自己定义的java.lang.String，那么别使用这个类的程序会去全部出错，所以，为了保证用户写的源代码不污染java出厂自带的源代码，而提供了一种“**双亲委派”机制，保证“沙箱安全”**。即先找到先使用。



### 沙箱安全机制

   Java安全模型的核心就是Java沙箱(sandbox) ,
   什么是沙箱? **沙箱是一个限制程序运行的环境。沙箱机制就是将Java代码限定在虚拟机(JVM)特定的运行范围中，并且严格限制代码对本地系统资源访问，通过这样的措施来保证对代码的有效隔离，防止对本地系统造成破坏。**
   沙箱主要限制系统资源访问，那系统资源包括什么? CPU、内存、文件系统、网络。不同级别的沙箱对这些资源访问的限制也可以不一样。
   所有的Java程序运行都可以指定沙箱，可以定制安全策略。

#### 组成沙箱的基本组件

●字节码校验器(bytecode verifier) :确保Java类文件遵循Java语言规范。这样可以帮助Java程序实现内存保护。但并不是所有的类文件都会经过字节码校验，比如核心类。
●类裝载器(class loader) :其中类装载器在3个方面对Java沙箱起作用
 	它防止恶意代码去干涉善意的代码;
 	它守护了被信任的类库边界;
 	它将代码归入保护域,确定了代码可以进行哪些操作。
   虚拟机为不同的类加载器载入的类提供不同的命名空间，命名空间由一系列唯一的名称组成， 每一个被装载的类将有一个名字，这个命名空间是由Java虚拟机为每一个类装载器维护的，它们互相之间甚至不可见。
   类装载器采用的机制是双亲委派模式。
   1.从最内层JVM自带类加载器开始加载,外层恶意同名类得不到加载从而无法使用;
   2.由于严格通过包来区分了访问域,外层恶意的类通过内置代码也无法获得权限访问到内层类，破  坏代码就自然无法生效。
●存取控制器(access controller) :存取控制器可以控制核心API对操作系统的存取权限，而这个控制的策略设定,可以由用户指定。
●安全管理器(security manager) : 是核心API和操作系统之间的主要接口。实现权限控制，比存取控制器优先级高。
●安全软件包(security package) : java.security下的类和扩展包下的类，允许用户为自己的应用增加新的安全特性，包括:
 	安全提供者
 	消息摘要
 	数字签名
 	加密
 	鉴别

---

## Native  本地

	native :凡是带了native关键字的，说明java的作用范围达不到了，回去调用底层c语言的库!
	会进入本地方法栈
	调用本地方法本地接口 JNI (Java Native Interface)
	JNI作用:开拓Java的使用，融合不同的编程语言为Java所用!最初: C、C++
	Java诞生的时候C、C++横行，想要立足，必须要有调用C、C++的程序
	它在内存区域中专门开辟了一块标记区域: Native Method Stack，登记native方法
	在最终执行的时候，加载本地方法库中的方法通过JNI
	例如：Java程序驱动打印机，管理系统，掌握即可，在企业级应用比较少
	private native void start0();
	//调用其他接口:Socket. . WebService~. .http~

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-e815940f813a7e28.png)
Thread类的start方法如下：

```java
public synchronized void start() {
        /**
         * This method is not invoked for the main method thread or "system"
         * group threads created/set up by the VM. Any new functionality added
         * to this method in the future may have to also be added to the VM.
         *
         * A zero status value corresponds to state "NEW".
         */
        if (threadStatus != 0)
            throw new IllegalThreadStateException();

        /* Notify the group that this thread is about to be started
         * so that it can be added to the group's list of threads
         * and the group's unstarted count can be decremented. */
        group.add(this);

        boolean started = false;
        try {
            start0();
            started = true;
        } finally {
            try {
                if (!started) {
                    group.threadStartFailed(this);
                }
            } catch (Throwable ignore) {
                /* do nothing. If start0 threw a Throwable then
                  it will be passed up the call stack */
            }
        }
    }

    private native void start0();
```
Thread类中竟然有一个只有声明没有实现的方法，并使用`native`关键字。用native表示，也此方法是系统级（底层操作系统或第三方C语言）的，而不是语言级的，java并不能对其进行操作。native方法装载在native method stack中。

### 本地方法栈

1.Java虚拟机栈用于管理Java方法的调用，而本地方法栈用于**管理本地方法**（一般非Java实现的方法）的调用

2.本地方法栈，也是**线程私有**的。

3.允许被实现成**固定或者是可动态拓展的内存大小**。（和Java虚拟机栈在内存溢出方面情况是相同的）

- 如果线程请求分配的栈容量超过本地方法栈允许的最大容量，Java虚拟机将会抛出一个StackOverFlowError异常。
- 如果本地方法栈可以动态扩展，并且在尝试扩展的时候无法申请到足够的内存，或者在创建新的线程时没有足够的内存去创建对应的本地方法栈，那么java虚拟机将会抛出一个OutOfMemoryError异常。

4.**本地方法是使用C语言实现**的

5.它的具体做法是Native Method Stack中登记native方法，在Execution Engine执行时加载本地方法库。

**6.当某个线程调用一个本地方法时，它就进入了一个全新的并且不再受虚拟机限制的世界。它和虚拟机拥有同样的权限**

- 本地方法可以通过本地方法接口来 **访问虚拟机内部的运行时数据区**
- 它甚至可以直接使用本地处理器中的寄存器
- 直接从本地内存的堆中分配任意数量的内存

7.并不是所有的JVM都支持本地方法。因为Java虚拟机规范并没有明确要求本地方法栈的使用语言、具体实现方式、数据结构等。如果JVM产品不打算支持native方法，也可以无需实现本地方法栈。

---

## PC寄存器

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-eb47cd3689d8fd02.png)

* 注意：**native方法不归java管，所以计数器是空的**

PC寄存器是用来存储指向下一条指令的地址，也就是即将将要执行的指令代码。由执行引擎读取下一条指令。

1.它是一块很小的内存空间，几乎可以忽略不计。也是运行速度最快的存储区域

2.在JVM规范中，每个线程都有它自己的程序计数器，是线程私有的，生命周期与线程的生命周期保持一致

3.任何时间一个线程都只有一个方法在执行，也就是所谓的**当前方法**。程序计数器会存储当前线程正在执行的java方法的JVM指令地址；或者，如果实在执行native方法，则是未指定值（undefined）,因为程序计数器不负责本地方法栈。

4.它是程序控制流的指示器，分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖这个计数器来完成

5.字节码解释器工作时就是通过改变这个计数器的值来选取吓一跳需要执行的字节码指令

6.它是唯一一个在java虚拟机规范中**没有规定任何OOM（Out Of Memery）情况**的区域,而且没有垃圾回收

> 利用javap -v xxx.class反编译字节码文件，查看指令等信息

![img](https://gitee.com/xudongyin/img/raw/master/img/1846149-20200401181046406-478634269.png)

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-2dfbe0fa4266a276.png)

>上面图中是亮色的地方有两个特点：
>* 1. **所有线程共享**（**灰色是线程私有**） 
>* 2. **亮色地方存在垃圾回收**



### PC寄存器面试常问

1.**使用PC寄存器存储字节码指令地址有什么用呢（为什么使用PC寄存器记录当前线程的执行地址呢）**

（1）多线程宏观上是并行（多个事件在同一时刻同时发生）的，但实际上是并发交替执行的

（2）因为CPU需要不停的切换各个线程，这时候切换回来以后，就得知道接着从哪开始继续执行

（3）JVM的字节码解释器就需要通过改变PC寄存器的值来明确下一条应该执行什么样的字节码指令

所以，众多线程在并发执行过程中，任何一个确定的时刻，一个处理器或者多核处理器中的一个内核，只会执行某个线程中的一条指令。这样必然导致经常中断或恢复，如何保证分毫无差呢？每个线程在创建后，都会产生自己的程序计数器和栈帧，程序计数器在各个线程之间互不影响。

2.**PC寄存器为什么会设定为线程私有？**

（1）我们都知道所谓的多线程在一个特定的时间段内只会执行其中某一个线程的方法，CPU会不停做任务切换，这样必然会导致经常中断或恢复，如何保证分毫无差呢？

（2）为了能够准确地记录各个线程正在执行的当前字节码指令地址，最好的办法自然是**为每一个线程都分配一个PC寄存器,**这样一来各个线程之间便可以进行独立计算，从而不会出现相互干扰的情况。

---

## 方法区

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-656d85ebbb170bdd.png)

>**注意：**
>
>* 方法区是被所有线程共享，所有字段和方法字节码，以及一些特殊方法，如构造函数,接口代码也在此定义,简单说，所有定义的方法的信息都保存在该区域,此区域属于共享区间;
>  **静态变量、常量、类信息(构造方法、接口定义、普通方法)、运行时的常量池存在方法区中，但是实例变量（普通变量）存在堆内存中，和方法区无关**
>* 方法区：绝对不是放方法的地方，他是存储的每一个类的结构信息(比如static)
>*  永久代和元空间的解释：
>方法区是一种规范，类似于接口定义的规范：`List list = new ArrayList();`
>把这种比喻用到方法区则有：
>  1. java 7中：`方法区 f = new 永久代();`
>  2. java 8中：`方法去 f = new 元空间();`

---

## 栈


![](https://gitee.com/xudongyin/img/raw/master/img/4070621-d0edbf860466dcb4.png)

>**注意：**
>* **栈是运行时的单位，堆是存储的单位。**
>
>  即，栈解决程序的运行问题，即程序如何执行，或者说如何处理数据。堆解决的是数据存储的问题，即数据怎么放，放在哪里。
>
>* 栈是线程私有，不存在垃圾回收
>
>* 虚拟机栈的生命周期同线程一致
>
>* 栈帧的概念：java中的方法被扔进虚拟机的栈空间之后就成为“栈帧”，比如main方法，是程序的入口，被压栈之后就成为栈帧。
>
>* 栈的作用：
>
>  **主管Java程序的运行，它保存方法的局部变量（8种基本数据类型、对象的引用地址）、部分结果，并参与方法的调用和返回。**

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-19b5b0443400e313.png)

### 栈运行原理

1. 不同线程所包含的栈帧是私有的，不允许存在相互引用，即**不可能在一个栈帧之中引用另一个线程的栈帧。**
2. 如果当前方法调用了其他方法，被调用方法返回之际，当前栈帧会传回此被调用方法的执行结果给前一个栈帧（调用者方法），接着，虚拟机会丢弃当前栈帧（被调用方法的栈帧），使得前一个栈帧重新成为当前栈帧。
3. Java方法有两种返回函数的方式 。一种是正常的函数返回，使用return指令，另一种是如果出现未经捕捉的异常，则已抛出异常的形式返回。不管使用哪种方式，都会导致栈帧被弹出。

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-ad996fd40b4f0d2b.png)



![](https://gitee.com/xudongyin/img/raw/master/img/4070621-1b59709fae10a4a1.png)

### 栈帧的内部结构

#### **1. 局部变量表**

![img](https://gitee.com/xudongyin/img/raw/master/img/20200528085309686.png)
    `StartPC：变量的作用域起始字节码指令位置，Length：作用域长度；StartPC+Length=总字节码指令数`

- 局部变量表也被称之为局部变量数组或者本地变量表。
- 参数值的存放总是在局部变量数组的index0开始，到数组长度-1的索引结束
- 定义为一个数字数组，主要用于存储方法参数和定义在方法体内的局部变量，**包括8种基本数据类型、对象引用以及returnAddress类型。**
- 由于局部变量表是建立在线程的栈上，**是线程的私有数据，因此不存在数据安全问题**。
- 局部变量表所需的**容量大小是在编译期确定下来的，并保存在方法的code属性的maximum local variables数据项中**。在方法运行期间是不会改变局部变量表的大小的。
- 方法嵌套调用的次数由栈的大小决定。一般来说，栈越大，方法嵌套调用次数越多。对于一个函数而言，它的**参数和局部变量越多，使得局部变量表膨胀，它的栈帧就越大**，以满足方法调用所需传递的信息增大的需求，进而函数调用就会占用更多的栈空间，导致其嵌套调用次数就会减少。
- **局部变量表中的变量只在当前方法调用中有效**。在方法执行时，虚拟机通过使用局部变量表完成参数值到参数变量列表的传递过程。当方法调用结束后，局部变量表随着方法栈帧的销毁而销毁。
- **局部变量表最基本的存储单元是Slot（变量槽）**，其中，32位以内的类型只占一个slot(包括returnAddress类型)，64位的类型（long和double占用两个slot）；byte、short、char在存储前被转换为int，boolean也被转化为int，0表示false,非0表示true。

<img src="https://gitee.com/xudongyin/img/raw/master/img/1846149-20200401213601820-1712399995.png" alt="img" style="zoom:50%;" />

- JVM会为每一个slot都分配一个访问索引，通过这个索引即可成功访问到局部变量表中指定的局部变量值，**如果要访问一个64bit的局部变量值时，只需要使用两个slot中的第一个slot的索引即可**。**如果当前栈帧是由构造方法或者实例方法创建的，那么该对象引用this将会放在index=0的slot处**，其余的参数按照参数表顺序继续排列，这也就解释了**为什么静态方法中不可以引用this，因为this变量（当前对象的引用）不存在于静态方法的局部变量表中**。此外，**栈帧中的局部变量表中的槽位是可以重用的**，如果一个局部变量过了其作用域，那么在其作用域之后申明的新的局部变量就很有可能会复用过期局部变量的槽位，从而达到节省资源的目的。

![img](https://gitee.com/xudongyin/img/raw/master/img/20200528094440916.png)

- 在**栈帧中，与性能调优关系最为密切的部分就是局部变量表**。在方法执行时，虚拟机使用局部变量表完成方法的传递。局部变量表中的变量也是重要的垃圾回收的根节点，只要被局部变量表中直接或间接引用的对象都不会被回收。

> **静态变量与局部变量的对比及小结**

变量的分类：

- 按照数据类型分：

  - ①基本数据类型;
  - ②引用数据类型；

- 按照在类中声明的位置分：

  - ①成员变量：在使用前，都经历过默认初始化赋值
    - static修饰：类变量：类加载链接的准备preparation阶段给类变量默认赋0值——>初始化阶段initialization给类变量显式赋值即静态代码块赋值；
    - 不被static修饰：实例变量：随着对象的创建，会在堆空间分配实例变量空间，并进行默认赋值
  - ②局部变量：在使用前，必须要进行显式赋值的！否则，编译不通过

  

#### **2. 操作数栈**

每一个独立的栈帧除了包含局部变量表以外，还包含一个后进先出的操作数栈，在方法执行过程中，根据字节码指令，往操作数栈中**写入数据或提取数据，即操作数栈的入栈/出栈**。例如某些字节码指令将值压入操作数栈，其余的字节码指令将操作数取出栈，使用他们后再把结果压入栈。（如字节码指令bipush操作）

比如：执行复制、交换、求和等操作

如果说Java虚拟机的解释引擎是基于栈的执行引擎，其中栈指的就是操作数栈。

- 操作数栈**主要用于保存计算过程中的中间结果，同时作为计算过程中的变量临时的存储空间**。

- 操作数栈就是JVM执行引擎的一个工作区，当**一个方法刚开始执行的时候**，一个新的栈帧也会随之被创建出来，这个方法的**操作数栈是空的**。
- 每一个操作数栈都会拥有一个明确的栈深度用于存储数值，**其所需的最大深度在编译期就定义好了，保存在方法的code属性中，为max_stack的值**。
- **栈中任何一个元素都是可以任意的Java数据类型**，其中，32bit类型占用一个栈单位深度，64bit类型占用两个。
- 操作数栈不同于局部变量表，并非采用访问索引的方式来进行数据访问，而是**通过标准的入栈出栈操作来完成一次数据访问。**
- **如果被调用的方法带有返回值的话，其返回值将会被压入当前栈帧的操作数栈中**，并更新PC寄存器中下一条需要执行的字节码指令。

> **操作数栈代码追踪**

结合上图结合下面的图来看一下一个方法（栈帧）的执行过程

①15入栈；②存储15，15进入局部变量表

注意：局部变量表的0号位被构造器占用，这里的15从局部变量表1号开始

![img](https://gitee.com/xudongyin/img/raw/master/img/1846149-20200402100253935-1789842330.png)

 

③压入8；④8出栈，存储8进入局部变量表；

![img](https://gitee.com/xudongyin/img/raw/master/img/1846149-20200402100334236-1637713126.png)

 

⑤从局部变量表中把索引为1和2的是数据取出来，放到操作数栈；⑥iadd相加操作

![img](https://gitee.com/xudongyin/img/raw/master/img/1846149-20200402100351272-751766212.png)

 

⑦iadd操作结果23出栈⑧将23存储在局部变量表索引为3的位置上istore_3

![img](https://gitee.com/xudongyin/img/raw/master/img/1846149-20200402100449299-194786867.png)

> **栈顶缓存技术ToS（Top-of-Stack Cashing）**

- 基于栈式架构的虚拟机所使用的零地址指令（即不考虑地址，单纯入栈出栈）更加紧凑，但完成一项操作的时候必然需要使用更多的入栈和出栈指令，这同时也就意味着将需要更多的指令分派（instruction dispatch）次数和内存读/写次数
- 由于操作数是存储在内存中的，因此频繁地执行内存读/写操作必然会影响执行速度。为了解决这个问题，HotSpot JVM的设计者们提出了栈顶缓存技术，**将栈顶元素全部缓存在物理CPU的寄存器中，以此降低对内存的读/写次数，提升执行引擎的执行效率**



#### **3.动态链接（或指向运行时常量池的方法引用）**

> 运行时常量池位于方法区（注意： JDK1.7 及之后版本的 JVM 已经将运行时常量池从方法区中移了出来，在 Java 堆（Heap）中开辟了一块区域存放运行时常量池。）

​		每一个栈帧内部都包含一个指向运行时常量池（运行时常量池是在方法区里的）中该栈帧所属方法的引用。这个引用的目的就是为了支持当前方法的代码能够实现动态链接。比如：invokedynamic指令。

​		在Java源文件被编译到字节码文件中时，所有的变量和方法引用都作为符号引用保存在class文件的常量池里。比如：描述一个方法调用了另外的其它方法时，就是通过常量池中指向方法的符号引用来表示的，那么**动态链接的作用就是为了将这些符号引用转换为调用方法的直接引用**。

​		常量池的作用就是为了提供一些符号和常量以便于指令的识别，以节约内存。

![img](https://gitee.com/xudongyin/img/raw/master/img/1846149-20200402110625116-1337433158.png)

> **方法的调用**

在JVM中，将符号引用转换为调用方法的直接引用与方法的绑定机制相关

- **静态链接**
  当一个 字节码文件被装载进JVM内部时，如果被调用的目标方法在编译期可知，且运行期保持不变时。这种情况下**将调用方法的符号引用转换为直接引用的过程称之为静态链接**。
- **动态链接**
  如果被调用的方法在编译期无法被确定下来，也就是说，只能够**在程序运行期将调用方法的符号引用转换为直接引用**，由于这种引用转换过程具备动态性，因此也就被称之为动态链接。

对应的方法的绑定机制为：早期绑定（Early Binding）和晚期绑定（Late Bingding）。**绑定是一个字段、方法或者类在符号引用被替换为直接引用的过程，这仅仅发生一次。**

- **早期绑定**
  早期绑定就是指被调用的目标方法如果在编译期可知，且运行期保持不变时，即可将这个方法与所属的类型进行绑定，这样一来，由于明确了被调用的目标方法究竟是哪一个，因此也就可以使用静态链接的方式将符号引用转换为直接引用。
- **晚期绑定**
  如果被调用的方法在编译期无法被确定下来，只能够在程序运行期根据实际的类型绑定相关的方法，这种绑定方式也就被称之为晚期绑定。

随着高级语言的横空出世，类似于java一样的基于面向对象的编程语言如今越来越多，尽管这类编程语言在语法风格上存在一定的差别，但是它们彼此之间始终保持着一个**共性，那就是都支持封装，集成和多态等面向对象特性**，既然这一类的编程语言具备多态特性，那么自然也就具备早期绑定和晚期绑定两种绑定方式。
Java中任何一个普通的方法其实都具备虚函数的特征，它们相当于C++语言中的虚函数（C++中则需要使用关键字virtual来显式定义）。如果在Java程序中**不希望某个方法拥有虚函数的特征**时，则可以**使用关键字final来标记这个方法**。

> **虚方法和非虚方法**

子类对象的多态性使用前提：
①类的继承关系（父类的声明）②方法的重写（子类的实现）

实际开发编写代码中用的接口，实际执行是导入的的三方jar包已经实现的功能

**非虚方法**

- 如果方法在编译器就确定了具体的调用版本，这个版本在**运行时是不可变的**。这样的方法称为**非虚方法**
- **静态方法、私有方法、final方法、实例构造器（实例已经确定，this()表示本类的构造器）、父类方法（super调用）都是非虚方法**

**虚方法**

- 其他所有体现多态特性的方法称为**虚方法**

> **虚拟机中提供了以下几条方法调用指令**

普通调用指令：
1**.invokestatic**：调用静态方法，解析阶段确定唯一方法版本；
2**.invokespecial**:调用<init>方法、私有及父类方法，解析阶段确定唯一方法版本；
3.**invokevirtual**调用所有虚方法；
4.**invokeinterface**：调用接口方法；
动态调用指令（Java7新增）：
5.**invokedynamic**：动态解析出需要调用的方法，然后执行 .
前四条指令固化在虚拟机内部，方法的调用执行不可人为干预，而invokedynamic指令则支持由用户确定方法版本。

**其中invokestatic指令和invokespecial指令调用的方法称为非虚方法**

**其中invokevirtual（final修饰的除外，JVM会把final方法调用也归为invokevirtual指令，但要注意final方法调用不是虚方法）invokeinterface指令调用的方法称称为虚方法。**

~~~java
/**
 * 解析调用中非虚方法、虚方法的测试
 */
class Father {
    public Father(){
        System.out.println("Father默认构造器");
    }

    public static void showStatic(String s){
        System.out.println("Father show static"+s);
    }

    public final void showFinal(){
        System.out.println("Father show final");
    }

    public void showCommon(){
        System.out.println("Father show common");
    }

}

public class Son extends Father{
    public Son(){
        super();
    }

    public Son(int age){
        this();
    }

    public static void main(String[] args) {
        Son son = new Son();
        son.show();
    }

    //不是重写的父类方法，因为静态方法不能被重写
    public static void showStatic(String s){
        System.out.println("Son show static"+s);
    }

    private void showPrivate(String s){
        System.out.println("Son show private"+s);
    }

    public void show(){
        //invokestatic
        showStatic(" 大头儿子");
        //invokestatic
        super.showStatic(" 大头儿子");
        //invokespecial
        showPrivate(" hello!");
        //invokespecial
        super.showCommon();
        //invokevirtual 因为此方法声明有final 不能被子类重写，所以也认为该方法是非虚方法
        showFinal();
        //虚方法如下
        //invokevirtual
        showCommon();//没有显式加super，被认为是虚方法，因为子类可能重写showCommon
        info();

        MethodInterface in = null;
        //invokeinterface  不确定接口实现类是哪一个 需要重写
        in.methodA();

    }

    public void info(){

    }

}

interface MethodInterface {
    void methodA();
}
~~~

> **关于invokedynamic指令**

- JVM字节码指令集一直比较稳定，一直到java7才增加了一个invokedynamic指令，这是**Java为了实现【动态类型语言】支持而做的一种改进**
- 但是java7中并没有提供直接生成invokedynamic指令的方法，需要借助ASM这种底层字节码工具来产生invokedynamic指令，**直到Java8的Lambda表达式的出现，invokedynamic指令的生成，在java中才有了直接生成方式**
- Java7中增加的动态语言类型支持的本质是对java虚拟机规范的修改，而不是对java语言规则的修改，这一块相对来讲比较复杂，增加了虚拟机中的方法调用，最直接的受益者就是运行在java平台的动态语言的编译器

> **动态类型语言和静态类型语言**

- 动态类型语言和静态类型语言**两者的区别就在于对类型的检查是在编译期还是在运行期，满足前者就是静态类型语言，反之则是动态类型语言**。
- 直白来说 **静态语言是判断变量自身的类型信息；动态类型语言是判断变量值的类型信息，变量没有类型信息，变量值才有类型信息**，这是动态语言的一个重要特征
- Java是静态类型语言（尽管lambda表达式为其增加了动态特性），js，python是动态类型语言.

```
Java：String info = "硅谷";//静态语言

JS：var name = "硅谷“；var name = 10;//动态语言

Pythom: info = 130;//更加彻底的动态语言
```

> **方法重写的本质**

1. 找到操作数栈的第一个元素所执行的对象的实际类型，记作C。
2. 如果在类型C中找到与常量池中的描述符、简单名称都相符的方法，则进行访问权限校验，如果通过则返回这个方法的直接引用，查找过程结束；如果不通过，则返回java.lang.IllegalAccessError异常。**IllegalAccessError介绍** 程序视图访问或修改一个属性或调用一个方法，这个属性或方法，你没有权限访问。一般的，这个会引起编译器异常。这个错误如果发生在运行时，就说明一个类发生了不兼容的改变。
3. 否则，按照继承关系从下往上依次对c的各个父类进行第二步的搜索和验证过程。
4. 如果始终没有找到合适的方法，则抛出java.lang.AbstractMethodError异常。

> **虚方法表**

- 在面向对象编程中，会很频繁期使用到动态分派，如果在每次动态分派的过程中都要重新在类的方法元数据中搜索合适的目标的话就可能影响到执行效率。因此，为了提高性能，jvm采用在类的方法区建立一个虚方法表（virtual method table）（非虚方法不会出现在表中）来实现。使用索引表来代替查找。
- 每个类中都有一个虚方法表，表中存放着各个方法的实际入口。
- 那么虚方法表什么时候被创建？ 虚方法表会在类加载的链接阶段被创建并开始初始化，类的变量初始值准备完成之后，jvm会把该类的虚方法表也初始化完毕。



#### **4.方法返回地址（或方法正常退出或者异常退出的定义）**

​		存放调用该方法得到PC寄存器的值

​		一个方法的结束，有正常执行完成和出现未处理的异常从而非正常退出两种方式，无论通过哪种方式退出，在方法退出后都返回到该方法被调用的位置。**方法正常退出时，调用者的PC计数器的值作为返回地址，即调用该方法的指令的下一条地址。如果是异常退出的，返回地址是要通过异常表来确定的，栈帧中一般不会保存这部分的信息。**

​		本质上，方法的退出就是当前栈帧出栈的过程。此时，需要恢复上层方法的局部变量表、操作数栈、将返回值压入调用者栈帧的操作数栈、设置PC寄存器值等，让调用者方法继续执行下去。

​		正常完成出口和异常完成出口的区别在于：**通过异常完成出口退出的不会给他的上层调用者产生任何的返回值。**



#### **5.一些附加信息**

栈帧中还允许携带与java虚拟机实现相关的一些附加信息。例如，对程序调试提供支持的信息。（很多资料都忽略了附加信息）



> **开发过程中，关于虚拟机栈可能的异常？**

Java虚拟机规范允许**Java栈的大小是动态的或者是固定不变的**。

- 如果采用**固定大小**的虚拟机栈，拿每一个线程的Java虚拟机栈容量可以再线程创建的时候独立选定。如果线程请求分配的栈容量超过Java虚拟机栈允许的最大容量，Java虚拟机会抛出一个**StackOverflowError**的异常。在开发过程中，不合理的递归方法就会导致这个问题。
- 如果Java虚拟机栈可以**动态扩展**，并且在尝试扩展的时候无法申请到足够的内存，或者在创建新的线程时没有足够的内存去创建对应的虚拟机栈，那Java虚拟机将会抛出一个**OutOfMerroyError**异常。

```java
public class  Test{

    public static  void  m(){
        m();
    }

    public static void main(String[] args) {

        System.out.println("111");
        //Exception in thread "main" java.lang.StackOverflowError
        m();
        System.out.println("222");

    }
}

/*
*output:
* 111
* Exception in thread "main" java.lang.StackOverflowError
* */
```
>**注意：**
>* StackOverflowError是一个**“错误”**，而不是**“异常”**。
>![](https://gitee.com/xudongyin/img/raw/master/img/4070621-5df3029ee7dff3d3.png)
>* 关于Error我们再多说一点，上面的讨论不涉及Exception
>  * 首先Exception和Error都是继承于Throwable 类，在 Java 中只有 Throwable 类型的实例才可以被抛出（throw）或者捕获（catch），它是异常处理机制的基本组成类型。
>  * Exception和Error体现了JAVA这门语言对于异常处理的两种方式。
>  * Exception是java程序运行中可预料的异常情况，咱们可以获取到这种异常，并且对这种异常进行业务外的处理。
>  * Error是java程序运行中不可预料的异常情况，这种异常发生以后，会直接导致JVM不可处理或者不可恢复的情况。所以这种异常不可能抓取到，比如OutOfMemoryError、NoClassDefFoundError等。
>  * 其中的Exception又分为检查性异常和非检查性异常。两个根本的区别在于，检查性异常 必须在编写代码时，使用try catch捕获（比如：IOException异常）。非检查性异常 在代码编写使，可以忽略捕获操作（比如：ArrayIndexOutOfBoundsException），这种异常是在代码编写或者使用过程中通过规范可以避免发生的。



### 虚拟机栈的相关面试题

1.举例栈溢出的情况？（StackOverflowError）

- 递归调用等，通过-Xss设置栈的大小；

2.调整栈的大小，就能保证不出现溢出么？

- 不能 如递归无限次数肯定会溢出，调整栈大小只能保证溢出的时间晚一些，极限情况会导致OOM内存溢出（Out Of Memery Error）注意是Error

3.分配的栈内存越大越好么？

- 不是 ，会挤占其他线程的空间

4.垃圾回收是否会涉及到虚拟机栈？

- 不会

5.方法中定义的局部变量是否线程安全？

- 具体问题具体分析，如果是线程内部产生内部消亡的，那一定是线程安全的，如果是外部传入或者是要返回到外部的局部变量，是线程不安全的，就是要**注意局部变量的生命周期**。



![](https://gitee.com/xudongyin/img/raw/master/img/4070621-5bcc19f55683b59d.png)
>**注意：**
>* HotSpot：如果没有明确指明，JDK的名字就叫HotSpot
>![](https://gitee.com/xudongyin/img/raw/master/img/4070621-3be86c30f479e979.png)
>* 元数据：描述数据的数据（即模板，也就是“大Class”）
>上面的关系图的一个实例为下图：
>![](https://gitee.com/xudongyin/img/raw/master/img/4070621-1e9d4eb5deff6083.png)

---

## 堆

> **核心概述**

一个JVM实例值存在一个堆内存，堆也是Java内存管理的核心区域。

Java堆区在JVM启动的时候就被创建了，其空间大小也就确定了，是JVM管理的**最大一块内存空间**。《Java虚拟机规范》规定，堆可以处于物理上不连续的内存空间中，但在逻辑上它应该被视为连续的。所有的线程共享Java堆，在这里还可以划分线程私有的缓冲区（TLAB，Thread Local Allocation Buffer）。《Java虚拟机规范》中对Java堆的描述是：**所有的对象实例以及数组都应当在运行时分配在堆上（但并不是全部）**。数组和对象可能永远不会存储在栈上，因为**栈帧中保存引用，这个引用指向对象或者数组在堆中的位置**。

在方法结束后，堆中的对象不会马上被移除，仅仅在垃圾收集的时候才会被移除。**堆，是GC（垃圾收集器）执行垃圾回收的重点区域**。

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-a3d31ee101600821.png)




![](https://gitee.com/xudongyin/img/raw/master/img/4070621-1926dd216ce9e26c.png)
>**注意：**
>* **Java 7**之前和图上一模一样，**Java 8**把**永久区**换成了**元空间**（方法区）
>* **堆逻辑上由”新生+养老+元空间“三个部分组成，物理上由”新生+养老“两个部分组成**
>* 当执行`new Person()；`时，其实是new在新生区的伊甸园区，然后往下走，走到养老区，但是并未到元空间。
>* 虽然说，逻辑上是将堆空间划分为新生代、老年代和永久代三部分，实际上是不考虑永久代（也就是Java8中提到的元空间）的，可以将其看做是**方法区的落地实现**。


![](https://gitee.com/xudongyin/img/raw/master/img/4070621-1926dd216ce9e26c.png)

>**注意：**
>* GC发生在伊甸园区，当对象快占满新生代时，就会发生YGC（Young GC，轻量级GC）操作，伊甸园区基本全部清空
>* 幸存者0区(S0)，别名“from区”。伊甸园区没有被YGC清空的对象将移至幸存者0区，幸存者1区别名“to 区”
>* 每次进行YGC操作，幸存的对象就会从伊甸园区移到幸存者0区，如果幸存者0区满了，就会继续往下移，如果经历数次YGC操作对象还没有消亡，最终会来到养老区
>* 如果到最后，养老区也满了，那么就对养老区进行FGC(Full GC，重GC)，对养老区进行清洗  
>* 如果进行了多次FGC之后，还是无法腾出养老区的空间，就会报**OOM（out of Memory）**异常
>* **from区和to区位置和名分不是固定的，每次GC过后都会交换，GC交换后，谁空谁是to区**
>* **大对象直接进入养老区**，大对象就是需要大量连续内存空间的对象（比如：字符串、数组），为了**避免为大对象分配内存时由于分配担保机制带来的复制而降低效率**。


![](https://gitee.com/xudongyin/img/raw/master/img/4070621-45ec61c8f5d1c887.png)

>**注意：**
>* 整个堆分为新生区和养老区，新生区占整个堆的1/3，养老区占2/3。新生区又分为3份：伊甸园区：幸存者0区(from区):幸存者1区(to区) = 8:1:1
>
>* **每次从伊甸园区经过GC幸存的对象，年龄(代数)会+1**
>
>* `-XX:MaxTenuringThreshold=15`调整多少代进入老年区
>
>* 关于默认的晋升年龄是15，这个说法的来源大部分都是《深入理解Java虚拟机》这本书。 如果你去Oracle的官网阅读[相关的虚拟机参数](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/java.html)，你会发现`-XX:MaxTenuringThreshold=threshold`这里有个说明
>
>  Sets the maximum tenuring threshold for use in adaptive GC sizing. The largest value is 15. The default value is 15 for the parallel (throughput) collector, and 6 for the CMS collector.
>
>  默认晋升年龄并不都是15，这个是要区分垃圾收集器的，CMS就是6.

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-df06084e11c11870.png)

> **堆空间的分代思想**

**为什么需要把Java堆分代？不分代就不能正常工作了吗？**

经研究，不同对象的生命周期不同，70%-90%的对象是临时对象。

![img](https://gitee.com/xudongyin/img/raw/master/img/20200608230458772.png)



### TLAB（Thread Local Allocation Buffer）

我们知道，堆区是线程共享区域，任何线程都可以访问到堆区的共享数据，由于对象实例的创建在JVM中非常频繁，因此在并发环境下从堆区中划分内存空间是线程不安全的。为了避免多个线程操作同一地址，需要使用加锁等机制，进而影响分配速度，TLAB就是为了解决这一问题。

> 什么是TLAB？

从内存模型而不是垃圾收集的角度，对Eden区域继续进行划分，**JVM为每个线程分配一个私有缓存区域，它包含在Eden空间内**。

![img](https://gitee.com/xudongyin/img/raw/master/img/20200609093234420.png)

多线程同时分配内存时，使用TLAB可以避免一系列的非线程安全问题，同时还能够提升内存分配的吞吐量，因此我们可以将这种内存分配方式称之为**快速分配策略**。

- 尽管不是所有的对象实例都能够在TLAB中成功分配内存，但是JVM确实是将TLAB作为内存分配的首选。
- 在程序中，开发人员通过"-XX:UseTLAB"设置是否开启TLAB空间（可以通过 `jinfo -flag UseTLAB 线程号` 查询是否开启）。
- 默认情况下，TLAB空间的内存非常小，仅占有整个Eden空间的１％，可以通过选项`-XX:TLABWasteTargetPercent`设置TLAB空间所占用Eden空间的百分比大小。
- 一旦对象在TLAB空间分配内存失败时，JVM就会尝试着通过使用加锁机制确保数据操作的原子性，从而直接在Eden空间中分配内存。

![img](https://gitee.com/xudongyin/img/raw/master/img/20200609095046879.png)

​      												`加入TLAB后的对象分配过程`

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-ede6cec97ade9a23.png)


>**注意：**
>* 临时对象就是说明，其在伊甸园区生，也在伊甸园区死。
>* **堆逻辑上由”新生+养老+元空间“三个部分组成，物理上由”新生+养老“两个部分组成，元空间也叫方法区**
>* 永久代(方法区)几乎没有垃圾回收，里面存放的都是加载的rt.jar等，让你随时可用

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-fa575fe14562f74a.png)

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-cb6e0dffd5a7106b.png)


![](https://gitee.com/xudongyin/img/raw/master/img/4070621-539c3eed7284a5bb.png)

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-db894c5dc43cc231.png)

>**注意**
>* 上面的图展示的是物理上的堆，分为两块，新生区和养老区。
>* 堆的参数主要有两个：`-Xms`，`Xmx`：
>  1. `-Xms`堆的初始化的内存大小
>  2. `-Xmx`堆的最大内存
>* Young Gen(新生代)有一个参数`-Xmn`，这个参数可以调新生区和养老区的比例。但是，这个参数一般不调。
>* 永久代也有两个参数：`-XX:PermSize`，`-XX:MaxPermSize`，可以分别调永久代的初始值和最大值。**Java 8 后没有这两个参数啦**，因为**Java 8后元空间不在虚拟机内啦，而是在本机物理内存中**


![](https://gitee.com/xudongyin/img/raw/master/img/4070621-dff0b5b2865e1067.png)

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-8901fb1813c06fa5.png)

```java
//查看自己机器上的默认堆内存和最大堆内存
public class  Test{

    public static void main(String[] args) {
        System.out.println(Runtime.getRuntime().availableProcessors());
        //返回 Java虚拟机试图使用的最大内存量。物理内存的1/4（-Xmx）
        long maxMemory = Runtime.getRuntime().maxMemory() ;
        //返回 Java虚拟机中的内存总量(初始值)。物理内存的1/64（-Xms）
        long totalMemory = Runtime.getRuntime().totalMemory() ;
        System.out.println("MAX_MEMORY =" + maxMemory +"(字节)、" + (maxMemory / (double)1024 / 1024) + "MB");
        System.out.println("DEFALUT_MEMORY = " + totalMemory + " (字节)、" + (totalMemory / (double)1024 / 1024) + "MB");

    }
}

/*
*   8
    MAX_MEMORY =1868038144(字节)、1781.5MB
    TOTAL_MEMORY = 126877696 (字节)、121.0MB
* */
```

>* **注意：**JVM参数调优，平时可以随便挑初始大小和最大大小，但是**实际工作中，初始大小和最大大小应该是一致的，原因是为了能够在java垃圾回收机制清理完堆区后不需要重新分隔计算堆区的大小，从而提高性能。避免内存忽高忽低产生停顿**
>* IDEA 的JVM内存配置
>   1. 点击Run列表下的Edit Configuration
>   ![](https://gitee.com/xudongyin/img/raw/master/img/4070621-35de395dfee6c3e2.png)
>   2. 在VM Options中输入以下参数:`-Xms1024m  -Xmx1024m  -XX:+PrintGCDetails`。
>   ![](https://gitee.com/xudongyin/img/raw/master/img/4070621-3792f8476ec10953.png)
>    3. 运行程序查看结果
>    ![](https://gitee.com/xudongyin/img/raw/master/img/4070621-8eaf64157a3b1ce1.png)
>* 把堆内存调成10M后，再一直new对象，导致Full GC也无法处理，直至撑爆堆内存，查看堆溢出错误(OOM)，程序及结果如下：
>   ![](https://gitee.com/xudongyin/img/raw/master/img/4070621-b7c7631f058cb3eb.png)
>![](https://gitee.com/xudongyin/img/raw/master/img/4070621-58e8e307ddbeef5c.png)
>![](https://gitee.com/xudongyin/img/raw/master/img/4070621-098f475b3ef94c31.png)

>**GC收集日志信息详解**
>* 第一次进行YGC相关参数：
>  [PSYoungGen: 2008K->482K(2560K)] 2008K->782K(9728K), 0.0011440 secs]  [Times: user=0.00 sys=0.00, real=0.00 secs]
>![](https://gitee.com/xudongyin/img/raw/master/img/4070621-f9fdd483c3983a8f.png)
>* 最后一次进行FGC相关参数：
>[Full GC (Allocation Failure) [PSYoungGen: 0K->0K(2048K)] [ParOldGen: 4025K->4005K(7168K)] 4025K->4005K(9216K), [Metaspace: 3289K->3289K(1056768K)], 0.0082055 secs] [Times: user=0.00 sys=0.00, real=0.01 secs] 
>![](https://gitee.com/xudongyin/img/raw/master/img/4070621-900f4e7a0826e16b.png)
>![](https://gitee.com/xudongyin/img/raw/master/img/4070621-cc943f01079a4552.png)

>**面试题：GC是什么（分代收集算法）**
> * 次数上频繁收集Young区
>* 次数上较少收集Old区
>* 基本不动元空间
>
>**面试题：GC的四大算法（后有详解）**
>* 1. 引用计数法
>* 2. 复制算法(Copying)
>* 3. 标记清除(Mark-Sweep)
>* 4. 标记压缩(Mark-Compact)
>
>**面试题：下面程序中，有几个线程在运行**
>![](https://gitee.com/xudongyin/img/raw/master/img/4070621-8539dbb83f6556a0.png)
> Answer:有两个线程，一个是main线程，一个是后台的gc线程。


![GC算法概述](https://gitee.com/xudongyin/img/raw/master/img/4070621-98170629a0965926.png)

>**知识点：**
>* JVM在进行GC时，并非每次都对上面三个内存区域一起回收的，大部分时候回收的都是指新生代。因此GC按照回收的区域又分了两种类型，一种是普通GC（minor GC or Young GC），一种是全局GC（major GC or Full GC）
>* Minor GC和Full GC的区别
>　　* 普通GC（minor GC）：只针对新生代区域的GC,指发生在新生代的垃圾收集动作，因为大多数Java对象存活率都不高，所以Minor GC非常频繁，一般回收速度也比较快。
>*  全局GC（major GC or Full GC）：指发生在老年代的垃圾收集动作，出现了Major GC，经常会伴随至少一次的Minor GC（但并不是绝对的）。Major GC的速度一般要比Minor GC慢上10倍以上 (因为养老区比较大，占堆的2/3)

### 对象实例化

![image-20200718224459831](https://gitee.com/xudongyin/img/raw/master/img/image-20200718224459831.png)

从创建对象的执行步骤来分析 对象的创建过程：

1. **判断对象对应的类是否加载、链接、初始化。**虚拟机遇到一条new指令，首先去检查这个指令的参数能否在Metaspace的常量池中定位到一个类的符号引用，并且检查这个符号引用代表的类是否已经被加载、解析和初始化（即判断类元信息是否存在）。如果没有，那么在双亲委派模式下，使用当前类加载器以ClassLoader+包名+类名为key进行查找对应的.class文件。如果没有找到文件，则抛出ClassNotFoundException异常，如果找到，则进行类加载，并生成对应的Class类对象。
2. **为对象分配内存。**首先计算对象占用空间大小，接着在堆中划分一块内存给新对象，如果实例成员变量是引用变量，仅分配引用变量空间即可，即4个字节大小。如果内存是规整的，那么虚拟机将采用指针碰撞法来为对象分配内存。假设java堆中内存是绝对规整的，所有用过的内存放一边，未使用过的放一边，中间有一个指针作为临界点，如果新创建了一个对象则是把指针往未分配的内存挪动与对象内存大小相同距离，这个称为指针碰撞。如果垃圾收集器选择的是Serial、ParNew这种基于压缩算法的，虚拟机采用这种分配方式。一般使用带有整理过程的收集器时使用指针碰撞；如果内存不规整，则使用空闲列表法（Free List）。事实上，Java堆的内存并不是完整的，已分配的内存和空闲内存相互交错，JVM通过维护一个列表，记录可用的内存块信息，当分配操作发生时，从列表中找到一个足够大的内存块分配给对象实例，并更新列表上的记录。使用的GC收集器：CMS，适用堆内存不规整的情况下。
3. **处理并发安全问题。**在创建对象的时候有一个很重要的问题，就是线程安全，因为在实际开发过程中，创建对象是很频繁的事情，作为虚拟机来说，必须要保证线程是安全的，通常来讲，虚拟机采用两种方式来保证线程安全：一是采用CAS， CAS 是乐观锁的一种实现方式。所谓乐观锁就是每次不加锁，而是假设没有冲突而去完成某项操作，如果因为冲突失败，就重试，直到成功为止。虚拟机采用 CAS 配上失败重试的方式保证更新操作的原子性；二是为每个线程预先分配一块TLAB——通过-XX:+/-UseTLAB参数来设定（JDK8及之后默认开启），为每一个线程预先分配一块内存，JVM在给线程中的对象分配内存时，首先在TLAB分配，当对象大于TLAB中的剩余内存或TLAB的内存已用尽时，再采用上述的CAS进行内存分配。
4. **初始化分配到的空间。**所有属性设置默认值，保证对象实例字段在不赋值时可以直接使用。（这里要区别一下类加载过程的准备阶段）
5. **设置对象的对象头。**将对象的所属类（即类的元数据信息）、对象的HashCode和对象的GC信息、锁信息等数据存储在对象的对象头中。这个过程的具体设置方式取决于JVM的实现。
6. **执行init方法进行初始化。**在Java程序的视角看来，初始化才正式开始。初始化成员变量，执行实例化代码块，调用类的构造方法，并把堆内对象的首地址赋值给引用变量。因此一般来说（由字节码中是否跟随有invokespecial指令所决定），new指令之后会接着就是执行方法，把对象按照程序员的意愿进行初始化，这样一个真正可用的对象才算完全创建出来。

![img](https://gitee.com/xudongyin/img/raw/master/img/2020061720145670.png)

### 对象的访问定位

![img](https://gitee.com/xudongyin/img/raw/master/img/20200618084457645.png)

一、句柄访问。虚拟机栈的局部变量表中记录了对象引用，指向堆空间中对应的句柄，句柄位于Java堆空间的句柄池中，**一个句柄包含两个指针，分别是到堆空间的实例池的对应对象实例数据的指针和到方法区的对象类型数据的指针。**

<img src="https://gitee.com/xudongyin/img/raw/master/img/image-20200718230144448.png" alt="image-20200718230144448" style="zoom: 67%;" />

二、直接指针，虚拟机栈的**局部变量表中记录的对象引用直接指向了对象实例数据**，而在**对象实例数据中有一个到对象类型数据的指针，指向方法区中相应的对象类型数据。HotSpot采用的就是这种直接指针法。**

<img src="https://gitee.com/xudongyin/img/raw/master/img/image-20200718230201770.png" alt="image-20200718230201770" style="zoom:67%;" />

三、两者比较。两种访问定位方式各有优劣，一方面，很明显**直接指针法要比句柄访问的效率高一些**，另一方面，对于句柄访问而言，reference中存储稳定的句柄地址，**对象被移动（垃圾收集时移动对象很普遍）时只需改变句柄中实例数据指针即可，reference本身不需要被修改，而对于直接指针而言，对象移动的话reference也要修改。**



### 逃逸分析-

堆是分配对象存储的唯一选择吗？

随着JIT编译器的发展与逃逸分析技术的逐渐成熟，栈上分配、标量替换优化技术将会导致一些微妙的变化，所有的对象都分配到堆上也渐渐变得不那么“绝对”了。

在Java虚拟机中，对象是在Java堆中分配内存的，这是一个普遍的常识，但是，有一种特殊的情况，就是如果经过逃逸分析后发现，**一个对象并没有逃逸出方法的话，那么就有可能被优化成栈上分配**。这样就无需在堆上分配内存，也无需进行垃圾回收了。这也是常见的**堆外存储技术**。

此外，前面提到的基于OpenJDK深度定制的TaoBaoVM，其中创新的GCIH（GC invisible heap）技术实现off-heap，将生命周期较长的Java对象从heap中移到heap外，并且GC 不能管理GCIH内部的Java对象，以此达到降低GC回收频率跟提升GC的回收效率的目的。

> 逃逸分析

如何将堆上的对象分配到栈，需要使用**逃逸分析手段**。

**这是一种可以有效减少Java程序中同步负载和内存堆分配压力的跨函数全局数据流分析算法。**

通过逃逸分析，Java Hotspot编译器能够分析出一个新的对象的引用的使用范围从而决定是否要将这个对象分配到堆上。

**逃逸分析的基本行为就是分析对象的动态作用域:**

当一个对象在方法中被定义后，对象**只在方法内部使用，则认为没有发生逃逸**，那就使用栈上分配，随着方法执行的结束，栈空间就被移除。
当一个对象在方法中被定义后，它**被外部方法所引用，则认为发生逃逸**。例如，作为调用参数传递到其他地方中。
几种常见的情况进行逃逸分析：

~~~java
public class EscapeAnalysis {
    public EscapeAnalysis obj;
    /*
     *方法返回EscapeAnalysis对象，发生逃逸
     */
    public  EscapeAnalysis getInstance(){
        return obj == null ? new EscapeAnalysis() : obj;
    }
    /*
     *为成员属性赋值，发生逃逸
     */
    public  void setObj(){
        this.obj = new EscapeAnalysis();
    }
    /*
     * 对象的作用域只在当前方法中有效，没有发生逃逸
     */
    public void useEscapeAnalysis(){
        EscapeAnalysis escapeAnalysis = new EscapeAnalysis();
    }
    /*
     *引用成员变量的值，发生逃逸
     */
    public void useEscapeAnalysis1(){
        EscapeAnalysis escapeAnalysis = getInstance();
    }
 
}
~~~

在JDK 6u23之后，HotSpot就默认开启了逃逸分析，较早的版本可以通过“-XX:+DoEscapeAnalysis”显示开启逃逸分析，“-XX: +PrintEscapeAnalysis”查看逃逸分析的筛选结果。

为了提高性能，使用逃逸分析，编译器可以对代码做如下优化：

**栈上分配**。将堆分配转化为栈分配。如果一个对象在子程序中被分配，要使指向该对象的指针永远不会逃逸，对象可能是栈分配的候选，而不是堆分配，这就要求开发中能使用局部变量的，就不要在方法外定义。
**同步省略**。如果一个对象被发现只能从一个线程被访问到，那么对于这个对象的操作可以不考虑同步。线程同步的代价是相当高的，同步的后果是降低并发性和性能。在动态编译同步块的时候，JIT编译器可以借助逃逸分析来判断同步块所使用的的锁对象是否只能够被一个线程访问而没有被发布到其他线程。如果没有，那么JIT编译器在编译这个同步块的时候就会取消对这部分代码的同步，这个过程就叫做同步省略，也叫锁消除。
**分离对象或标量替换**。有的对象可能不需要作为一个连续的内存结构存在也可以被访问到，那么对象的部分（或全部）可以不存储在内存，而是存储在CPU寄存器中。所谓标量，是指一个无法再分解成更小的数据的数据，Java中的原始数据类型就是标量。相对的，那些还可以分解的数据叫做聚合量。在JIT阶段，如果经过逃逸分析，发现一个对象不会被外界访问的话，那么经过JIT优化，就会把这个对象拆解成若干个成员变量来代替，这个过程就是标量替换。可以通过-XX:+EliminateAllocation开启标量替换，默认是打开的，允许将对象打散分配在栈上。

其实即使到如今逃逸分析技术也不是特别成熟，其根本原因就是**无法保证逃逸分析的性能消耗一定能低于其他的消耗**，虽然经过逃逸分析可以以做标量替换、栈上分配和锁消除，但是逃逸分析本身也是需要进行一系列复杂的分析的。一个比较极端的例子就是，经过逃逸分析后，发现没有一个对象是不逃逸的，那这个逃逸分析的过程就白白浪费掉了。

---

## 四类垃圾收集器

Java 8可以将垃圾收集器分为四类。

### 串行垃圾收集器Serial

为单线程环境设计且**只使用一个线程**进行GC，会暂停所有用户线程，不适用于服务器。就像去餐厅吃饭，只有一个清洁工在打扫。

### 并行垃圾收集器Parrallel

使用**多个线程**并行地进行GC，会暂停所有用户线程，适用于科学计算、大数据后台，交互性不敏感的场合。多个清洁工同时在打扫。停顿的时间会比串行垃圾收集器短。

### 并发垃圾收集器CMS

用户线程和GC线程同时执行（不一定是并行，交替执行），GC时不需要停顿用户线程，互联网公司多用，适用对响应时间有要求的场合。清洁工打扫的时候，也可以就餐。

![img](https:////upload-images.jianshu.io/upload_images/4070621-097cb3a3ee40c29c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

### G1垃圾收集器

对内存的划分与前面3种很大不同，G1将**堆内存分割成不同的区域**，然后**并发地进行垃圾回收**。

> 默认收集器有哪些？

有`Serial`、`Parallel`、`ConcMarkSweep`（CMS）、`ParNew`、`ParallelOld`、`G1`。还有一个`SerialOld`，快被淘汰了。

> 查看默认垃圾收集器

使用`java -XX:+PrintCommandLineFlags`即可看到，**Java 8默认使用`-XX:+UseParallelGC`**。

```
-XX:InitialHeapSize=132375936 -XX:MaxHeapSize=2118014976 -XX:+PrintCommandLineFlags -XX:+UseCompressedClassPointers -XX:+UseCompressedOops 
-XX:-UseLargePagesIndividualAllocation -XX:+UseParallelGC
```

> **七大垃圾收集器(上面四种垃圾收集器的实现，重要！)**

**七种回收器的使用：**
 `Serial`(串行)
 `Parallel Scavenge`(并行)
 `ParNew`(只在新生代使用并行)；

------

`SerialOld`(原本用在养老区，已经不用啦)
 `ParallelOld`(老年代的并行)
 `CMS`(并发标记清除，用于回收老年代)

------

`G1`收集器，既可以回收新生代，也可以回收老年代。

![img](https://gitee.com/xudongyin/img/raw/master/img/4070621-1cb9837ad1b4d69e.png)

连线表示可以搭配使用，红叉表示不推荐一同使用，比如新生代用`Serial`，老年代用`CMS`搭配使用。并且，**配置好新生代后，会默认配置好老年代相搭配的回收器**。

![img](https://gitee.com/xudongyin/img/raw/master/img/4070621-b4fb97b605f90e45.png)

### 1. Serial收集器（Serial/Serial Copying）

一句话：一个单线程的收集器，在进行垃圾收集时候，必须暂停其他所有的工作线程直到它收集结束。

**串行收集器是最古老，最稳定以及效率高的收集器，只使用一个线程去回收但其在进行垃圾收集过程中可能会产生较长的停顿（Stop- The-World”状态）**。虽然在收集垃圾过程中需要暂停所有其他的工作线程，但是它简单高效，对于限定单个CPU环境来说，**没有线程交互的开销可以获得最高的单线程垃圾收集效率**，因此 Serial垃圾收集器依然是java虛拟机运行在 Client模式下默认的新生代垃圾收集器。其使用**复制算法**：

- **优点**：单个线程收集，没有线程切换开销，拥有最高的单线程GC效率
- **缺点**：收集的时候会暂停用户线程。

使用`-XX:+UseSerialGC`可以显式开启，开启后默认使用`Serial`+`SerialOld`的组合。

![img](https://gitee.com/xudongyin/img/raw/master/img/4070621-3e1736ed1b44ac71.jpeg)

![img](https://gitee.com/xudongyin/img/raw/master/img/4070621-fb9f8fd436753ade.png)


### 2. ParNew收集器

**ParNew收集器其实就是 Seria收集器新生代的并行多线程版本**，最常见的应用场景是**配合老年代的 CMS GC工作**，其余的行为和Seria收集器完全一样， ParNew垃圾收集器在垃圾收集过程中同样也要暂停所有其他的工作线程。它是很多java虚拟机运行在 Server的默认新生代收集器，采用**复制算法**。

使用`-XX:+UseParNewGC`可以显式开启，开启后默认使用`ParNew`+`SerialOld`的组合。但是由于`SerialOld`已经过时，所以建议配合`CMS`使用。**ParNew收集器只影响新生代，不影响老年代。**

![img](https://gitee.com/xudongyin/img/raw/master/img/4070621-71a745748802645f.png)

![img](https://gitee.com/xudongyin/img/raw/master/img/4070621-73d4a32ca377a5d9.png)


### 3. Parallel Scavenge收集器(JDK 1.8后默认)

`ParNew`收集器仅在新生代使用多线程收集，老年代默认是`SerialOld`，所以是单线程收集。而`Parallel Scavenge`在新、老两代都采用多线程收集。`Parallel Scavenge`还有一个特点就是**吞吐量优先收集器**，可以通过自适应调节，保证最大吞吐量。采用**复制算法**。

它重点关注的是:可控制的吞吐量（ Thoughput = 运行用户代码时间/ (运行用户代码时间+垃圾收集时间），也即比如程序运行100分钟，垃圾收集时间1分钟，吞吐量就是99％）。高吞吐量意味着高效利用CPU的时间，它多用于在后台运算而不需要太多交互的任务。

**自适应调节策略也是parallelScavenge收集器与 ParNew收集器的一个重要区别。**（自适应调节策略:虚拟机会根据当前系统的运行情况收集性能监控信息，动态调整这些参数以提供最合适的停顿时间: MaxGCPause Millis）或最大的吞吐量。

常用JVM参数: `XX: +Use ParallelGC`或`-XX:+ Use ParallelOldGC`**（可互相激活）**使用 Paralle！ Scavenge收集器开启该参数后:新生代使用复制算法，老年代使用标记-整理算法。

使用`-XX:+UseParallelGC`可以开启， 同时也会使用`ParallelOld`收集老年代。其它参数，比如`-XX:ParallelGCThreads=N`可以选择N个线程进行GC，`-XX:+UseAdaptiveSizePolicy`使用自适应调节策略。

![img](https://gitee.com/xudongyin/img/raw/master/img/4070621-f5c1367f2015e35f.png)


### 4. SerialOld收集器

Serial Old是 Seria 垃圾收集器老年代版本，它同样是个单线程的收集器，使用**标记-整理**算法，这个收集器也主要是运行在 Client 默认的 java 虚拟机默认的老年代垃圾收集器。

在 Server模式下，主要有两个用途（了解，版本已经到8及以后）

- 在JDK1.5之前版本中与新生代的 Parallel Scavenge收集器搭配使用。（ Parallel Scavenge+ Serial old）
- **作为老年代版中使用CMS收集器的后备垃圾收集方案**


### 5. ParallelOld收集器

Parallel Old收集器是 Parallel Scavenge的老年代版本，**使用多线程的标记-整理算法**，parallel Old收集器在JDK1.6才开始提供。

在JDK1.6之前，新生代使用 ParalleIScavenge收集器只能搭配年老代的 Serial old 收集器，只能保证新生代的吞吐量优先，无法保证整体的吞吐量。在JDK1.6之前（ Parallel Scavenge+ Serial old），Parallel old正是为了在年老代同样提供吞吐量优先的垃圾收集器，如果系统对吞吐量要求比较高，JDK1.8后可以优先考虑新生代Parallel Scavenge和老年代 Parallel old收集器的搭配策略。**在JDK1.8及后（ Parallel Scavenge+ Parallel old）**

**使用`-XX:+UseParallelOldGC`可以开启， 同时也会使用`Parallel`收集新生代(JDK 1.8之后默认的就是这种组合)。**

### 6. CMS收集器

**CMS并发标记清除收集器**，是标记清除(Mark-Sweep)算法的实现，是一种以获得**最短GC停顿为**目标的收集器。适用在互联网或者B/S系统的服务器上，这类应用尤其重视服务器的**响应速度**，希望停顿时间最短。是`G1`收集器出来之前大型应用的首选收集器。

在GC的时候，会与用户线程并发执行，不会停顿用户线程。但是在**标记**的时候，仍然会**STW**，只不过时间非常短。

**使用`-XX:+UseConcMarkSweepGC`开启。开启过后，新生代默认使用`ParNew`，如果CMS收集效果不太理想，老年代会使用`SerialOld`作为CMS的后备收集器。**

![img](https://gitee.com/xudongyin/img/raw/master/img/4070621-5132d9dfd3110741.jpeg)

![img](https://gitee.com/xudongyin/img/raw/master/img/4070621-49a8915a92655ab0.png)

#### CMS有四步过程：

1. **初始标记**：只是标记一下GC Roots能直接关联的对象，速度很快，需要**STW**(暂停所有的工作线程)。
2. **并发标记**：主要标记过程，标记全部对象，和用户线程一起工作，不需要STW。
3. **重新标记**：修正在并发标记阶段出现的变动，需要**STW**。
4. **并发清除**：和用户线程一起，清除垃圾，不需要STW。

#### 优缺点

**优点**：停顿时间少，响应速度快，用户体验好 **(并发收集低停顿)**。

**缺点**：

- 由于并发进行，CMS在收集与应用线程会同时增加对堆内存的占用，也就是说，CMS必须要在老年代堆内存用尽之前完成垃圾回收，否则CMS回收失败时，将触发担保机制，串行老年代收集器将会以STW的方式进行一次GC，从而造成较大停顿时间
- 对CPU资源非常敏感：由于需要并发工作，多少会占用系统线程资源。
- 无法处理浮动垃圾：由于标记垃圾的时候，用户进程仍然在运行，无法有效处理新产生的垃圾。
- 产生内存碎片：由于使用**标记清除算法**，会产生内存碎片。

**回收器的选择：**

![img](https://gitee.com/xudongyin/img/raw/master/img/4070621-7493748f40d4e8ca.png)

### 7. G1收集器

G1（Garbage-First）收集器，是一款面向服务器端应用的收集器。

`G1`收集器与之前垃圾收集器的一个显著区别就是——之前收集器都有三个区域，新、老两代和元空间。**而G1收集器只有G1区(garbage-first heap)和元空间(Metaspace)**。而G1区，不像之前的收集器，分为新、老两代，而是一个一个Region，每个Region既可能包含新生代，也可能包含老年代。

`G1`收集器既可以提高吞吐量，又可以减少GC时间。最重要的是**STW可控**，增加了预测机制，让用户指定停顿时间。

CMS垃圾收集器虽然减少了暂停应用程序的运行时间，但是它还是存在着内存碎片问题。于是，**为了去除内存碎片问题，同时又保留CMS垃圾收集器低暂停时间的优点，JAVA7发布了一个新的垃圾收集器——G1垃圾收集器。**

**主要改变是Eden， Survⅳor和 Tenured等内存区域不再是连续的了，而是变成了一个个大小一样的 region，每个 region从1M到32M不等。一个 region有可能属于Eden， Survivor或者 Tenured内存区域。**

**G1收集器的设计目标是取代CMS收集器**，它同CMS相比，在以下方面表现的更出色，**G1与CMS的区别**：

- G1是一个有整理内存过程的垃圾收集器，不会产生很多内存碎片。
- G1的 Stop The World（STW）更可控，G1在停顿时间上添加预测机制，**用户可以指定期望停顿时间**
- G1整理空闲空间更快
- G1需要更多的时间来预测GC停顿的时间
- G1不希望牺牲大量的吞吐性能
- G1不需要更大的Java Heap

使用`-XX:+UseG1GC`开启，还有`-XX:G1HeapRegionSize=n`、`-XX:MaxGCPauseMillis=n`等参数可调。

#### 特点对比：

- G1之前收集器的特点：
  1. 年轻代和老年代是各自独立且连续的内存块；
  2. 年轻代收集使用单Eden+S0+S1进行复制算法；
  3. 老年代收集必须扫描整个老年代区域；
  4. 都是以尽可能少而快速地执行GC为设计原则。
- G1收集器的特点：
  1. **并行和并发**：像CMS一样，能与应用程序线程并发执行。充分利用多核、多线程CPU，尽量缩短STW
  2. **分代收集**：虽然还保留着新、老两代的概念（逻辑上分代），但物理上不再隔离，而是融合在Region中，Region也不要求连续，且会采用不同的GC方式处理不同的区域
  3. **空间整合**：`G1`整体上看是**标整**算法，在局部看又是**复制算法**，不会产生内存碎片
  4. **可预测停顿**：用户可以指定一个GC停顿时间，`G1`收集器会尽量满足

#### G1过程

与`CMS`类似，最大的好处是化整为零，只需要按照区域来进行扫描即可。

在堆的使用上，**G1并不要求对象的存储一定是物理上连续的只要逻辑上连续即可，每个分区也不会固定地为某个代服务，可以按需在年轻代和老年代之间切换**。启动时可以通过参数`-XX:G1HeapRegionSize=n`可指定分区大小（1MB~32MB，且**必须是2的幂**），默认将整堆划分为2048个分区。

大小范围在1MB~32MB，最多能设置2048个区域，也即能够支持的最大内存为：32MB*2048=65536MB=64G 内存。

![img](https://gitee.com/xudongyin/img/raw/master/img/4070621-98bbc54d0e3c75f6.png)

**G1 YGC过程：**

针对Eden区进行收集，Eden区耗尽后会被触发，主要是**小区域收集**+**形成连续的内存块**，避免内存碎片

- Eden区数据移动到Survivor区，假如出现Survivor区空间不够，Eden区数据会部分晋升到Old区
- Survivor区的数据移动到新的Survivor区，部分数据晋升到Old区
- 最后Eden区收拾干净了，GC结束，用户的应用程序继续执行

<img src="https://gitee.com/xudongyin/img/raw/master/img/4070621-49840eeae5de723f.png" alt="img" style="zoom:50%;" />

<img src="https://gitee.com/xudongyin/img/raw/master/img/4070621-316f86d8fc0fcbcd.png" alt="img" style="zoom:50%;" />

**G1收集过程小结：**

1. 初始标记。
2. 并发标记。
3. 最终标记。
4. 筛选回收。

![img](https://gitee.com/xudongyin/img/raw/master/img/4070621-42c892eb6b3bd946.png)

---

## 判断对象已经死亡

堆中几乎放着所有的对象实例，对堆垃圾回收前的第一步就是要判断那些对象已经死亡（即不能再被任何途径使用的对象）。

![img](https://gitee.com/xudongyin/img/raw/master/img/1846149-20200508182331834-429535747.png)



### 引用计数法

给对象中添加一个引用计数器，每当有一个地方引用它，计数器就加 1；当引用失效，计数器就减 1；任何时候计数器为 0 的对象就是不可能再被使用的。

这个方法实现简单，效率高，但是目前主流的虚拟机中并没有选择这个算法来管理内存，其最主要的原因是它很难解决对象之间相互循环引用的问题。 所谓对象之间的相互引用问题，如下面代码所示：除了对象 objA 和 objB 相互引用着对方之外，这两个对象之间再无任何引用。但是他们因为互相引用对方，导致它们的引用计数器都不为 0，于是引用计数算法无法通知 GC 回收器回收他们。

~~~java
public class ReferenceCountingGc {
    Object instance = null;
    public static void main(String[] args) {
        ReferenceCountingGc objA = new ReferenceCountingGc();
        ReferenceCountingGc objB = new ReferenceCountingGc();
        objA.instance = objB;
        objB.instance = objA;
        objA = null;
        objB = null;

    }
}
~~~

### 可达性分析算法

这个算法的基本思想就是通过一系列的称为 “GC Roots” 的对象作为起点，从这些节点开始向下搜索，节点所走过的路径称为引用链，当一个对象到 GC Roots 没有任何引用链相连的话，则证明此对象是不可用的。

![img](https://gitee.com/xudongyin/img/raw/master/img/1846149-20200508182418786-627240958.png)

### 再谈引用

无论是通过引用计数法判断对象引用数量，还是通过可达性分析法判断对象的引用链是否可达，判定对象的存活都与“**引用**”有关。

JDK1.2 之前，Java 中引用的定义很传统：如果 reference 类型的数据存储的数值代表的是另一块内存的起始地址，就称这块内存代表一个引用。

**JDK1.2 以后，Java 对引用的概念进行了扩充，将引用分为强引用、软引用、弱引用、虚引用四种（引用强度逐渐减弱）**

1．**强引用（StrongReference）**

以前我们使用的大部分引用实际上都是强引用，这是使用最普遍的引用。如果一个对象具有强引用，那就类似于必不可少的生活用品，垃圾回收器绝不会回收它。**当内存空间不足，Java 虚拟机宁愿抛出 OutOfMemoryError 错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足问题。**

2．**软引用（SoftReference）**

如果一个对象只具有软引用，那就类似于可有可无的生活用品。如果内存空间足够，垃圾回收器就不会回收它，如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。软引用可用来实现内存敏感的高速缓存。

软引用可以和一个引用队列（ReferenceQueue）联合使用，如果软引用所引用的对象被垃圾回收，JAVA 虚拟机就会把这个软引用加入到与之关联的引用队列中。

3．弱引用（WeakReference）

如果一个对象只具有弱引用，那就类似于可有可无的生活用品。弱引用与软引用的区别在于：**只具有弱引用的对象拥有更短暂的生命周期。**在垃圾回收器线程扫描它所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。不过，由于垃圾回收器是一个优先级很低的线程， 因此不一定会很快发现那些只具有弱引用的对象。

弱引用可以和一个引用队列（ReferenceQueue）联合使用，如果弱引用所引用的对象被垃圾回收，Java 虚拟机就会把这个弱引用加入到与之关联的引用队列中。

4．虚引用（PhantomReference）

"虚引用"顾名思义，就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。**如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收。**

**虚引用主要用来跟踪对象被垃圾回收的活动。**

虚引用与软引用和弱引用的一个区别在于： **虚引用必须和引用队列（ReferenceQueue）联合使用。**当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之关联的引用队列中。程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。程序如果发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。

特别注意，**在程序设计中一般很少使用弱引用与虚引用，使用软引用的情况较多，这是因为软引用可以加速 JVM 对垃圾内存的回收速度，可以维护系统的运行安全，防止内存溢出（OutOfMemory）等问题的产生。**


### 不可达的对象并非“非死不可”

即使在可达性分析法中不可达的对象，也并非是“非死不可”的，这时候它们暂时处于“缓刑阶段”，要真正宣告一个对象死亡，至少要经历两次标记过程；可达性分析法中不可达的对象被第一次标记并且进行一次筛选，筛选的条件是此对象是否有必要执行 finalize 方法。当对象没有覆盖 finalize 方法，或 finalize 方法已经被虚拟机调用过时，虚拟机将这两种情况视为没有必要执行。

被判定为需要执行的对象将会被放在一个队列中进行第二次标记，除非这个对象与引用链上的任何一个对象建立关联，否则就会被真的回收。


### 如何判断一个常量是废弃常量

运行时常量池主要回收的是废弃的常量。那么，我们如何判断一个常量是废弃常量呢？

**假如在常量池中存在字符串 "abc"，如果当前没有任何 String 对象引用该字符串常量的话，就说明常量 "abc" 就是废弃常量**，如果这时发生内存回收的话而且有必要的话，"abc" 就会被系统清理出常量池。

注意： JDK1.7 及之后版本的 JVM 已经将运行时常量池从方法区中移了出来，在 Java 堆（Heap）中开辟了一块区域存放运行时常量池。


### 如何判断一个类是无用的类

方法区主要回收的是无用的类，那么如何判断一个类是无用的类的呢？

判定一个常量是否是“废弃常量”比较简单，而要判定一个类是否是“无用的类”的条件则相对苛刻许多。类需要**同时满足下面 3 个条件**才能算是 “无用的类” ：

- 该类所有的实例都已经被回收，也就是 **Java 堆中不存在该类的任何实例**。
- **加载该类的 ClassLoader 已经被回收。**
- **该类对应的 java.lang.Class 对象没有在任何地方被引用，无法在任何地方通过反射访问该类的方法。**

虚拟机可以对满足上述 3 个条件的无用类进行回收，这里说的仅仅是“可以”，而**并不是和对象一样不使用了就会必然被回收。**

*****
## GC四大算法详解：

### 1. 引用计数法（现在一般不采用）
![](https://gitee.com/xudongyin/img/raw/master/img/4070621-5bc6b1cc93da0670.png)

代码示例如下：虽然objectA和objectB都置空，但是他们之前曾发生过相互引用，所以调用system.gc（手动版唤醒GC，后台也开着自动档）并不能进行垃圾回收。并且，**system.gc执行完之后也不是立刻执行垃圾回收。**

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-b7ab84e2aed6ff67.png)

**注意：在实际工作中，禁用system.gc() !!!**



### 2. 复制算法(Copying)

年轻代中使用的是Minor GC（YGC），这种GC算法采用的是复制算法(Copying)。

Minor GC会把Eden中的所有活的对象都移到Survivor区域中，如果Survivor区中放不下，那么剩下的活的对象就被移到Old  generation中，即一旦收集后，Eden是就变成空的了。

当对象在 Eden ( 包括一个 Survivor 区域，这里假设是 from 区域 ) 出生后，在经过一次 Minor GC 后，如果对象还存活，并且能够被另外一块 Survivor 区域所容纳( 这里应为 to 区域，即 to 区域有足够的内存空间来存储 Eden 和 from 区域中存活的对象 )，则使用复制算法将这些仍然还存活的对象复制到另外一块 Survivor 区域 ( 即 to 区域 ) 中，然后清理所使用过的 Eden 以及 Survivor 区域 ( 即 from 区域 )，并且将这些对象的年龄设置为1，以后对象在 Survivor 区每熬过一次 Minor GC，就将对象的年龄 + 1，当对象的年龄达到某个值时 ( 默认是 15 岁，通过 **-XX:MaxTenuringThreshold** 来设定参数)，这些对象就会成为老年代。

**-XX:MaxTenuringThreshold — 设置对象在新生代中存活的次数**



![](https://gitee.com/xudongyin/img/raw/master/img/4070621-65895cbe7327a999.png)

年轻代中的GC,主要是复制算法（Copying）。 HotSpot JVM把年轻代分为了三部分：1个Eden区和2个Survivor区（分别叫from和to）。默认比例为8:1:1,一般情况下，新创建的对象都会被分配到Eden区(一些大对象特殊处理),这些对象经过第一次Minor GC后，如果仍然存活，将会被移到Survivor区。对象在Survivor区中每熬过一次Minor GC，年龄就会增加1岁，当它的年龄增加到一定程度时，就会被移动到年老代中。因为年轻代中的对象基本都是朝生夕死的(90%以上)，**所以在年轻代的垃圾回收算法使用的是复制算法**，复制算法的基本思想就是将内存分为两块，每次只用其中一块(from)，当这一块内存用完，就将还活着的对象复制到另外一块上面。**复制算法的优点是不会产生内存碎片，缺点是耗费空间**。

在GC开始的时候，对象只会存在于Eden区和名为“From”的Survivor区，Survivor区“To”是空的。紧接着进行GC，Eden区中所有存活的对象都会被复制到“To”，而在“From”区中，仍存活的对象会根据他们的年龄值来决定去向。年龄达到一定值(年龄阈值，可以通过-XX:MaxTenuringThreshold来设置)的对象会被移动到年老代中，没有达到阈值的对象会被复制到“To”区域。**经过这次GC后，Eden区和From区已经被清空。这个时候，“From”和“To”会交换他们的角色，也就是新的“To”就是上次GC前的“From”，新的“From”就是上次GC前的“To”**。不管怎样，都会保证名为To的Survivor区域是空的。Minor GC会一直重复这样的过程，直到“To”区被填满，“To”区被填满之后，会将所有对象移动到年老代中。

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-6c6b1a262845b208.png)



因为Eden区对象一般存活率较低，一般的，使用两块10%的内存作为空闲和活动区间，而另外80%的内存，则是用来给新建对象分配内存的。一旦发生GC，将10%的from活动区间与另外80%中存活的eden对象转移到10%的to空闲区间，接下来，将之前90%的内存全部释放，以此类推。 


![蜜汁动画：看不懂请忽略](https://gitee.com/xudongyin/img/raw/master/img/4070621-1db29c74a72b69e9.gif)

上面动画中，Area空闲代表to，Area激活代表from，绿色代表不被回收的，红色代表被回收的。


**复制算法它的缺点也是相当明显的:**
* 它浪费了一半的内存，这太要命了。 
* 如果对象的存活率很高，我们可以极端一点，假设是100%存活，那么我们需要将所有对象都复制一遍，并将所有引用地址重置一遍。复制这一工作所花费的时间，在对象存活率达到一定程度时，将会变的不可忽视。 所以从以上描述不难看出，复制算法要想使用，最起码对象的存活率要非常低才行，而且最重要的是，我们必须要克服50%内存的浪费。

  


### 3 .标记清除(Mark-Sweep)
**复制算法的缺点就是费空间，其是用在年轻代的，老年代一般是由标记清除或者是标记清除与标记整理的混合实现。** 

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-f33c19fb1b50e432.png)
![](https://upload-images.jianshu.io/upload_images/4070621-048d4544ed0c75a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


用通俗的话解释一下标记清除算法，就是当程序运行期间，若可以使用的内存被耗尽的时候，GC线程就会被触发并将程序暂停，随后将要回收的对象标记一遍，最终统一回收这些对象，完成标记清理工作接下来便让应用程序恢复运行。

主要进行两项工作，第一项则是标记，第二项则是清除。  
 * 标记：从引用根节点开始标记遍历所有的GC Roots， 先标记出要回收的对象。
* 清除：遍历整个堆，把标记的对象清除。 

**缺点：此算法需要暂停整个应用，会产生内存碎片**

![标记清除算法动态版](https://gitee.com/xudongyin/img/raw/master/img/4070621-6cf07565eed0fb2f.gif)

**标记清除算法小结：**

* 1、首先，它的缺点就是效率比较低（递归与全堆对象遍历），而且在进行GC的时候，需要停止   应用程序，这会导致用户体验非常差劲
* 2、其次，主要的缺点则是这种方式清理出来的空闲内存是不连续的，这点不难理解，我们的死亡对象都是随即的出现在内存的各个角落的，现在把它们清除之后，内存的布局自然会乱七八糟。而为了应付这一点，JVM就不得不维持一个内存的空闲列表，这又是一种开销。而且在分配数组对象的时候，寻找连续的内存空间会不太好找。 




### 4. 标记压缩(Mark-Compact)

标记压缩(Mark-Compact)又叫标记清除压缩(Mark-Sweep-Compact)，或者标记清除整理算法。老年代一般是由**标记清除**或者是**标记清除与标记整理的混合**实现

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-2833156bab33fba9.png)

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-c535423e8c9ad06b.png)

![](https://gitee.com/xudongyin/img/raw/master/img/4070621-cf3df690e1ca5521.png)


![标记清除整理动态版](https://gitee.com/xudongyin/img/raw/master/img/4070621-fdc8c57e8e175c3a.gif)


>**面试题：四种算法那个好**
>Answer：没有那个算法是能一次性解决所有问题的，因为JVM垃圾回收使用的是**分代收集算法**，没有最好的算法，只有根据每一代他的垃圾回收的特性用对应的算法。**新生代使用复制算法，老年代使用标记清除和标记整理算法。**没有最好的垃圾回收机制，只有最合适的。

>**面试题：请说出各个垃圾回收算法的优缺点**
>* **内存效率：**复制算法>标记清除算法>标记整理算法（此处的效率只是简单的对比时间复杂度，实际情况不一定如此）。 
>* **内存整齐度：**复制算法=标记整理算法>标记清除算法。 
>* **内存利用率：**标记整理算法=标记清除算法>复制算法。 
>
>可以看出，效率上来说，复制算法是当之无愧的老大，但是却浪费了太多内存，而**为了尽量兼顾上面所提到的三个指标，标记/整理算法相对来说更平滑一些**，但效率上依然不尽如人意，它比复制算法多了一个标记的阶段，又比标记/清除多了一个整理内存的过程
>
>难道就没有一种最优算法吗？Java 9 之后出现了**G1垃圾回收器**，能够解决以上问题，有兴趣参考[这篇文章](https://www.jianshu.com/p/ba415aa2330b)。

*****
### 总结：

* ##### 年轻代(Young Gen)  
 年轻代特点是区域相对老年代较小，对象存活率低。

这种情况复制算法的回收整理，速度是最快的。复制算法的效率只和当前存活对像大小有关，因而很适用于年轻代的回收。而复制算法内存利用率不高的问题，通过hotspot中的两个survivor的设计得到缓解。

* ##### 老年代(Tenure Gen)

老年代的特点是区域较大，对象存活率高。

这种情况，存在大量存活率高的对象，复制算法明显变得不合适。一般是由标记清除或者是标记清除与标记整理的混合实现。

**Mark（标记）**阶段的开销与存活对象的数量成正比，这点上说来，对于老年代，标记清除或者标记整理有一些不符，但可以通过多核/线程利用，对并发、并行的形式提标记效率。

**Sweep（清除）**阶段的开销与所管理区域的大小正相关，但Sweep“就地处决”的特点，回收的过程没有对象的移动。使其相对其它有对象移动步骤的回收算法，仍然是效率最好的。但是需要解决内存碎片问题。

**Compact（压缩）**阶段的开销与存活对像的数量成正比，如上一条所描述，对于大量对象的移动是很大开销的，做为老年代的第一选择并不合适。

基于上面的考虑，老年代一般是由标记清除或者是标记清除与标记整理的混合实现。以hotspot中的CMS回收器为例，CMS是基于Mark-Sweep实现的，对于对像的回收效率很高，而对于碎片问题，CMS采用基于Mark-Compact算法的Serial Old回收器做为补偿措施：当内存回收不佳（碎片导致的Concurrent Mode Failure时），将采用Serial Old执行Full GC以达到对老年代内存的整理。

---

# JMM

1.什么是JMM？
JMM：（Java Memory Model 的缩写）JAVA 内存模型

2.作用是什么？
作用：缓存一致性协议，用于定义数据读写规则
JMM定义了线程工作内存和主内存之间的抽象关系：线程之间的共享变量存储在主内存（Main Memory）中，每个线程都有一个私有的本地内存（Local Memory）

JMM规定了内存主要划分为主内存和工作内存两种。此处的主内存和工作内存跟JVM内存划分（堆、栈、方法区）是在不同的层次上进行的，如果非要对应起来，主内存对应的是Java堆中的对象实例部分，工作内存对应的是栈中的部分区域，从更底层的来说，主内存对应的是硬件的物理内存，工作内存对应的是寄存器和高速缓存。

![img](https://gitee.com/xudongyin/img/raw/master/img/1102674-20180815143324915-2024156794.png)

![image-20200719145804616](https://gitee.com/xudongyin/img/raw/master/img/image-20200719145804616.png)

解决共享对象可见性问题：volatile或者synchronized 同步机制

## 内存交互操作

 　内存交互操作有8种，虚拟机实现必须保证每一个操作都是原子的，不可在分的（对于double和long类型的变量来说，load、store、read和write操作在某些平台上允许例外）

- lock   （锁定）：作用于主内存的变量，把一个变量标识为线程独占状态
- unlock （解锁）：作用于主内存的变量，它把一个处于锁定状态的变量释放出来，释放后的变量才可以被其他线程锁定
- read  （读取）：作用于主内存变量，它把一个变量的值从主内存传输到线程的工作内存中，以便随后的load动作使用
- load   （载入）：作用于工作内存的变量，它把read操作从主存中变量放入工作内存中
- use   （使用）：作用于工作内存中的变量，它把工作内存中的变量传输给执行引擎，每当虚拟机遇到一个需要使用到变量的值，就会使用到这个指令
- assign （赋值）：作用于工作内存中的变量，它把一个从执行引擎中接受到的值放入工作内存的变量副本中
- store  （存储）：作用于主内存中的变量，它把一个从工作内存中一个变量的值传送到主内存中，以便后续的write使用
- write 　（写入）：作用于主内存中的变量，它把store操作从工作内存中得到的变量的值放入主内存的变量中

　　JMM对这八种指令的使用，制定了如下规则：

- 不允许read和load、store和write操作之一单独出现。即使用了read必须load，使用了store必须write
- 不允许线程丢弃他最近的assign操作，即工作变量的数据改变了之后，必须告知主存
- 不允许一个线程将没有assign的数据从工作内存同步回主内存
- 一个新的变量必须在主内存中诞生，不允许工作内存直接使用一个未被初始化的变量。就是对变量实施use、store操作之前，必须经过load和assign操作
- 一个变量同一时间只有一个线程能对其进行lock。多次lock后，必须执行相同次数的unlock才能解锁
- 如果对一个变量进行lock操作，会清空所有工作内存中此变量的值，在执行引擎使用这个变量前，必须重新load或assign操作初始化变量的值
- 如果一个变量没有被lock，就不能对其进行unlock操作。也不能unlock一个被其他线程锁住的变量
- 对一个变量进行unlock操作之前，必须把此变量同步回主内存

　　JMM对这八种操作规则和对[volatile的一些特殊规则](https://www.cnblogs.com/null-qige/p/8569131.html)就能确定哪些操作是线程安全，哪些操作是线程不安全的了。但是这些规则实在复杂，很难在实践中直接分析。所以一般我们也不会通过上述规则进行分析。更多的时候，使用java的happen-before规则来进行分析。

**happens-before原则：**

　　Java内存模型中定义的两项操作之间的次序关系，如果说操作A先行发生于操作B，操作A产生的影响能被操作B观察到，“影响”包含了修改了内存中共享变量的值、发送了消息、调用了方法等。

　　下面是Java内存模型下一些”天然的“happens-before关系，这些happens-before关系无须任何同步器协助就已经存在，可以在编码中直接使用。如果两个操作之间的关系不在此列，并且无法从下列规则推导出来的话，它们就没有顺序性保障，虚拟机可以对它们进行随意地重排序。

　　a.程序次序规则(Pragram Order Rule)：在一个线程内，按照程序代码顺序，书写在前面的操作先行发生于书写在后面的操作。准确地说应该是控制流顺序而不是程序代码顺序，因为要考虑分支、循环结构。

　　b.管程锁定规则(Monitor Lock Rule)：一个unlock操作先行发生于对同一个锁的lock操作的后面。lock先于unlock

　　c.volatile变量规则(Volatile Variable Rule)：对一个volatile变量的写操作先行发生于后面对这个变量的读取操作。volatile变量写先于读。

　　d.线程启动规则(Thread Start Rule)：Thread对象的start()方法先行发生于此线程的每一个动作。

　　e.线程终于规则(Thread Termination Rule)：线程中的所有操作都先行发生于对此线程的终止检测，我们可以通过Thread.join()方法结束，Thread.isAlive()的返回值等作段检测到线程已经终止执行。

　　f.线程中断规则(Thread Interruption Rule)：对线程interrupt()方法的调用先行发生于被中断线程的代码检测到中断事件的发生，可以通过Thread.interrupted()方法检测是否有中断发生。中断先于中断检测

　　g.对象终结规则(Finalizer Rule)：一个对象初始化完成(构造方法执行完成)先行发生于它的finalize()方法的开始。

　　f.传递性(Transitivity)：如果操作A先行发生于操作B，操作B先行发生于操作C，那就可以得出操作A先行发生于操作C的结论。

---

# Java编译与解释

编程语言分为低级语言和高级语言，机器语言、汇编语言是低级语言，C、C++、java、python等是高级语言。
**机器语言**是最底层的语言，能够直接执行。而我们编写的源代码是人类语言, 计算机只能识别某些特定的二进制指令，在程序真正运行之前必须将源代码转换成二进制指令。
**汇编语言**通过汇编器翻译成机器指令后执行，一条汇编指令，对应着一条机器指令。
**高级语言**编程的程序有三种执行方式：

1.一种是编译执行，源程序先通过编译器（负责将源程序翻译成目标机器指令）翻译成机器指令，通过**编译-->链接-->目标可执行文件，然后执行**；即提前将所有源代码一次性转换成二进制指令，也就是生成一个可执行程序。比如**C，C++等语言都是编译执行的**。

2.一种是解释执行，是使用解释器会将我们的一句句代码解释成机器可以识别的二进制代码来执行，可以认为是，**解释一句，执行一句**。在这个过程中，**不会生成中间文件**。如：脚本方式是一条条命令，在执行时，是由系统的解释器，将其一条条翻译成机器可识别的指令，例如**shell脚本是由shell程序执行的，js是由浏览器解释执行的。**

3.最后一种是**编译和解释相结合的执行方式，比如Java。**

### 理解Java的几个编译器

前端编译器：把.java文件转变成.class文件。包括Sun的Javac、Eclipse JDT中的增量式编辑器（ECJ）

后端运行期**即时编译器**（JIT编译器，Just In Time Compiler）：把字节码转成机器码。包括HotSpot VM的C1、C2编译器

静态提前编译器（AOT编译器，Ahead Of Time Compiler）：把*.java编译成本地机器码。包括GNU Compiler for the Java（GCJ）、Excelsior JET


### Java采用的是解释和编译混合的模式

在编译时期，我们通过将源代码编译成.class ，配合JVM这种跨平台的抽象，屏蔽了底层计算机操作系统和硬件的区别，实现了“一次编译，到处运行” 。 而在运行时期，目前主流的JVM 都是混合模式（-Xmixed），即**解释运行和编译运行配合使用**。 

Java一开始被定位为“解释执行”的语言，但是现在主流的虚拟机中都包含了**即时编译器JIT**。

程序从源代码到运行经历阶段：**java程序--（编译javac）-->字节码文件.class-->类装载子系统化身为反射类Class--->运行时数据区--->（解释执行+JIT编译器编译）-->操作系统（Win，Linux，Mac JVM）。**

**.class文件就是可以到处运行的文件。**然后Java字节码会被转化为目标机器代码，这是是由JVM来执行的，即Java的第二次编译。

Java采用的是解释和编译混合的模式:基于JVM执行引擎当中的解释器interpreter与即使编译器JIT共存

![img](https://gitee.com/xudongyin/img/raw/master/img/1846149-20200718104718856-2054989511.png)

执行引擎获取到由javac将源码编译成的字节码文件class，然后在运行的时候通过解释器interpreter转换成最终的机器码。（解释型）

另外JVM平台支持一种叫作即时编译的技术。即时编译的目的是避免函数被解释执行，而是将整个函数体编译成为机器码，这种方式可以使执行效率大幅度提升（直接编译型）

![img](https://img2020.cnblogs.com/blog/1846149/202007/1846149-20200718111501995-1978575555.png)

#### JIT将字节码转换成最终的机器码：

以 Oracle JDK提供的HotSpot虚拟机为例，在HotSpot虚拟机中，提供了两种编译模式：解释执行 和 即时编译（JIT，Just-In-Time）。

**解释执行即逐条翻译字节码为可运行的机器码，而即时编译则以方法为单位将字节码翻译成机器码**（上述提到的“编译执行”）。**前者的优势在于不用等待，后者则在实际运行当中效率更高**。

　　即时编译存在的意义在于它是提高程序性能的重要手段之一。根据“二八定律”（即：百分之二十的代码占据百分之八十的系统资源），**对于大部分不常用的代码，我们无需耗时间将之编译为机器码，而是采用解释执行的方式，用到就去逐条解释运行；对于一些仅占据小部分的热点代码（可认为是反复执行的重要代码），则可将之翻译为符合机器的机器码高效执行，提高程序的效率，即运行时的即时编译。**

　　为了满足不同的场景，HotSpot虚拟机内置了多个即时编译器：C1,C2与Graal。Graal 是Java10正式引入的实验性即时编译器，在此暂不叙述。先看一下C1、C2 ，相信大家或多或少接触过。

- C1：即Client编译器，面向对启动性能有要求的客户端GUI程序，采用的优化手段比较简单，因此**编译的时间较短。**
- C2：即Server编译器，面向对性能峰值有要求的服务端程序，采用的优化手段复杂，因此**编译时间长，但是在运行过程中性能更好。**

从Java7开始，HotSpot虚拟机默认采用分层编译的方式：热点方法首先被C1编译器编译，而后 **热点方法中的热点**再进一步被C2编译，根据前面的运行计算出更优的编译优化。为了不干扰程序的正常运行，JIT编译时放在额外的线程中执行的，HotSpot根据实际CPU的资源，以 1:2的比例分配给C1和C2线程数。在计算机资源充足的情况，字节码的解释运行和编译运行时可以同时进行，JIT编译执行完后的机器码会在下次调用该方法时启动，已替换原本的解释执行（意思就是已经翻译出效率更高的机器码，自然替换原来的相对低效率执行的方法）。

　　以上，可以看出在Java中不单单是解释执行，即时编译（编译执行）在Java性能优化中彰显重要的作用，所以现在应该说：Java是解释执行和编译执行共同存在的，至少大部分是这样。


### 编译与解释比较？

1.一段程序编译会浪费时间，并且移植到其他平台上时还要进行重新编译，但是其编译后生成的可执行文件运行速度快。

2.解释型程序可跨平台执行，无需将全部代码编译之后再运行，能够及时运行，但因为是逐条解释执行所以最终的运行速度不如编译型程序。

3.内存使用：编译执行需要生成编译后的机器码文件，而解释执行时逐句解释执行，所以解释执行对内存占用更少。

> **单独使用解释器的缺点：**

抛弃了JIT可能带来的性能优势。如果代码没有被JIT编译的话，再次运行时需要重复解析。

> **单独使用JIT编译器的缺点：**

需要将全部的代码编译成本地机器码。要花更多的时间，JVM启动会变慢非常多；

增加可执行代码的长度（字节码比JIT编译后的机器码小很多），这将导致页面调度，从而降低程序的速度。

有些JIT编译器的优化方式，比如分支预测，如果不进行profiling，往往并不能进行有效优化。

因此，HotSpot采用了惰性评估(Lazy Evaluation)的做法，根据二八定律，消耗大部分系统资源的只有那一小部分的代码（热点代码），而这也就是JIT所需要编译的部分。JVM会根据代码每次被执行的情况收集信息并相应地做出一些优化，因此执行的次数越多，它的速度就越快。

JDK 9引入了一种新的编译模式AOT(Ahead of Time Compilation)，它是直接将字节码编译成机器码，这样就避免了JIT预热等各方面的开销。JDK支持分层编译和AOT协作使用。

注：JIT为方法级，它会缓存编译过的字节码在CodeCache中，而不需要被重复解释。

![image-20200719144135490](https://gitee.com/xudongyin/img/raw/master/img/image-20200719144135490.png) 