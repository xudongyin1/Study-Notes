## 基础篇

**1、面向对象的三大特性？分别解释下？**

（1）封装：通常认为封装是把数据和操作数据的方法封装起来，对数据的访问只能通过已定义的接口。

（2）继承：继承是从已有类得到继承信息创建新类的过程。提供继承信息的类被称为父类（超类/基类），得到继承信息的被称为子类（派生类）。

（3）多态：分为编译时多态（方法重载）和运行时多态（方法重写）。要实现多态需要做两件事：一是子类继承父类并重写父类中的方法，二是用父类型引用子类型对象，这样同样的引用调用同样的方法就会根据子类对象的不同而表现出不同的行为。

> **关于继承的几点补充：**

（1）子类拥有父类对象所有的属性和方法（包括私有属性和私有方法），但是父类中的私有属性和方法子类是无法访问，只是拥有。因为在一个子类被创建的时候，首先会在内存中创建一个父类对象，然后在父类对象外部放上子类独有的属性，两者合起来形成一个子类的对象；

（2）子类可以拥有自己属性和方法；

（3）子类可以用自己的方式实现父类的方法。（重写）

**2、重载和重写的区别？**

（1）重载：编译时多态、同一个类中同名的方法具有不同的参数列表、不能只根据返回类型进行区分【因为：函数调用时不能指定类型信息，编译器不知道你要调哪个函数】；

（2）重写（又名覆盖）：运行时多态、子类与父类之间、子类重写父类的方法具有相同的返回类型、更好的访问权限。

**普通的类方法是可以和类名同名的，和构造方法唯一的区分就是，构造方法没有返回值。**

**3、Java 中是否可以重写一个 private 或者 static 方法？**

Java 中 static 方法不能被覆盖，因为方法覆盖是基于运行时动态绑定的，而 static 方法是编译时静态绑定的。static 方法跟类的任何实例都不相关，所以概念上不适用。 

Java 中也不可以覆盖 private 的方法，因为 private 修饰的变量和方法只能在当前类中使用， 如果是其他的类继承当前类是不能访问到 private 变量或方法的，当然也不能覆盖。

> **静态方法的补充**

静态的方法可以被继承，但是不能重写。如果父类和子类中存在同样名称和参数的静态方法，那么该子类的方法会把原来继承过来的父类的方法隐藏，而不是重写。通俗的讲就是父类的方法和子类的方法是两个没有关系的方法，具体调用哪一个方法是看是哪个对象的引用；这种父子类方法也不在存在多态的性质。

**4、Java 中创建对象的几种方式？**

1、使用 new 关键字；

2、使用 Class 类的 newInstance 方法，该方法调用无参的构造器创建对象（反射）：Class.forName.newInstance()；

3、使用 clone() 方法；

4、反序列化，比如调用 ObjectInputStream 类的 readObject() 方法。

**5、抽象类和接口有什么区别？**

（1）抽象类中可以定义构造函数，接口不能定义构造函数；

（2）抽象类中可以有抽象方法和具体方法，而接口中只能有抽象方法（public abstract）；

（3）抽象类中的成员权限可以是 public、默认、protected（抽象类中抽象方法就是为了重写，所以不能被 private 修饰），而接口中的成员只可以是 public（方法默认：public abstrat、成员变量默认：public static final）；

（4）抽象类中可以包含静态方法，而接口中不可以包含静态方法；

> **JDK8中的改变：**

1、在 JDK1.8中，允许在接口中包含带有具体实现的方法，使用 default 修饰，这类方法就是默认方法。

2、抽象类中可以包含静态方法，在 JDK1.8 之前接口中不能包含静态方法，JDK1.8 以后可以包含。之前不能包含是因为，接口不可以实现方法，只可以定义方法，所以不能使用静态方法（因为静态方法必须实现）。现在可以包含了，只能直接用接口调用静态方法。JDK1.8 仍然不可以包含静态代码块。

**6、short s1 = 1；s1 = s1 + 1；有什么错？那么 short s1 = 1; s1 += 1；呢？有没有错误？**

对于 short s1 = 1; s1 = s1 + 1; 来说，在 s1 + 1 运算时会自动提升表达式的类型为 int ，那么将 int 型值赋值给 short 型变量，s1 会出现类型转换错误。

对于 short s1 = 1; s1 += 1; 来说，+= 是 Java 语言规定的运算符，Java 编译器会对它进行特殊处理，因此可以正确编译。

**7、Integer 和 int 的区别？**

（1）int 是 Java 的八种基本数据类型之一，而 Integer 是 Java 为 int 类型提供的封装类；

（2）int 型变量的默认值是 0，Integer 变量的默认值是 null，这一点说明 Integer 可以区分出未赋值和值为 0 的区分；

（3）Integer 变量必须实例化后才可以使用，而 int 不需要。

> **关于 Integer 和 int 的比较的延伸：**

1、由于 Integer 变量实际上是对一个 Integer 对象的引用，所以两个通过 new 生成的 Integer 变量永远是不相等的，因为其内存地址是不同的；

2、Integer 变量和 int 变量比较时，只要两个变量的值是相等的，则结果为 true。因为包装类 Integer 和基本数据类型 int 类型进行比较时，Java 会自动拆包装类为 int，然后进行比较，实际上就是两个 int 型变量在进行比较；

3、非 new 生成的 Integer 变量和 new Integer() 生成的变量进行比较时，结果为 false。因为非 new 生成的 Integer 变量指向的是 Java 常量池中的对象，而 new Integer() 生成的变量指向堆中新建的对象，两者在内存中的地址不同；

4、对于两个非 new 生成的 Integer 对象进行比较时，如果两个变量的值在区间 [-128, 127] 之间，则比较结果为 true，否则为 false。Java 在编译 Integer i = 100 时，会编译成 Integer i = Integer.valueOf(100)，而 Integer 类型的 valueOf 的源码如下所示：

```java
public static Integer valueOf(int var0) {           
    return var0 >= -128 && var0 <= Integer.IntegerCache.high ?Integer.IntegerCache.cache[var0 + 128] : new Integer(var0);}
```

从上面的代码中可以看出：Java 对于 [-128, 127] 之间的数会进行缓存，比如：Integer i = 127，会将 127 进行缓存，下次再写 Integer j = 127 的时候，就会直接从缓存中取出，而对于这个区间之外的数就需要 new 了。

> **包装类的缓存：**

Boolean：全部缓存     Byte：全部缓存    Character：<= 127 缓存     Short：-128 — 127 缓存

Long：-128 — 127 缓存      Integer：-128 — 127 缓存     Float：没有缓存    Doulbe：没有缓存

**8、String、StringBuilder、StringBuffer 的区别？**

String：用于字符串操作，属于不可变类；【补充：String 不是基本数据类型，是引用类型，底层用 char 数组实现的】

StringBuilder：与 StringBuffer 类似，都是字符串缓冲区，但线程不安全；

StringBuffer：也用于字符串操作，不同之处是 StringBuffer 属于可变类，对方法加了同步锁，线程安全。

> **StringBuffer的补充：**

说明：StringBuffer 中并不是所有方法都使用了 Synchronized 修饰来实现同步：

```java
@Override
public StringBuffer insert(int dstOffset, CharSequence s) {  
    // Note, synchronization achieved via invocations of other StringBuffer methods    // after narrowing of s to specific type  
    // Ditto for toStringCache clearing  
    super.insert(dstOffset, s);  
    return this;
}
```

执行效率：StringBuilder > StringBuffer > String

**9、String 字符串修改实现的原理？**

当用 String 类型来对字符串进行修改时，其实现方法是首先创建一个 StringBuffer，其次调用 StringBuffer 的 append() 方法，最后调用 StringBuffer 的 toString() 方法把结果返回。

**10、final 修饰 StringBuffer 后还可以 append 吗？**

可以。final 修饰的是一个引用变量，那么这个引用始终只能指向这个对象，但是这个对象内部的属性是可以变化的。

**11、final、finally、finalize 的区别？**

final：用于声明属性、方法和类，分别表示属性不可变、方法不可覆盖、被其修饰的类不可继承；

finally：异常处理语句结构的一部分，表示总是执行；

finallize：Object类的一个方法，在垃圾回收时会调用被回收对象的finalize。

**12、finally 块中的代码什么时候被执行？**

在 Java 语言的异常处理中，finally 块的作用就是为了保证无论出现什么情况，finally 块里的代码一定会被执行。由于程序执行 return 就意味着结束对当前函数的调用并跳出这个函数体，因此任何语句要执行都只能在 return 前执行（除非碰到 exit 函数），因此 finally 块里的代码也是在 return 之前执行的。

此外，如果 try-finally 或者 catch-finally 中都有 return，那么 finally 块中的 return 将会覆盖别处的 return 语句，最终返回到调用者那里的是 finally 中 return 的值。

**13、finally 是不是一定会被执行到？**

不一定。下面列举两种执行不到的情况：

（1）当程序进入 try 块之前就出现异常时，会直接结束，不会执行 finally 块中的代码；

（2）当程序在 try 块中强制退出时也不会去执行 finally 块中的代码，比如在 try 块中执行 exit 方法。

**14、try-catch-finally 中，如果 catch 中 return 了，finally 还会执行吗？**

会。程序在执行到 return 时会首先将返回值存储在一个指定的位置，其次去执行 finally 块，最后再返回。因此，对基本数据类型，在 finally 块中改变 return 的值没有任何影响，直接覆盖掉；而对引用类型是有影响的，返回的是在 finally 对 前面 return 语句返回对象的修改值。

> **初始化补充：**

存在继承的情况下，初始化顺序为：

1. 父类（静态变量、静态语句块）

2. 子类（静态变量、静态语句块）

3. 父类（实例变量、普通语句块）

4. 父类（构造函数）

5. 子类（实例变量、普通语句块）

6. 子类（构造函数）

**15、super 关键字的作用？**

（1）访问父类的构造函数：可以使用 super() 函数访问父类的构造函数，从而委托父类完成一些初始化的工作。

（2）访问父类的成员：如果子类重写了父类的某个方法，可以通过使用 super 关键字来引用父类的方法实现。

（3）this 和 super 不能同时出现在一个构造函数里面，因为 this 必然会调用其它的构造函数，其它的构造函数必然也会有 super 语句的存在，所以在同一个构造函数里面有相同的语句，就失去了语句的意义，编译器也不会通过。

**16、transient 关键字的作用？**

对于不想进行序列化的变量，使用 transient 关键字修饰。

transient 关键字的作用是：阻止实例中那些用此关键字修饰的的变量序列化。当对象被反序列化时，被 transient 修饰的变量值不会被持久化和恢复。transient 只能修饰变量，不能修饰类和方法。

**17、== 和 equals 的区别？**

==：如果比较的对象是基本数据类型，则比较的是数值是否相等；如果比较的是引用数据类型，则比较的是对象的地址值是否相等。

equals 方法：先比较引用地址是否相同，再比较两个对象的内容是否相等。注意：equals 方法不能被基本数据类型的变量调用。如果没有对 equals 方法进行重写，则比较的是引用类型的变量所指向的对象的地址（很多类重新了 equals 方法，比如 String、Integer 等把它变成了值比较，所以一般情况下 equals 比较的是值是否相等）。

**18、Java 中的参数传递时传值呢？还是传引用？**

Java 的参数是以值传递的形式传入方法中，而不是引用传递。

1、**基本类型作为参数传递时，是传递值的拷贝，无论你怎么改变这个拷贝，原值是不会改变的**

2、**对象作为参数传递时，是把对象在内存中的地址拷贝了一份传给了参数。**

当传递方法参数类型为引用数据类型时，一个方法将修改一个引用数据类型的参数所指向对象的值。即使 Java 函数在传递引用数据类型时，也只是拷贝了引用的值罢了，之所以能修改引用数据是因为它们同时指向了一个对象，但这仍然是按值调用而不是引用调用。

**19、Java 中的 Math.round(-1.5) 等于多少？**

等于 -1，因为在数轴上取值时，中间值（0.5）向右取整，所以正 0.5 是往上取整，负 0.5 是直接舍弃。

**20、Error 和 Exception 的区别？**

Error 类和 Exception 类的父类都是 Throwable 类。主要区别如下：

Error 类： 一般是指与虚拟机相关的问题，如：系统崩溃、虚拟机错误、内存空间不足、方法调用栈溢出等。这类错误将会导致应用程序中断，仅靠程序本身无法恢复和预防；

Exception 类：分为运行时异常和受检查的异常，是程序员可以解决修复的。

**21、运行时异常与受检异常有何异同？**

运行时异常：如：空指针异常、指定的类找不到、数组越界、方法传递参数错误、数据类型转换错误。可以编译通过，但是一运行就停止了，程序不会自己处理；

受检查异常：要么用 try … catch… 捕获，要么用 throws 声明抛出，交给父类处理。

**22、常见的异常类有哪些？**

NullPointerException：当应用程序试图访问空对象时，则抛出该异常。

SQLException：提供关于数据库访问错误或其他错误信息的异常。

IndexOutOfBoundsException：指示某排序索引（例如对数组、字符串或向量的排序）超出范围时抛出。

FileNotFoundException：当试图打开指定路径名表示的文件失败时，抛出此异常。

IOException：当发生某种 I/O 异常时，抛出此异常。此类是失败或中断的 I/O 操作生成的异常的通用类。

ClassCastException：当试图将对象强制转换为不是实例的子类时，抛出该异常。

IllegalArgumentException：抛出的异常表明向方法传递了一个不合法或不正确的参数。

**23、主线程可以捕获到子线程的异常吗？**

线程设计的理念：“线程的问题应该线程自己本身来解决，而不要委托到外部”。

正常情况下，如果不做特殊的处理，在主线程中是不能够捕获到子线程中的异常的。如果想要在主线程中捕获子线程的异常，我们可以用如下的方式进行处理，使用 Thread 的静态方法。

```java
Thread.setDefaultUncaughtExceptionHandler(new MyUncaughtExceptionHandle());
```

**24、Java 的泛型是如何工作的 ? 什么是类型擦除 ?**

 泛型是通过类型擦除来实现的，编译器在编译时擦除了所有类型相关的信息，所以在运行时不存在任何类型相关的信息。例如：List<String> 在运行时仅用一个 List 来表示。这样做的目的，是确保能和 Java 5 之前的版本开发二进制类库进行兼容。

类型擦除：泛型信息只存在于代码编译阶段，在进入 JVM 之前，与泛型相关的信息会被擦除掉，专业术语叫做类型擦除。在泛型类被类型擦除的时候，之前泛型类中的类型参数部分如果没有指定上限，如 <T> 则会被转译成普通的 Object 类型，如果指定了上限如 <T extends String> 则类型参数就被替换成类型上限。

> **补充：**

```java
List<String> list = new ArrayList<String>();
```

1、两个 String 其实只有第一个起作用，后面一个没什么卵用，只不过 JDK7 才开始支持 List<String>list = new ArrayList<> 这种写法。

2、第一个 String 就是告诉编译器，List 中存储的是 String 对象，也就是起类型检查的作用，之后编译器会擦除泛型占位符，以保证兼容以前的代码。

**25、如何实现对象的克隆？**

（1）实现 Cloneable 接口并重写 Object 类中的 clone() 方法；

（2）实现 Serializable 接口，通过对象的序列化和反序列化实现克隆，可以实现真正的深克隆。

**26、深克隆和浅克隆的区别？**

（1）浅克隆：拷贝对象和原始对象的引用类型引用同一个对象。浅克隆只是复制了对象的引用地址，两个对象指向同一个内存地址，所以修改其中任意的值，另一个值都会随之变化，这就是浅克隆。

（2）深克隆：拷贝对象和原始对象的引用类型引用不同对象。深拷贝是将对象及值复制过来，两个对象修改其中任意的值另一个值不会改变，这就是深拷贝（例：JSON.parse() 和 JSON.stringify()，但是此方法无法复制函数类型）。

> **克隆的补充：**

深克隆的实现就是在引用类型所在的类实现 Cloneable 接口，并使用 public 访问修饰符重写 clone 方法。

Java 中定义的 clone 没有深浅之分，都是统一的调用 Object 的 clone 方法。为什么会有深克隆的概念？是由于我们在实现的过程中刻意的嵌套了 clone 方法的调用。也就是说深克隆就是在需要克隆的对象类型的类中重新实现克隆方法 clone()。

**27、什么是 Java 的序列化，如何实现 Java 的序列化？**

对象序列化是一个用于将对象状态转换为字节流的过程，可以将其保存到磁盘文件中或通过网络发送到任何其他程序。从字节流创建对象的相反的过程称为反序列化。而创建的字节流是与平台无关的，在一个平台上序列化的对象可以在不同的平台上反序列化。序列化是为了解决在对象流进行读写操作时所引发的问题。

序列化的实现：将需要被序列化的类实现 Serializable 接口，该接口没有需要实现的方法，只是用于标注该对象是可被序列化的，然后使用一个输出流（如：FileOutputStream）来构造一个 ObjectOutputStream 对象，接着使用 ObjectOutputStream 对象的 writeObject(Object obj) 方法可以将参数为 obj 的对象写出，要恢复的话则使用输入流。

**28、反射的优缺点？**

**优点：**

运行期类型的判断，class.forName() 动态加载类，提高代码的灵活度；

**缺点：**

尽管反射非常强大，但也不能滥用。如果一个功能可以不用反射完成，那么最好就不用。在我们使用反射技术时，下面几条内容应该牢记于心。

（1）性能开销 ：反射涉及了动态类型的解析，所以 JVM 无法对这些代码进行优化。因此，反射操作的效率要比那些非反射操作低得多。我们应该避免在经常被执行的代码或对性能要求很高的程序中使用反射。

（2）安全限制 ：使用反射技术要求程序必须在一个没有安全限制的环境中运行。如果一个程序必须在有安全限制的环境中运行，如 Applet，那么这就是个问题了。

（3）内部暴露：由于反射允许代码执行一些在正常情况下不被允许的操作（比如：访问私有的属性和方法），所以使用反射可能会导致意料之外的副作用，这可能导致代码功能失调并破坏可移植性。反射代码破坏了抽象性，因此当平台发生改变的时候，代码的行为就有可能也随着变化。

**29、Java 中的动态代理是什么？有哪些应用？**

动态代理：当想要给实现了某个接口的类中的方法，加一些额外的处理。比如说加日志，加事务等。可以给这个类创建一个代理，故名思议就是创建一个新的类，这个类不仅包含原来类方法的功能，而且还在原来的基础上添加了额外处理的新功能。这个代理类并不是定义好的，是动态生成的。具有解耦意义，灵活，扩展性强。

动态代理的应用：Spring 的 AOP 、加事务、加权限、加日志。

**30、怎么实现动态代理？**

首先必须定义一个接口，还要有一个 InvocationHandler（将实现接口的类的对象传递给它）处理类。再有一个工具类 Proxy（习惯性将其称为代理类，因为调用它的 newInstance() 可以产生代理对象，其实它只是一个产生代理对象的工具类）。利用到 InvocationHandler，拼接代理类源码，将其编译生成代理类的二进制码，利用加载器加载，并将其实例化产生代理对象，最后返回。

每一个动态代理类都必须要实现 InvocationHandler 这个接口，并且每个代理类的实例都关联到了一个 handler，当我们通过代理对象调用一个方法的时候，这个方法的调用就会被转发为由 InvocationHandler 这个接口的 invoke 方法来进行调用。我们来看看 InvocationHandler 这个接口的唯一一个方法 invoke 方法：

```java
Object invoke(Object proxy, Method method, Object[] args) throws Throwable
```

proxy: 指代我们所代理的那个真实对象

method: 指代的是我们所要调用真实对象的某个方法的 Method 对象

args: 指代的是调用真实对象某个方法时接受的参数

Proxy 类的作用是动态创建一个代理对象的类。它提供了许多的方法，但是我们用的最多的就是 newProxyInstance 这个方法：

```java
public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces,  InvocationHandler handler)  throws IllegalArgumentException
```

loader：一个 ClassLoader 对象，定义了由哪个 ClassLoader 对象来对生成的代理对象进行加载；

interfaces：一个 Interface 对象的数组，表示的是我将要给我需要代理的对象提供一组什么接口，如果我提供了一组接口给它，那么这个代理对象就宣称实现了该接口(多态)，这样我就能调用这组接口中的方法了 

handler：一个 InvocationHandler 对象，表示的是当我这个动态代理对象在调用方法的时候，会关联到哪一个 InvocationHandler 对象上。

通过 Proxy.newProxyInstance 创建的代理对象是在 Jvm 运行时动态生成的一个对象，它并不是我们的 InvocationHandler 类型，也不是我们定义的那组接口的类型，而是在运行是动态生成的一个对象。

**31、Java 中的 IO 流的分类？说出几个你熟悉的实现类？**

按功能来分：输入流（input）、输出流（output）。

按类型来分：字节流 和 字符流。

字节流：InputStream/OutputStream 是字节流的抽象类，这两个抽象类又派生了若干子类，不同的子类分别处理不同的操作类型。具体子类如下所示：

![img](https://gitee.com/xudongyin/img/raw/master/img/20200923135224)

字符流：Reader/Writer 是字符的抽象类，这两个抽象类也派生了若干子类，不同的子类分别处理不同的操作类型。

![img](https://gitee.com/xudongyin/img/raw/master/img/20200923135230)

**32、字节流和字符流有什么区别？**

字节流按 8 位传输，以字节为单位输入输出数据，字符流按 16 位传输，以字符为单位输入输出数据。

但是不管文件读写还是网络发送接收，信息的最小存储单元都是字节。

**33、引用类型和基本类型**

java语言是强类型语言，支持的类型分为两类：基本类型和引用类型。

基本类型包括boolean类型和数值类型，数值类型有整数类型和浮点类型。整数类型包括：byte、short、int、long和char；浮点类型包括：float和double

引用类型包括类、接口和数组类型以及特殊的null类型。



## 集合篇

**1、Java 中常用的容器有哪些？**

常见容器主要包括 Collection 和 Map 两种，Collection 存储着对象的集合，而 Map 存储着键值对（两个对象）的映射表。

![img](https://gitee.com/xudongyin/img/raw/master/img/20200924105131)

![img](https://gitee.com/xudongyin/img/raw/master/img/20200924105135)

**Collection**

- **Set**

1. TreeSet：基于**红黑树**实现，支持有序性操作，例如：根据一个范围查找元素的操作。但是查找效率不如 HashSet，**HashSet 查找的时间复杂度为 O(1)，TreeSet 则为 O(logN)**。
2. HashSet：基于**哈希表**实现，支持快速查找，但不支持有序性操作。并且失去了元素的插入顺序信息，也就是说使用 Iterator 遍历 HashSet 得到的结果是不确定的。
3. LinkedHashSet：具有 HashSet 的查找效率，且**内部使用双向链表维护元素的插入顺序**。

- **List**

1. ArrayList：基于**动态数组**实现，支持随机访问。
2. Vector：和 ArrayList 类似，但它是**线程安全**的。
3. LinkedList：基于**双向链表**实现，只能顺序访问，但是可以快速地在链表中间插入和删除元素。不仅如此，**LinkedList 还可以用作栈、队列和双向队列**。

- **Queue**

1. LinkedList：可以用它来实现**双向队列**。
2. PriorityQueue：基于**堆结构**实现，可以用它来**实现优先队列**。

- **Map**

1. TreeMap：基于**红黑树**实现。
2. HashMap：基于**哈希表**实现。
3. HashTable：和 HashMap 类似，但它是线程安全的，这意味着同一时刻多个线程可以同时写入 HashTable 并且不会导致数据不一致。它是遗留类，不应该去使用它。现在可以使用 ConcurrentHashMap 来支持线程安全，并且 ConcurrentHashMap 的效率会更高，因为 ConcurrentHashMap 引入了**分段锁**。
4. LinkedHashMap：使用**双向链表**来维护元素的顺序，顺序为插入顺序或者最近最少使用（LRU）顺序。



**2、ArrayList 和 LinkedList 的区别？**

**ArrayList：**底层是基于数组实现的，查找快，增删较慢；

**LinkedList：**底层是基于链表实现的。确切的说是循环双向链表（JDK1.6 之前是双向循环链表、JDK1.7 之后取消了循环），查找慢、增删快。LinkedList 链表由一系列表项连接而成，一个表项包含 3 个部分：元素内容、前驱表和后驱表。链表内部有一个 header 表项，既是链表的开始也是链表的结尾。header 的后继表项是链表中的第一个元素，header 的前驱表项是链表中的最后一个元素。

> **补充：**

**ArrayList 的增删未必就是比 LinkedList 要慢：**

1. 如果增删都是在末尾来操作【每次调用的都是 remove() 和 add()】，此时 ArrayList 就不需要移动和复制数组来进行操作了。如果数据量有百万级的时候，速度是会比 LinkedList 要快的。

2. 如果删除操作的位置是在中间。由于 LinkedList 的消耗主要是在遍历上，ArrayList 的消耗主要是在移动和复制上（底层调用的是 arrayCopy() 方法，是 native 方法）。LinkedList 的遍历速度是要慢于 ArrayList 的复制移动速度的，如果数据量有百万级的时候，还是 ArrayList 要快。

   

**3、ArrayList 实现 RandomAccess 接口有何作用？为何 LinkedList 却没实现这个接口？**

1. RandomAccess 接口只是一个标志接口，只要 List 集合实现这个接口，就能支持快速随机访问。通过查看 Collections 类中的 binarySearch() 方法，可以看出，判断 List 是否实现 RandomAccess 接口来实行 indexedBinarySerach(list, key) 或 iteratorBinarySerach(list, key) 方法。再通过查看这两个方法的源码发现：实现 RandomAccess 接口的 List 集合采用一般的 for 循环遍历，而未实现这接口则采用迭代器，即 ArrayList 一般采用 for 循环遍历，而 LinkedList 一般采用迭代器遍历；

2. ArrayList 用 for 循环遍历比 iterator 迭代器遍历快，LinkedList 用 iterator 迭代器遍历比 for 循环遍历快。所以说，当我们在做项目时，应该考虑到 List 集合的不同子类采用不同的遍历方式，能够提高性能。



**4、ArrayList 的扩容机制？**

推荐阅读：

**https://juejin.im/post/5d42ab5e5188255d691bc8d6**

1. 当使用 add 方法的时候首先调用 ensureCapacityInternal 方法，传入 size+1 进去，检查是否需要扩充 elementData 数组的大小；

2. newCapacity = 扩充数组为原来的 1.5 倍(不能自定义)，如果还不够，就使用它需要扩充的大小 minCapacity ，然后判断 minCapacity 是否大于 MAX_ARRAY_SIZE(Integer.MAX_VALUE - 8) ，如果大于，就取 Integer.MAX_VALUE；

3. 扩容的主要方法：grow；

4. ArrayList 中 copy 数组的核心就是 System.arraycopy 方法，将 original 数组的所有数据复制到 copy 数组中，这是一个本地方法。

   

**5、Array 和 ArrayList 有何区别？什么时候更适合用 Array？**

1. Array 可以容纳基本类型和对象，而 ArrayList 只能容纳对象；
2. Array 是指定大小的，而 ArrayList 大小是固定的。

> **什么时候更适合使用 Array：**

1. 如果列表的大小已经指定，大部分情况下是存储和遍历它们；
2. 对于遍历基本数据类型，尽管 Collections 使用自动装箱来减轻编码任务，在指定大小的基本类型的列表上工作也会变得很慢；
3. 如果你要使用多维数组，使用 Array 比 ArrayList 更容易。



**6、HashMap 的实现原理/底层数据结构？JDK1.7 和 JDK1.8**

**JDK1.7**：Entry数组 + 链表

**JDK1.8**：Node 数组 + 链表/红黑树，当链表上的元素个数超过 8 个并且数组长度 >= 64 时自动转化成红黑树，节点变成树节点，以提高搜索效率和插入效率到 O(logN)。Entry 和 Node 都包含 key、value、hash、next 属性。



**7、HashMap 的 put 方法的执行过程？**

当我们想往一个 HashMap 中添加一对 key-value 时，系统首先会计算 key 的 hash 值，然后根据 hash 值确认在 table 中存储的位置。若该位置没有元素，则直接插入。否则迭代该处元素链表并依次比较其 key 的 hash 值。如果两个 hash 值相等且 key 值相等(e.hash == hash && ((k = e.key) == key || key.equals(k)))，则用新的 Entry 的 value 覆盖原来节点的 value。如果两个 hash 值相等但 key 值不等 ，则将该节点插入该链表的链头。



**8、HashMap 的 get 方法的执行过程？**

通过 key 的 hash 值找到在 table 数组中的索引处的 Entry，然后返回该 key 对应的 value 即可。

在这里能够根据 key 快速的取到 value 除了和 HashMap 的数据结构密不可分外，还和 Entry 有莫大的关系。HashMap 在存储过程中并没有将 key，value 分开来存储，而是当做一个整体 key-value 来处理的，这个整体就是Entry 对象。同时 value 也只相当于 key 的附属而已。在存储的过程中，系统根据 key 的 HashCode 来决定 Entry 在 table 数组中的存储位置，在取的过程中同样根据 key 的 HashCode 取出相对应的 Entry 对象（value 就包含在里面）。



**9、HashMap 的 resize 方法的执行过程？**

**有两种情况会调用 resize 方法：**

1. 第一次调用 HashMap 的 put 方法时，会调用 resize 方法对 table 数组进行初始化，如果不传入指定值，默认大小为 16。

2. 扩容时会调用 resize，即 size &gt; threshold 时，table 数组大小翻倍。

每次扩容之后容量都是**翻倍**。扩容后要将原数组中的所有元素找到在新数组中合适的位置。

当我们把 table[i] 位置的所有 Node 迁移到 newtab 中去的时候：这里面的 node 要么在 newtab 的 i 位置（不变），要么在 newtab 的 i + n (旧数组长度) 位置。也就是我们可以这样处理：把 table[i] 这个桶中的 node 拆分为两个链表 l1 和 l2：如果 hash & n == 0，那么当前这个 node 被连接到 l1 链表；否则连接到 l2 链表。这样下来，当遍历完 table[i] 处的所有 node 的时候，我们得到两个链表 l1 和 l2，这时我们令 newtab[i] = l1，newtab[i + n] = l2，这就完成了 table[i] 位置所有 node 的迁移（rehash），这也是 HashMap 中容量一定的是 2 的整数次幂带来的方便之处。



**10、HashMap 的 size 为什么必须是 2 的整数次方？**

1. 这样做总是能够保证 HashMap 的底层数组长度为 2 的 n 次方。当 length 为 2 的 n 次方时，h & (length - 1) 就相当于h 对 length 取模，而且速度比直接取模快得多，这是 HashMap 在速度上的一个优化。而且每次扩容时都是翻倍。
2. 如果 length 为 2 的次幂，则 length - 1 转化为二进制必定是 11111……的形式，在与 h 的二进制进行与操作时效率会非常的快，而且空间不浪费。但是，如果 length 不是 2 的次幂，比如：length 为 15，则 length - 1 为 14，对应的二进制为 1110，在于 h 与操作，最后一位都为 0 ，而 0001，0011，0101，1001，1011，0111，1101 这几个位置永远都不能存放元素了，空间浪费相当大，更糟的是这种情况中，数组可以使用的位置比数组长度小了很多，这意味着进一步增加了碰撞的几率，减慢了查询的效率，这样就会造成空间的浪费。



**11、HashMap 多线程死循环问题？**

详细请阅读：

https://blog.csdn.net/xuefeng0707/article/details/40797085

https://blog.csdn.net/dgutliangxuan/article/details/78779448

主要是多线程同时 put 时，如果有两个线程同时进行扩容，同时触发了 rehash 操作，一个线程先在刚进入扩容循环的时候，线程暂停调度CPU，把执行权让个给另一个线程，然后得到执行权的线程先扩容完成，接着暂停的线程获取CPU调度，继续进行扩容，会导致 HashMap 重新 hash 后搬到新数组的链表中出现循环节点，进而使得后面 get  HashMap 不存在的值的时候，会死循环。因为 get 值是遍历 HashMap 的键。

JDK1.8后，除了对 HashMap 增加红黑树结果外，对原有造成死锁的关键原因点（新table复制在头端添加元素）改进为依次在末端添加新的元素。虽然JDK1.8后添加红黑树改进了链表过长查询遍历慢问题和 resize 时出现导致put死循环的bug，但还是非线性安全的，比如数据丢失等等。因此多线程情况下还是建议使用concurrenthashmap。

Hashmap在jdk1.8的时候也会出现死循环的情况，是在链表转换为树或者对树进行重新平衡操作的时候for循环一直无法跳出，导致死循环。



**12、HashMap 的 get 方法能否判断某个元素是否在 map 中？**

HashMap 的 get 函数的返回值不能判断一个 key 是否包含在 map 中，因为 get 返回 null 有可能是不包含该 key，也有可能该 key 对应的 value 为 null。因为 HashMap 中允许 key 为 null，也允许 value 为 null。



**13、HashMap 与 HashTable 的区别是什么？**

1. HashTable 基于 Dictionary 类，而 HashMap 是基于 AbstractMap。Dictionary 是任何可将键映射到相应值的类的抽象父类，而 AbstractMap 是基于 Map 接口的实现，它以最大限度地减少实现此接口所需的工作。
2. HashMap 的 key 和 value 都允许为 null，而 Hashtable 的 key 和 value 都不允许为 null。HashMap 遇到 key 为 null 的时候，调用 putForNullKey 方法进行处理，而对 value 没有处理；Hashtable 遇到 null，直接返回 NullPointerException。
3. Hashtable 是线程安全的，而 HashMap 不是线程安全的，但是我们也可以通过 Collections.synchronizedMap(hashMap)，使其实现同步。

> **HashTable的补充：**

HashTable 和 HashMap 的实现原理几乎一样，差别无非是

1. HashTable 不允许 key 和 value 为 null；

2. HashTable 是线程安全的。但是 HashTable 线程安全的策略实现代价却太大了，简单粗暴，get/put 所有相关操作都是 synchronized 的，这相当于给整个哈希表加了一把大锁，多线程访问时候，只要有一个线程访问或操作该对象，那其他线程只能阻塞，相当于将所有的操作串行化，在竞争激烈的并发场景中性能就会非常差。



**14、HashMap 与 ConcurrentHashMap 的区别是什么?**

HashMap 不是线程安全的，而 ConcurrentHashMap 是线程安全的。

ConcurrentHashMap 采用锁分段技术，将整个Hash桶进行了分段segment，也就是将这个大的数组分成了几个小的片段 segment，而且每个小的片段 segment 上面都有锁存在，那么在插入元素的时候就需要先找到应该插入到哪一个片段 segment，然后再在这个片段上面进行插入，而且这里还需要获取 segment 锁，这样做明显减小了锁的粒度。



**15、HashTable 和 ConcurrentHashMap 的区别？**

HashTable 和 ConcurrentHashMap 相比，效率低。 Hashtable 之所以效率低主要是使用了 synchronized 关键字对 put 等操作进行加锁，而 synchronized 关键字加锁是对整张 Hash 表的，即每次锁住整张表让线程独占，致使效率低下，而 ConcurrentHashMap 在对象中保存了一个 Segment 数组，即将整个 Hash 表划分为多个分段；而每个Segment元素，即每个分段则类似于一个Hashtable；这样，在执行 put 操作时首先根据 hash 算法定位到元素属于哪个 Segment，然后对该 Segment 加锁即可，因此， ConcurrentHashMap 在多线程并发编程中可是实现多线程 put操作。



**16、ConcurrentHashMap 的实现原理是什么？**

**数据结构**

JDK 7：中 ConcurrentHashMap 采用了数组 + Segment + 分段锁的方式实现。

JDK 8：中 ConcurrentHashMap 参考了 JDK 8 HashMap 的实现，采用了数组 + 链表 + 红黑树的实现方式来设计，内部大量采用 CAS 操作。

ConcurrentHashMap 采用了非常精妙的"分段锁"策略，ConcurrentHashMap 的主干是个 Segment 数组。

```java
final  Segment<K,V>[]  segments;
```

Segment 继承了 ReentrantLock，所以它就是一种可重入锁（ReentrantLock)。在 ConcurrentHashMap，一个 Segment 就是一个子哈希表，Segment 里维护了一个 HashEntry 数组，并发环境下，对于不同 Segment 的数据进行操作是不用考虑锁竞争的。就按默认的 ConcurrentLevel 为 16 来讲，理论上就允许 16 个线程并发执行。

所以，对于同一个 Segment 的操作才需考虑线程同步，不同的 Segment 则无需考虑。Segment 类似于 HashMap，一个 Segment 维护着一个HashEntry 数组：

```java
transient  volatile  HashEntry<K,V>[]  table;
```

HashEntry 是目前我们提到的最小的逻辑处理单元了。一个 ConcurrentHashMap 维护一个 Segment 数组，一个 Segment 维护一个 HashEntry 数组。因此，ConcurrentHashMap 定位一个元素的过程需要进行两次 Hash 操作。第一次 Hash 定位到 Segment，第二次 Hash 定位到元素所在的链表的头部。



**17、HashSet 的实现原理？**

HashSet 的实现是依赖于 HashMap 的，HashSet 的值都是存储在 HashMap 中的。在 HashSet 的构造法中会初始化一个 HashMap 对象，HashSet 不允许值重复。因此，HashSet 的值是作为 HashMap 的 key 存储在 HashMap 中的，当存储的值已经存在时返回 false。



**18、HashSet 怎么保证元素不重复的？**

```java
public boolean add(E e) {     return map.put(e, PRESENT)==null; }
```

元素值作为的是 map 的 key，map 的 value 则是 PRESENT 变量，这个变量只作为放入 map 时的一个占位符而存在，所以没什么实际用处。其实，这时候答案已经出来了：HashMap 的 key 是不能重复的，而这里HashSet 的元素又是作为了 map 的 key，当然也不能重复了。



**19、LinkedHashMap 的实现原理?**

LinkedHashMap 也是基于 HashMap 实现的，不同的是它定义了一个 Entry header，这个 header 不是放在 Table 里，它是额外独立出来的。LinkedHashMap 通过继承 hashMap 中的 Entry，并添加两个属性 Entry before，after 和 header 结合起来组成一个双向链表，来实现按插入顺序或访问顺序排序。

LinkedHashMap 定义了排序模式 accessOrder，该属性为 boolean 型变量，对于访问顺序，为 true；对于插入顺序，则为 false。一般情况下，不必指定排序模式，其迭代顺序即为默认为插入顺序。



**20、Iterator 怎么使用？有什么特点？**

迭代器是一种设计模式，它是一个对象，它可以遍历并选择序列中的对象，而开发人员不需要了解该序列的底层结构。迭代器通常被称为“轻量级”对象，因为创建它的代价小。Java 中的 Iterator 功能比较简单，并且只能单向移动：　　

1. 使用方法 iterator() 要求容器返回一个 Iterator。第一次调用 Iterator 的 next() 方法时，它返回序列的第一个元素。注意：iterator() 方法是 java.lang.Iterable 接口，被 Collection 继承。　　
2. 使用 next() 获得序列中的下一个元素。　
3. 使用 hasNext() 检查序列中是否还有元素。　　
4. 使用 remove() 将迭代器新返回的元素删除。　



**21、 Iterator 和 ListIterator 有什么区别？**

Iterator 可用来遍历 Set 和 List 集合，但是 ListIterator 只能用来遍历 List。Iterator 对集合只能是前向遍历，ListIterator 既可以前向也可以后向。ListIterator 实现了 Iterator 接口，并包含其他的功能，比如：增加元素，替换元素，获取前一个和后一个元素的索引等等。



**22、Iterator 和 Enumeration 接口的区别？**

与 Enumeration 相比，Iterator 更加安全，因为当一个集合正在被遍历的时候，它会阻止其它线程去修改集合。否则会抛出 ConcurrentModificationException 异常。这其实就是 fail-fast 机制。具体区别有三点：

1. Iterator 的方法名比 Enumeration 更科学；
2. Iterator 有 fail-fast 机制，比 Enumeration 更安全；
3. Iterator 能够删除元素，Enumeration 并不能删除元素。



**23、fail-fast 与 fail-safe 有什么区别？**

Iterator 的 fail-fast 属性与当前的集合共同起作用，因此它不会受到集合中任何改动的影响。Java.util 包中的所有集合类都被设计为 fail-fast 的，而 java.util.concurrent 中的集合类都为 fail-safe 的。当检测到正在遍历的集合的结构被改变时，fail-fast 迭代器抛出ConcurrentModificationException，而 fail-safe 迭代器从不抛出 ConcurrentModificationException。



**24、Collection 和 Collections 有什么区别？**

Collection：是最基本的集合接口，一个 Collection 代表一组 Object，即 Collection 的元素。它的直接继承接口有 List，Set 和 Queue。

Collections：是不属于 Java 的集合框架的，它是集合类的一个工具类/帮助类。此类不能被实例化， 服务于 Java 的 Collection 框架。它包含有关集合操作的静态多态方法，实现对各种集合的搜索、排序、线程安全等操作。



## 并发篇

**1、线程和进程的区别？**

**进程：**是程序运行和资源分配的基本单位，一个程序至少有一个进程，一个进程至少有一个线程。进程在执行过程中拥有独立的内存单元，而多个线程共享内存资源，减少切换次数，从而效率更高。

**线程：**是进程的一个实体，是 cpu 调度和分派的基本单位，是比程序更小的能独立运行的基本单位。同一进程中的多个线程之间可以并发执行。

**2、守护线程是什么？**

守护线程（即 Daemon thread），是个服务线程，准确地来说就是服务其他的线程。

**3、创建线程的几种方式？**

1. 继承 Thread 类创建线程；

2. 实现 Runnable 接口创建线程；

3. 通过 Callable 和 Future 创建线程；

4. 通过线程池创建线程。

**4、Runnable 和 Callable 有什么区别？**

1. Runnable 接口中的 run() 方法的返回值是 void，它做的事情只是纯粹地去执行 run() 方法中的代码而已；

2. Callable 接口中的 call() 方法是有返回值的，是一个泛型，和 Future、FutureTask 配合可以用来获取异步执行的结果。

**5、线程状态及转换？**

Thread 的源码中定义了6种状态：new（新建）、runnnable（可运行）、blocked（阻塞）、waiting（等待）、time waiting （定时等待）和 terminated（终止）。

![img](https://gitee.com/xudongyin/img/raw/master/img/20200927224959)

线程状态转换如下图所示：

![img](https://gitee.com/xudongyin/img/raw/master/img/20200927225011)

**6、sleep() 和 wait() 的区别？**

1. sleep() 方法正在执行的线程主动让出 cpu（然后 cpu 就可以去执行其他任务），在 sleep 指定时间后 cpu 再回到该线程继续往下执行（注意：sleep 方法只让出了 cpu，而并不会释放同步资源锁）；而 wait() 方法则是指当前线程让自己暂时退让出同步资源锁，以便其他正在等待该资源的线程得到该资源进而运行，只有调用了 notify() 方法，之前调用 wait() 的线程才会解除 wait 状态，可以去参与竞争同步资源锁，进而得到执行。（注意：notify 的作用相当于叫醒睡着的人，而并不会给他分配任务，就是说 notify 只是让之前调用 wait 的线程有权利重新参与线程的调度）；

2. sleep() 方法可以在任何地方使用，而 wait() 方法则只能在同步方法或同步块中使用；

3. sleep() 是线程类（Thread）的方法，调用会暂停此线程指定的时间，但监控依然保持，不会释放对象锁，到时间自动恢复；wait() 是 Object 的方法，调用会放弃对象锁，进入等待队列，待调用 notify() / notifyAll() 唤醒指定的线程或者所有线程，才会进入锁池，再次获得对象锁才会进入运行状态。

**7、线程的 run() 和 start() 有什么区别？**

1. 每个线程都是通过某个特定 Thread 对象所对应的方法 run() 来完成其操作的，方法 run() 称为线程体。通过调用 Thread 类的 start() 方法来启动一个线程；

2. start() 方法来启动一个线程，真正实现了多线程运行。这时无需等待 run() 方法体代码执行完毕，可以直接继续执行下面的代码；这时此线程是处于就绪状态，并没有运行。然后通过此 Thread 类调用方法 run() 来完成其运行状态，这里方法 run() 称为线程体，它包含了要执行的这个线程的内容，run() 方法运行结束，此线程终止。然后 cpu 再调度其它线程；

3. run() 方法是在本线程里的，只是线程里的一个函数，而不是多线程的。如果直接调用 run()，其实就相当于是调用了一个普通函数而已，直接调用 run() 方法必须等待 run() 方法执行完毕才能执行下面的代码，所以执行路径还是只有一条，根本就没有线程的特征，所以在多线程执行时要使用 start() 方法而不是 run() 方法。

**8、在 Java 程序中怎么保证多线程的运行安全？**

线程安全在三个方面体现：

原子性：提供互斥访问，同一时刻只能有一个线程对数据进行操作，（atomic，synchronized）；

可见性：一个线程对主内存的修改可以及时地被其他线程看到，（synchronized、volatile）；

有序性：一个线程观察其他线程中的指令执行顺序，由于指令重排序，该观察结果一般杂乱无序，（happens-before 原则）。

**9、Java 线程同步的几种方法？**

1. 使用 Synchronized 关键字；

2. wait 和 notify；

3. 使用特殊域变量 volatile 实现线程同步；

4. 使用可重入锁实现线程同步；

5. 使用阻塞队列实现线程同步；

6. 使用信号量 Semaphore。

**10、Thread.interrupt() 方法的工作原理是什么？**

在 Java 中，线程的中断 interrupt 只是改变了线程的中断状态，至于这个中断状态改变后带来的结果，那是无法确定的，有时它更是让停止中的线程继续执行的唯一手段。不但不是让线程停止运行，反而是继续执行线程的手段。

在一个线程对象上调用 interrupt() 方法，真正有影响的是 wait、join、sleep 方法，当然这 3 个方法包括它们的重载方法。请注意：上面这三个方法都会抛出 InterruptedException。

1. 对于 wait 中的等待 notify、notifyAll 唤醒的线程，其实这个线程已经“暂停”执行，因为它正在某一对象的休息室中，这时如果它的中断状态被改变，那么它就会抛出异常。这个 InterruptedException 异常不是线程抛出的，而是 wait 方法，也就是对象的 wait 方法内部会不断检查在此对象上休息的线程的状态，如果发现哪个线程的状态被置为已中断，则会抛出 InterruptedException，意思就是这个线程不能再等待了，其意义就等同于唤醒它了，然后执行 catch 中的代码。

2. 对于 sleep 中的线程，如果你调用了 Thread.sleep(一年)；现在你后悔了，想让它早些醒过来，调用 interrupt() 方法就是唯一手段，只有改变它的中断状态，让它从 sleep 中将控制权转到处理异常的 catch 语句中，然后再由 catch 中的处理转换到正常的逻辑。同样，对于 join 中的线程你也可以这样处理。

**11、谈谈对 ThreadLocal 的理解？**

1. Java 的 Web 项目大部分都是基于 Tomcat。每次访问都是一个新的线程，每一个线程都独享一个 ThreadLocal，我们可以在接收请求的时候 set 特定内容，在需要的时候 get 这个值。

2. ThreadLocal 提供 get 和 set 方法，为每一个使用这个变量的线程都保存有一份独立的副本。

```java
public T get() {...}
public void set(T value) {...}
public void remove() {...}
protected T initialValue() {...}
```

1. get() 方法是用来获取 ThreadLocal 在当前线程中保存的变量副本；

2. set() 用来设置当前线程中变量的副本；

3. remove() 用来移除当前线程中变量的副本；

4. initialValue() 是一个 protected 方法，一般是用来在使用时进行重写的，如果在没有 set 的时候就调用 get，会调用 initialValue 方法初始化内容。

**12、说一说自己对于 synchronized 关键字的了解？**

synchronized关键字解决的是多个线程之间访问资源的同步性，synchronized 关键字可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。

另外，在 Java 早期版本中，synchronized 属于重量级锁，效率低下，因为监视器锁（monitor）是依赖于底层的操作系统的 Mutex Lock 来实现的，Java 的线程是映射到操作系统的原生线程之上的。如果要挂起或者唤醒一个线程，都需要操作系统帮忙完成，而操作系统实现线程之间的切换时需要从用户态转换到内核态，这个状态之间的转换需要相对比较长的时间，时间成本相对较高，这也是为什么早期的 synchronized 效率低的原因。庆幸的是在 JDK6 之后 Java 官方对从 JVM 层面对synchronized 较大优化，所以现在的 synchronized 锁效率也优化得很不错了。JDK6 对锁的实现引入了大量的优化，如自旋锁、适应性自旋锁、锁消除、锁粗化、偏向锁、轻量级锁等技术来减少锁操作的开销。

**13、讲一下 synchronized 关键字的底层原理？**

synchronized 关键字底层原理属于 JVM 层面。

- **synchronized 同步语句块的情况**

```java
public class SynchronizedDemo {    
    public void method(){        
        synchronized(this){           
            System.out.println("manong qiuzhi xiaozhushou");        
        }    
    }
}
```

通过 JDK 自带的 javap 命令查看 SynchronizedDemo 类的相关字节码信息：首先切换到类的对应目录执行 javac SynchronizedDemo.java 命令生成编译后的 .class 文件，然后执行 javap -c -s -v -l SynchronizedDemo.class。

![img](https://gitee.com/xudongyin/img/raw/master/img/20200929095239)

从上面我们可以看出：synchronized 同步语句块的实现使用的是 monitorenter 和 monitorexit 指令，其中 monitorenter 指令指向同步代码块的开始位置，monitorexit 指令则指明同步代码块的结束位置。

当执行 monitorenter 指令时，线程试图获取锁也就是获取 monitor的持有权。monitor 对象存在于每个 Java 对象的对象头中，synchronized 锁便是通过这种方式获取锁的，也是为什么 Java 中任意对象可以作为锁的原因。当计数器为 0 则可以成功获取，获取后将锁计数器设为 1 也就是加 1。相应的在执行 monitorexit 指令后，将锁计数器设为 0，表明锁被释放。如果获取对象锁失败，那当前线程就要阻塞等待，直到锁被另外一个线程释放为止。

- **synchronized 修饰方法的的情况**

```java
public class SynchronizedDemo2 {    
    public synchronized void method() {        
        System.out.println("manong qiuzhi xiaozhushou");    
    }
}
```

synchronized 修饰的方法并没有 monitorenter 指令和 monitorexit 指令，取得代之的确实是 ACC_SYNCHRONIZED 标识，该标识指明了该方法是一个同步方法，JVM 通过该 ACC_SYNCHRONIZED 访问标志来辨别一个方法是否声明为同步方法，从而执行相应的同步调用。

**14、如何在项目中使用 synchronized 的？**

synchronized 关键字最主要的三种使用方式：

1. 修饰实例方法：作用于当前对象实例加锁，进入同步代码前要获得当前对象实例的锁；

2. 修饰静态方法：作用于当前类对象加锁，进入同步代码前要获得当前类对象的锁 。也就是给当前类加锁，会作用于类的所有对象实例，因为静态成员不属于任何一个实例对象，是类成员（static 表明这是该类的一个静态资源，不管 new了多少个对象，只有一份，所以对该类的所有对象都加了锁）。所以如果一个线程 A 调用一个实例对象的非静态 synchronized 方法，而线程 B 需要调用这个实例对象所属类的静态 synchronized 方法，是允许的，不会发生互斥现象，因为访问静态 synchronized 方法占用的锁是当前类的锁，而访问非静态 synchronized 方法占用的锁是当前实例对象锁；

3. 修饰代码块：指定加锁对象，对给定对象加锁，进入同步代码库前要获得给定对象的锁。和 synchronized 方法一样，synchronized(this) 代码块也是锁定当前对象的。synchronized 关键字加到 static 静态方法和 synchronized(class) 代码块上都是是给 Class 类上锁。这里再提一下：synchronized 关键字加到非 static 静态方法上是给对象实例上锁。另外需要注意的是：尽量不要使用 synchronized(String a) 因为 JVM 中，字符串常量池具有缓冲功能。

> **补充：双重校验锁实现单例模式**

  问到 synchronized 的使用，很有可能让你用 synchronized 实现个单例模式。这里补充下使用 synchronized 双重校验锁的方法实现单例模式：

```java
public class Singleton {
    private volatile static Singleton uniqueInstance;    
    private Singleton() {}
    public static Singleton getUniqueInstance() {       
        // 先判断对象是否已经实例过，没有实例化过才进入加锁代码        
        if (uniqueInstance == null) {            
            // 类对象加锁            
            synchronized (Singleton.class) {                
                if (uniqueInstance == null) {                    
                    uniqueInstance = new Singleton();               
                }            
            }        
        }        
        return uniqueInstance;    
    }
}
```

另外，需要注意 uniqueInstance 采用 volatile 关键字修饰也是很有必要。采用 volatile 关键字修饰也是很有必要的， uniqueInstance = new Singleton(); 这段代码其实是分为三步执行：

1. 为 uniqueInstance 分配内存空间

2. 初始化 uniqueInstance

3. 将 uniqueInstance 指向分配的内存地址

但是由于 JVM 具有指令重排的特性，执行顺序有可能变成 1 -> 3 -> 2。指令重排在单线程环境下不会出现问题，但是在多线程环境下会导致一个线程获得还没有初始化的实例。例如，线程 T1 执行了 1 和 3，此时 T2 调用 getUniqueInstance() 后发现 uniqueInstance 不为空，因此返回 uniqueInstance，但此时 uniqueInstance 还未被初始化。

使用 volatile 可以禁止 JVM 的指令重排，保证在多线程环境下也能正常运行。

**15、说说 JDK1.6 之后的 synchronized 关键字底层做了哪些优化，可以详细介绍一下这些优化吗？**

说明：这道题答案有点长，但是回答的详细面试会很加分。

JDK1.6 对锁的实现引入了大量的优化，如偏向锁、轻量级锁、自旋锁、适应性自旋锁、锁消除、锁粗化等技术来减少锁操作的开销。

锁主要存在四种状态，依次是：无锁状态、偏向锁状态、轻量级锁状态、重量级锁状态，它们会随着竞争的激烈而逐渐升级。注意锁可以升级不可降级，这种策略是为了提高获得锁和释放锁的效率。

- **偏向锁**

引入偏向锁的目的和引入轻量级锁的目的很像，它们都是为了没有多线程竞争的前提下，减少传统的重量级锁使用操作系统互斥量产生的性能消耗。但是不同的是：轻量级锁在无竞争的情况下使用 CAS 操作去代替使用互斥量。而偏向锁在无竞争的情况下会把整个同步都消除掉。

偏向锁的“偏”就是偏心的偏，它的意思是会偏向于第一个获得它的线程，如果在接下来的执行中，该锁没有被其他线程获取，那么持有偏向锁的线程就不需要进行同步。

但是对于锁竞争比较激烈的场合，偏向锁就失效了，因为这样场合极有可能每次申请锁的线程都是不相同的，因此这种场合下不应该使用偏向锁，否则会得不偿失，需要注意的是，偏向锁失败后，并不会立即膨胀为重量级锁，而是先升级为轻量级锁。

- **轻量级锁**

倘若偏向锁失败，虚拟机并不会立即升级为重量级锁，它还会尝试使用一种称为轻量级锁的优化手段(JDK1.6 之后加入的)。轻量级锁不是为了代替重量级锁，它的本意是在没有多线程竞争的前提下，减少传统的重量级锁使用操作系统互斥量产生的性能消耗，因为使用轻量级锁时，不需要申请互斥量。另外，轻量级锁的加锁和解锁都用到了 CAS 操作。

轻量级锁能够提升程序同步性能的依据是“对于绝大部分锁，在整个同步周期内都是不存在竞争的”，这是一个经验数据。如果没有竞争，轻量级锁使用 CAS 操作避免了使用互斥操作的开销。但如果存在锁竞争，除了互斥量开销外，还会额外发生 CAS 操作，因此在有锁竞争的情况下，轻量级锁比传统的重量级锁更慢！如果锁竞争激烈，那么轻量级将很快膨胀为重量级锁！

- **自旋锁和自适应自旋**

轻量级锁失败后，虚拟机为了避免线程真实地在操作系统层面挂起，还会进行一项称为自旋锁的优化手段。

互斥同步对性能最大的影响就是阻塞的实现，因为挂起线程/恢复线程的操作都需要转入内核态中完成（用户态转换到内核态会耗费时间）。

一般线程持有锁的时间都不是太长，所以仅仅为了这一点时间去挂起线程/恢复线程是得不偿失的。所以，虚拟机的开发团队就这样去考虑：“我们能不能让后面来的请求获取锁的线程等待一会而不被挂起呢？看看持有锁的线程是否很快就会释放锁”。为了让一个线程等待，我们只需要让线程执行一个忙循环（自旋），这项技术就叫做自旋。

> **百度百科对自旋锁的解释：**

~~~
何谓自旋锁？它是为实现保护共享资源而提出一种锁机制。其实，自旋锁与互斥锁比较类似，它们都是为了解决对某项资源的互斥使用。无论是互斥锁，还是自旋锁，在任何时刻，最多只能有一个保持者，也就说，在任何时刻最多只能有一个执行单元获得锁。但是两者在调度机制上略有不同。对于互斥锁，如果资源已经被占用，资源申请者只能进入睡眠状态。但是自旋锁不会引起调用者睡眠，如果自旋锁已经被别的执行单元保持，调用者就一直循环在那里看是否该自旋锁的保持者已经释放了锁，"自旋"一词就是因此而得名。
~~~

自旋锁在 JDK1.6 之前其实就已经引入了，不过是默认关闭的，需要通过 --XX:+UseSpinning 参数来开启。JDK1.6 及 1.6 之后，就改为默认开启的了。需要注意的是：自旋等待不能完全替代阻塞，因为它还是要占用处理器时间。如果锁被占用的时间短，那么效果当然就很好了。反之，自旋等待的时间必须要有限度。如果自旋超过了限定次数任然没有获得锁，就应该挂起线程。自旋次数的默认值是 10 次，用户可以修改 --XX:PreBlockSpin 来更改。

另外，在 JDK1.6 中引入了自适应的自旋锁。自适应的自旋锁带来的改进就是：自旋的时间不再固定了，而是和前一次同一个锁上的自旋时间以及锁的拥有者的状态来决定，虚拟机变得越来越“聪明”了。

- **锁消除**

锁消除理解起来很简单，它指的就是虚拟机即使编译器在运行时，如果检测到那些共享数据不可能存在竞争，那么就执行锁消除。锁消除可以节省毫无意义的请求锁的时间。

- **锁粗化**

原则上，我们在编写代码的时候，总是推荐将同步块的作用范围限制得尽量小。只在共享数据的实际作用域才进行同步，这样是为了使得需要同步的操作数量尽可能变小，如果存在锁竞争，那等待线程也能尽快拿到锁。

大部分情况下，上面的原则都是没有问题的，但是如果一系列的连续操作都对同一个对象反复加锁和解锁，那么会带来很多不必要的性能消耗。

**16、谈谈 synchronized 和 ReenTrantLock 的区别？**

1. synchronized 是和 for、while 一样的关键字，ReentrantLock 是类，这是二者的本质区别。既然 ReentrantLock 是类，那么它就提供了比 synchronized 更多更灵活的特性：等待可中断、可实现公平锁、可实现选择性通知（锁可以绑定多个条件）、性能已不是选择标准。

2. synchronized 依赖于 JVM 而 ReenTrantLock 依赖于 API。synchronized 是依赖于 JVM 实现的，JDK1.6 为 synchronized 关键字进行了很多优化，但是这些优化都是在虚拟机层面实现的，并没有直接暴露给我们。ReenTrantLock 是 JDK 层面实现的（也就是 API 层面，需要 lock() 和 unlock 方法配合 try / finally 语句块来完成），所以我们可以通过查看它的源代码，来看它是如何实现的。

**17、synchronized 和 volatile 的区别是什么？**

1. volatile 本质是在告诉 JVM当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取；synchronized 则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住。

2. volatile 仅能使用在变量级别；synchronized 则可以使用在变量、方法、和类级别的。

3. volatile 仅能实现变量的修改可见性，不能保证原子性；而 synchronized 则可以保证变量的修改可见性和原子性。

4. volatile 不会造成线程的阻塞；synchronized 可能会造成线程的阻塞。

5. volatile 标记的变量不会被编译器优化（指令重排）；synchronized 标记的变量可以被编译器优化。

**18、谈一下你对 volatile 关键字的理解？**

volatile 关键字是用来保证有序性和可见性的。这跟 Java 内存模型有关。我们所写的代码，不一定是按照我们自己书写的顺序来执行的，编译器会做重排序，CPU 也会做重排序的，这样做是为了减少流水线阻塞，提高 CPU 的执行效率。这就需要有一定的顺序和规则来保证，不然程序员自己写的代码都不知道对不对了，所以有 happens-before 规则，其中有条就是 volatile 变量规则：对一个变量的写操作先行发生于后面对这个变量的读操作、有序性实现是通过插入内存屏障来保证的。

被 volatile 修饰的共享变量，就具有了以下两点特性：

1 . 保证了不同线程对该变量操作的内存可见性;

2 . 禁止指令重排序。

备注：这个题如果扩展了答，可以从 Java 的内存模型入手，下一篇 Java 虚拟机高频面试题中会讲到，这里不做过多赘述。

**19、说下对 ReentrantReadWriteLock 的理解？**

ReentrantReadWriteLock 允许多个读线程同时访问，但是不允许写线程和读线程、写线程和写线程同时访问。读写锁内部维护了两个锁：一个是用于读操作的 ReadLock，一个是用于写操作的 WriteLock。读写锁 ReentrantReadWriteLock 可以保证多个线程可以同时读，所以在读操作远大于写操作的时候，读写锁就非常有用了。

ReentrantReadWriteLock 基于 AQS 实现，它的自定义同步器（继承 AQS）需要在同步状态 state 上维护多个读线程和一个写线程，该状态的设计成为实现读写锁的关键。ReentrantReadWriteLock 很好的利用了高低位。来实现一个整型控制两种状态的功能，读写锁将变量切分成了两个部分，高 16 位表示读，低 16 位表示写。

- **ReentrantReadWriteLock 的特点：**

1. 写锁可以降级为读锁，但是读锁不能升级为写锁；

2. 不管是 ReadLock 还是 WriteLock 都支持 Interrupt，语义与 ReentrantLock 一致；

3. WriteLock 支持 Condition 并且与 ReentrantLock 语义一致，而 ReadLock 则不能使用 Condition，否则抛出 UnsupportedOperationException 异常；

4. 默认构造方法为非公平模式 ，开发者也可以通过指定 fair 为 true 设置为公平模式 。

- **升降级**

1. 读锁里面加写锁，会导致死锁；

2. 写锁里面是可以加读锁的，这就是锁的降级。

**20、说下对悲观锁和乐观锁的理解？**

- **悲观锁**

总是假设最坏的情况，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁（共享资源每次只给一个线程使用，其它线程阻塞，用完后再把资源转让给其它线程）。传统的关系型数据库里边就用到了很多这种锁机制，比如：行锁、表锁、读锁、写锁等，都是在做操作之前先上锁。Java 中 synchronized 和 ReentrantLock 等独占锁就是悲观锁思想的实现。

- **乐观锁**

总是假设最好的情况，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号机制和 CAS 算法实现。乐观锁适用于多读的应用类型，这样可以提高吞吐量，像数据库提供的类似于 write_condition 机制，其实都是提供的乐观锁。在 Java 中 java.util.concurrent.atomic 包下面的原子变量类就是使用了乐观锁的一种实现方式 CAS 实现的。

- **两种锁的使用场景**

从上面对两种锁的介绍，我们知道两种锁各有优缺点，不可认为一种好于另一种，像乐观锁适用于写比较少的情况下（多读场景），即冲突真的很少发生的时候，这样可以省去了锁的开销，加大了系统的整个吞吐量。但如果是多写的情况，一般会经常产生冲突，这就会导致上层应用会不断的进行 retry，这样反倒是降低了性能，所以一般多写的场景下用悲观锁就比较合适。

**21、乐观锁常见的两种实现方式是什么？**

乐观锁一般会使用版本号机制或者 CAS 算法实现。

- **版本号机制**

  一般是在数据表中加上一个数据版本号 version 字段，表示数据被修改的次数，当数据被修改时，version 值会加 1。当线程 A 要更新数据值时，在读取数据的同时也会读取 version 值，在提交更新时，若刚才读取到的 version 值为当前数据库中的 version 值相等时才更新，否则重试更新操作，直到更新成功。

- **CAS 算法**

  即 compare and swap（比较与交换），是一种有名的无锁算法。无锁编程，即不使用锁的情况下实现多线程之间的变量同步，也就是在没有线程被阻塞的情况下实现变量的同步，所以也叫非阻塞同步（Non-blocking Synchronization）。CAS 算法涉及到三个操作数：

1、需要读写的内存值 V

2、进行比较的值 A

3、拟写入的新值 B

当且仅当 V 的值等于 A 时，CAS 通过原子方式用新值 B 来更新 V 的值，否则不会执行任何操作（比较和替换是一个原子操作）。一般情况下是一个自旋操作，即不断的重试。

**22、乐观锁的缺点有哪些？**

- **1. ABA 问题**

如果一个变量 V 初次读取的时候是 A 值，并且在准备赋值的时候检查到它仍然是 A 值，那我们就能说明它的值没有被其他线程修改过了吗？很明显是不能的，因为在这段时间它的值可能被改为其他值，然后又改回 A，那 CAS 操作就会误认为它从来没有被修改过。这个问题被称为 CAS 操作的 "ABA" 问题。

  JDK 1.5 以后的AtomicStampedReference 类就提供了此种能力，其中的 compareAndSet 方法就是首先检查当前引用是否等于预期引用，并且当前标志是否等于预期标志，如果全部相等，则以原子方式将该引用和该标志的值设置为给定的更新值。

- **2. 循环时间长开销大**

自旋 CAS（也就是不成功就一直循环执行直到成功）如果长时间不成功，会给 CPU 带来非常大的执行开销。如果 JVM 能支持处理器提供的 pause 指令那么效率会有一定的提升，pause 指令有两个作用，第一：它可以延迟流水线执行指令（de-pipeline），使 CPU 不会消耗过多的执行资源，延迟的时间取决于具体实现的版本，在一些处理器上延迟时间是零。第二：它可以避免在退出循环的时候因内存顺序冲突（memory order violation）而引起 CPU 流水线被清空（CPU pipeline flush），从而提高 CPU 的执行效率。

- **3. 只能保证一个共享变量的原子操作**

CAS 只对单个共享变量有效，当操作涉及跨多个共享变量时 CAS 无效。 但是从 JDK 1.5 开始，提供了 AtomicReference 类来保证引用对象之间的原子性，你可以把多个变量放在一个对象里来进行 CAS 操作。所以我们可以使用锁或者利用 AtomicReference 类把多个共享变量合并成一个共享变量来操作。

**23、CAS 和 synchronized 的使用场景？**

简单的来说 CAS 适用于写比较少的情况下（多读场景，冲突一般较少），synchronized 适用于写比较多的情况下（多写场景，冲突一般较多）。

1. 对于资源竞争较少（线程冲突较轻）的情况，使用 synchronized 同步锁进行线程阻塞和唤醒切换以及用户态内核态间的切换操作额外浪费消耗 cpu 资源；而 CAS 基于硬件实现，不需要进入内核，不需要切换线程，操作自旋几率较少，因此可以获得更高的性能。

2. 对于资源竞争严重（线程冲突严重）的情况，CAS 自旋的概率会比较大，从而浪费更多的 CPU 资源，效率低于 synchronized。

**24、简单说下对 Java 中的原子类的理解？**

这里 Atomic 是指一个操作是不可中断的。即使是在多个线程一起执行的时候，一个操作一旦开始，就不会被其他线程干扰。所以，所谓原子类说简单点就是具有原子操作特征的类。

并发包 java.util.concurrent 的原子类都存放在 java.util.concurrent.atomic 下。根据操作的数据类型，可以将 JUC 包中的原子类分为 4 类：

- **1. 基本类型**

使用原子的方式更新基本类型：

AtomicInteger：整型原子类

AtomicLong：长整型原子类

AtomicBoolean ：布尔型原子类

- **2. 数组类型**

使用原子的方式更新数组里的某个元素：

AtomicIntegerArray：整型数组原子类

AtomicLongArray：长整型数组原子类

AtomicReferenceArray ：引用类型数组原子类

- **3. 引用类型**

AtomicReference：引用类型原子类

AtomicStampedReference：原子更新引用类型里的字段原子类

AtomicMarkableReference ：原子更新带有标记位的引用类型

- **4. 对象的属性修改类型**

AtomicIntegerFieldUpdater：原子更新整型字段的更新器

AtomicLongFieldUpdater：原子更新长整型字段的更新器

AtomicStampedReference ：原子更新带有版本号的引用类型。该类将整数值与引用关联起来，可用于解决原子的更新数据和数据的版本号，可以解决使用 CAS 进行原子更新时可能出现的 ABA 问题。

**25、atomic 的原理是什么？**

Atomic 包中的类基本的特性就是在多线程环境下，当有多个线程同时对单个（包括基本类型及引用类型）变量进行操作时，具有排他性，即当多个线程同时对该变量的值进行更新时，仅有一个线程能成功，而未成功的线程可以向自旋锁一样，继续尝试，一直等到执行成功。

Atomic 系列的类中的核心方法都会调用 unsafe 类中的几个本地方法。我们需要先知道一个东西就是 Unsafe 类，全名为：sun.misc.Unsafe，这个类包含了大量的对 C 代码的操作，包括很多直接内存分配以及原子操作的调用，而它之所以标记为非安全的，是告诉你这个里面大量的方法调用都会存在安全隐患，需要小心使用，否则会导致严重的后果，例如在通过 unsafe 分配内存的时候，如果自己指定某些区域可能会导致一些类似 C++ 一样的指针越界到其他进程的问题。

**26、说下对同步器 AQS 的理解？**

AQS 的全称为：AbstractQueuedSynchronizer，这个类在 java.util.concurrent.locks 包下面。AQS 是一个用来构建锁和同步器的框架，使用 AQS 能简单且高效地构造出应用广泛的大量的同步器，比如：我们提到的 ReentrantLock，Semaphore，其他的诸如ReentrantReadWriteLock，SynchronousQueue，FutureTask 等等皆是基于 AQS 的。当然，我们自己也能利用 AQS 非常轻松容易地构造出符合我们自己需求的同步器。

**27、AQS 的原理是什么？**

AQS 核心思想是：如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制 AQS 是用 CLH 队列锁实现的，即将暂时获取不到锁的线程加入到队列中。

![img](https://gitee.com/xudongyin/img/raw/master/img/20200929125918)

> **CLH队列：**

CLH(Craig, Landin, and Hagersten)队列是一个虚拟的双向队列（虚拟的双向队列即不存在队列实例，仅存在结点之间的关联关系）。AQS 是将每条请求共享资源的线程封装成一个 CLH 锁队列的一个结点（Node）来实现锁的分配。

AQS 使用一个 int 成员变量 (state) 来表示同步状态，通过内置的 FIFO 队列来完成获取资源线程的排队工作。AQS 使用 CAS 对该同步状态进行原子操作实现对其值的修改。

```java
// 共享变量，使用 volatile 修饰保证线程可见性
private volatile int state;
```

**28、AQS 对资源的共享模式有哪些？**

1. Exclusive（独占）：只有一个线程能执行，如：ReentrantLock，又可分为公平锁和非公平锁：

2. Share（共享）：多个线程可同时执行，如：Semaphore、CountDownLatch、 CyclicBarrier、ReadWriteLock。

**29、AQS 底层使用了模板方法模式，你能说出几个需要重写的方法吗？**

使用者继承 AbstractQueuedSynchronizer 并重写指定的方法。将 AQS 组合在自定义同步组件的实现中，并调用其模板方法，而这些模板方法会调用使用者重写的方法。

1. isHeldExclusively() ：该线程是否正在独占资源。只有用到 condition 才需要去实现它。

2. tryAcquire(int) ：独占方式。尝试获取资源，成功则返回 true，失败则返回 false。

3. tryRelease(int) ：独占方式。尝试释放资源，成功则返回 true，失败则返回 false。

4. tryAcquireShared(int) ：共享方式。尝试获取资源。负数表示失败；0 表示成功，但没有剩余可用资源；正数表示成功，且有剩余资源。

5. tryReleaseShared(int) ：共享方式。尝试释放资源，成功则返回 true，失败则返回 false。

**30、说下对信号量 Semaphore 的理解？**

synchronized 和 ReentrantLock 都是一次只允许一个线程访问某个资源，Semaphore (信号量)可以指定多个线程同时访问某个资源。

执行 acquire 方法阻塞，直到有一个许可证可以获得然后拿走一个许可证；每个 release 方法增加一个许可证，这可能会释放一个阻塞的 acquire 方法。然而，其实并没有实际的许可证这个对象，Semaphore 只是维持了一个可获得许可证的数量。Semaphore 经常用于限制获取某种资源的线程数量。当然一次也可以一次拿取和释放多个许可证，不过一般没有必要这样做。除了 acquire方法（阻塞）之外，另一个比较常用的与之对应的方法是 tryAcquire 方法，该方法如果获取不到许可就立即返回 false。

**31、CountDownLatch 和 CyclicBarrier 有什么区别？**

CountDownLatch 是计数器，只能使用一次，而 CyclicBarrier 的计数器提供 reset 功能，可以多次使用。

对于 CountDownLatch 来说，重点是“一个线程（多个线程）等待”，而其他的 N 个线程在完成“某件事情”之后，可以终止，也可以等待。而对于 CyclicBarrier，重点是多个线程，在任意一个线程没有完成，所有的线程都必须等待。

CountDownLatch 是计数器，线程完成一个记录一个，只不过计数不是递增而是递减，而 CyclicBarrier 更像是一个阀门，需要所有线程都到达，阀门才能打开，然后继续执行。

> **应用场景：**

**CountDownLatch 应用场景：**

1. 某一线程在开始运行前等待 n 个线程执行完毕：启动一个服务时，主线程需要等待多个组件加载完毕，之后再继续执行。

2. 实现多个线程开始执行任务的最大并行性。注意是并行性，不是并发，强调的是多个线程在某一时刻同时开始执行。类似于赛跑，将多个线程放到起点，等待发令枪响，然后同时开跑。

3. 死锁检测：一个非常方便的使用场景是，你可以使用 n 个线程访问共享资源，在每次测试阶段的线程数目是不同的，并尝试产生死锁。

**CyclicBarrier 应用场景：**

CyclicBarrier 可以用于多线程计算数据，最后合并计算结果的应用场景。比如：我们用一个 Excel 保存了用户所有银行流水，每个 Sheet 保存一个帐户近一年的每笔银行流水，现在需要统计用户的日均银行流水，先用多线程处理每个 sheet 里的银行流水，都执行完之后，得到每个 sheet 的日均银行流水，最后，再用 barrierAction 用这些线程的计算结果，计算出整个 Excel 的日均银行流水。

**32、说下对线程池的理解？为什么要使用线程池？**

线程池提供了一种限制和管理资源（包括执行一个任务）的方式。每个线程池还维护一些基本统计信息，例如：已完成任务的数量。

- **使用线程池的好处**

1. 降低资源消耗：通过重复利用已创建的线程降低线程创建和销毁造成的消耗;

2. 提高响应速度：当任务到达时，任务可以不需要的等到线程创建就能立即执行;

3. 提高线程的可管理性：线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。

**33、创建线程池的参数有哪些？**

1. corePoolSize（线程池的基本大小）：当提交一个任务到线程池时，如果当前 poolSize < corePoolSize 时，线程池会创建一个线程来执行任务，即使其他空闲的基本线程能够执行新任务也会创建线程，等到需要执行的任务数大于线程池基本大小时就不再创建。如果调用了线程池的prestartAllCoreThreads() 方法，线程池会提前创建并启动所有基本线程。

2. maximumPoolSize（线程池最大数量）：线程池允许创建的最大线程数。如果队列满了，并且已创建的线程数小于最大线程数，则线程池会再创建新的线程执行任务。值得注意的是，如果使用了无界的任务队列这个参数就没什么效果。

3. keepAliveTime（线程活动保持时间）：线程池的工作线程空闲后，保持存活的时间。所以，如果任务很多，并且每个任务执行的时间比较短，可以调大时间，提高线程的利用率。

4. TimeUnit（线程活动保持时间的单位）：可选的单位有天（DAYS）、小时（HOURS）、分钟（MINUTES）、毫秒（MILLISECONDS）、微秒（MICROSECONDS，千分之一毫秒）和纳秒（NANOSECONDS，千分之一微秒）。

5. workQueue（任务队列）：用于保存等待执行的任务的阻塞队列。

> **可以选择以下几个阻塞队列：**

1. ArrayBlockingQueue：是一个基于数组结构的有界阻塞队列，此队列按 FIFO（先进先出）原则对元素进行排序。

2. LinkedBlockingQueue：一个基于链表结构的阻塞队列，此队列按 FIFO 排序元素，吞吐量通常要高于 ArrayBlockingQueue。静态工厂方法 Executors.newFixedThreadPool() 使用了这个队列。

3. SynchronousQueue：一个不存储元素的阻塞队列。每个插入操作必须等到另一个线程调用移除操作，否则插入操作一直处于阻塞状态，吞吐量通常要高于 LinkedBlockingQueue，静态工厂方法 Executors.newCachedThreadPool 使用了这个队列。

4. PriorityBlockingQueue：一个具有优先级的无限阻塞队列。

5. threadFactory：用于设置创建线程的工厂，可以通过线程工厂给每个创建出来的线程设置更有意义的名字。

6. RejectExecutionHandler（饱和策略）：队列和线程池都满了，说明线程池处于饱和状态，那么必须采取一种策略处理提交的新任务。这个策略默认情况下是 AbortPolicy，表示无法处理新任务时抛出异常。

> **饱和策略：**

在 JDK1.5 中 Java 线程池框架提供了以下 4 种策略：

1. AbortPolicy：直接抛出异常。

2. CallerRunsPolicy：用调用者所在线程来运行任务。

3. DiscardOldestPolicy：抛弃队列中等待最久的任务，然后把当前任务加入队列中尝试再次提交当前任务

4. DiscardPolicy：不处理，丢弃掉。

当然，也可以 根据应用场景需要来实现RejectedExecutionHandler 接口自定义策略。如记录日志或持久化存储不能处理的任务。

**34、如何创建线程池？**

方式一：通过 ThreadPoolExecutor 的构造方法实现：

![img](https://gitee.com/xudongyin/img/raw/master/img/20200930103218)

方式二：通过 Executor 框架的工具类 Executors 来实现：

我们可以创建三种类型的 ThreadPoolExecutor：

1. FixedThreadPool：该方法返回一个固定线程数量的线程池。该线程池中的线程数量始终不变。当有一个新的任务提交时，线程池中若有空闲线程，则立即执行。若没有，则新的任务会被暂存在一个任务队列中，待有线程空闲时，便处理在任务队列中的任务。

2. SingleThreadExecutor：方法返回一个只有一个线程的线程池。若多余一个任务被提交到该线程池，任务会被保存在一个任务队列中，待线程空闲，按先进先出的顺序执行队列中的任务。

3. CachedThreadPool：该方法返回一个可根据实际情况调整线程数量的线程池。线程池的线程数量不确定，但若有空闲线程可以复用，则会优先使用可复用的线程。若所有线程均在工作，又有新的任务提交，则会创建新的线程处理任务。所有线程在当前任务执行完毕后，将返回线程池进行复用。

> **注意：**

《阿里巴巴Java开发手册》中强制线程池不允许使用 Executors 去创建，而是通过 ThreadPoolExecutor 的方式，这样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险。

**Executors 创建线程池对象的弊端如下：**

FixedThreadPool 和 SingleThreadExecutor ：允许请求的队列长度为 Integer.MAX_VALUE，可能堆积大量的请求，从而导致 OOM。CachedThreadPool 和 ScheduledThreadPool ： 允许创建的线程数量为 Integer.MAX_VALUE ，可能会创建大量线程，从而导致 OOM。

**35、线程池中的的线程数一般怎么设置？需要考虑哪些问题？**

主要考虑下面几个方面：

- **1. 线程池中线程执行任务的性质：**

计算密集型的任务比较占 cpu，所以一般线程数设置的大小 等于或者略微大于 cpu 的核数；但 IO 型任务主要时间消耗在 IO 等待上，cpu 压力并不大，所以线程数一般设置较大。

- **2. cpu 使用率：**

当线程数设置较大时，会有如下几个问题：第一，线程的初始化，切换，销毁等操作会消耗不小的 cpu 资源，使得 cpu 利用率一直维持在较高水平。第二，线程数较大时，任务会短时间迅速执行，任务的集中执行也会给 cpu 造成较大的压力。第三， 任务的集中支持，会让 cpu 的使用率呈现锯齿状，即短时间内 cpu 飙高，然后迅速下降至闲置状态，cpu 使用的不合理，应该减小线程数，让任务在队列等待，使得 cpu 的使用率应该持续稳定在一个合理，平均的数值范围。所以 cpu 在够用时，不宜过大，不是越大越好。可以通过上线后，观察机器的 cpu 使用率和 cpu 负载两个参数来判断线程数是否合理。

- **3. 内存使用率：**

线程数过多和队列的大小都会影响此项数据，队列的大小应该通过前期计算线程池任务的条数，来合理的设置队列的大小，不宜过小，让其不会溢出，因为溢出会走拒绝策略，多少会影响性能，也会增加复杂度。

- **4. 下游系统抗并发能力：**

多线程给下游系统造成的并发等于你设置的线程数，例如：如果是多线程访问数据库，你就考虑数据库的连接池大小设置，数据库并发太多影响其 QPS，会把数据库打挂等问题。如果访问的是下游系统的接口，你就得考虑下游系统是否能抗的住这么多并发量，不能把下游系统打挂了。

**36、执行 execute() 方法和 submit() 方法的区别是什么呢？**

1. execute() 方法用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功与否；

2. submit() 方法用于提交需要返回值的任务。线程池会返回一个 Future 类型的对象，通过这个 Future 对象可以判断任务是否执行成功，并且可以通过 Future 的 get() 方法来获取返回值，get() 方法会阻塞当前线程直到任务完成，而使用 get(long timeout，TimeUnit unit) 方法则会阻塞当前线程一段时间后立即返回，这时候有可能任务没有执行完。

**37、说下对 Fork/Join 并行计算框架的理解？**

Fork/Join 并行计算框架主要解决的是分治任务。分治的核心思想是“分而治之”：将一个大的任务拆分成小的子任务的结果聚合起来从而得到最终结果。

Fork/Join 并行计算框架的核心组件是 ForkJoinPool。ForkJoinPool 支持任务窃取机制，能够让所有的线程的工作量基本均衡，不会出现有的线程很忙，而有的线程很闲的情况，所以性能很好。

ForkJoinPool 中的任务队列采用的是双端队列，工作线程正常获取任务和“窃取任务”分别是从任务队列不同的端消费，这样能避免很多不必要的数据竞争。

**38、JDK 中提供了哪些并发容器？**

JDK 提供的这些容器大部分在 java.util.concurrent 包中。

1. ConcurrentHashMap：线程安全的 HashMap；

2. CopyOnWriteArrayList：线程安全的 List，在读多写少的场合性能非常好，远远好于 Vector；

3. ConcurrentLinkedQueue：高效的并发队列，使用链表实现。可以看做一个线程安全的 LinkedList，这是一个非阻塞队列；

4. BlockingQueue：这是一个接口，JDK 内部通过链表、数组等方式实现了这个接口。表示阻塞队列，非常适合用于作为数据共享的通道；

5. ConcurrentSkipListMap：跳表的实现。这是一个 Map，使用跳表的数据结构进行快速查找。

**39、谈谈对 CopyOnWriteArrayList 的理解？**

在很多应用场景中，读操作可能会远远大于写操作。由于读操作根本不会修改原有的数据，因此对于每次读取都进行加锁其实是一种资源浪费。我们应该允许多个线程同时访问 List 的内部数据，毕竟读取操作是安全的。

CopyOnWriteArrayList 类的所有可变操作（add，set 等等）都是通过创建底层数组的新副本来实现的。当 List 需要被修改的时候，我们并不需要修改原有内容，而是对原有数据进行一次复制，将修改的内容写入副本。写完之后，再将修改完的副本替换原来的数据，这样就可以保证写操作不会影响读操作了。

从 CopyOnWriteArrayList 的名字就能看出 CopyOnWriteArrayList 是满足 CopyOnWrite 的 ArrayList，所谓 CopyOnWrite 也就是说：在计算机，如果你想要对一块内存进行修改时，我们不在原有内存块中进行写操作，而是将内存拷贝一份，在新的内存中进行写操作，写完之后，就将指向原来内存指针指向新的内存，原来的内存就可以被回收掉了。

CopyOnWriteArrayList 读取操作没有任何同步控制和锁操作，理由就是内部数组 array 不会发生修改，只会被另外一个 array 替换，因此可以保证数据安全。

CopyOnWriteArrayList 写入操作 add() 方法在添加集合的时候加了锁，保证了同步，避免了多线程写的时候会 copy 出多个副本出来。

**40、谈谈对 BlockingQueue 的理解？分别有哪些实现类？**

阻塞队列（BlockingQueue）被广泛使用在“生产者-消费者”问题中，其原因是 BlockingQueue 提供了可阻塞的插入和移除的方法。当队列容器已满，生产者线程会被阻塞，直到队列未满；当队列容器为空时，消费者线程会被阻塞，直至队列非空时为止。

BlockingQueue 是一个接口，继承自 Queue，所以其实现类也可以作为 Queue 的实现来使用，而 Queue 又继承自 Collection 接口。下面是 BlockingQueue 的相关实现类：

![img](https://gitee.com/xudongyin/img/raw/master/img/20201004145200)

**41、谈谈对 ConcurrentSkipListMap 的理解？**

对于一个单链表，即使链表是有序的，如果我们想要在其中查找某个数据，也只能从头到尾遍历链表，这样效率自然就会很低，跳表就不一样了。跳表是一种可以用来快速查找的数据结构，有点类似于平衡树。它们都可以对元素进行快速的查找。

但一个重要的区别是：对平衡树的插入和删除往往很可能导致平衡树进行一次全局的调整。而对跳表的插入和删除只需要对整个数据结构的局部进行操作即可。这样带来的好处是：在高并发的情况下，你会需要一个全局锁来保证整个平衡树的线程安全。而对于跳表，你只需要部分锁即可。这样，在高并发环境下，你就可以拥有更好的性能。而就查询的性能而言，跳表的时间复杂度也是 O(logn) 。**跳表的本质是同时维护了多个链表，并且链表是分层的。**



## JVM篇

**1、说一下 Jvm 的主要组成部分？及其作用？**

1. 类加载器（ClassLoader）

2. 运行时数据区（Runtime Data Area）

3. 执行引擎（Execution Engine）

4. 本地库接口（Native Interface）

各组件的作用：首先通过类加载器（ClassLoader）会把 Java 代码转换成字节码，运行时数据区（Runtime Data Area）再把字节码加载到内存中，而字节码文件只是 JVM 的一套指令集规范，并不能直接交给底层操作系统去执行，因此需要特定的命令解析器执行引擎（Execution Engine），将字节码翻译成底层系统指令，再交由 CPU 去执行，而这个过程中需要调用其他语言的本地库接口（Native Interface）来实现整个程序的功能。

**2、谈谈对运行时数据区的理解？**

Tip：这道题是非常重要的题目，几乎问到 Java 虚拟机这块都是会被问到的。建议不要简单的只回答几个区域的名称，最好展开的讲解下，下面的答案是比较详细的，根据自己的理解回答其中某一段即可。

![img](https://gitee.com/xudongyin/img/raw/master/img/20201004151437)

- **1. 程序计数器**

程序计数器（Program  Counter  Register）：是一块较小的内存空间，它可以看作是当前线程所执行的字节码的行号指示器。

字节码解释器工作时就是通过改变这个计数器的值来选取下一条需要执行的字节码指令。程序的分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖这个计数器来完成。

由于 Java 虚拟机的多线程是通过线程轮流切换并分配处理器执行时间的方式来实现的，在任何一个确定的时刻，一个处理器都只会执行一条线程中的命令。因此，为了线程切换后能恢复到正确的执行位置，每条线程都需要有一个独立的程序计数器，各线程之间的计数器互不影响，独立存储，我们称这块内存区域为“线程私有”的内存。

**此区域是唯一 一个虚拟机规范中没有规定任何 OutOfMemoryError 情况的区域。**

- **2. Java 虚拟机栈**

Java 虚拟机栈（Java  Virtual  Machine  Stacks）：描述的是Java方法执行的内存模型：每个方法在执行的同时都会创建一个帧栈（Stack  Frame）用于存储局部变量表、操作数栈、动态链接、方法出口等信息。每一个方法从调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中入栈到出栈的过程。它的线程也是私有的，生命周期与线程相同。

局部变量表存放了编译期可知的各种基本数据类型（boolean、byte、char、short、int、float、long、double）、对象引用和 returnAddress 类型（指向了一条字节码指令的地址）。

Java 虚拟机栈的局部变量表的空间单位是槽（Slot），其中 64 位长度的 double 和 long 类型会占用两个 Slot。局部变量表所需内存空间在编译期完成分配，当进入一个方法时，该方法需要在帧中分配多大的局部变量是完全确定的，在方法运行期间不会改变局部变量表的大小。

Java虚拟机栈有两种异常状况：如果线程请求的栈的深度大于虚拟机所允许的深度，将抛出 StackOverflowError 异常；如果扩展时无法申请到足够的内存，就会抛出 OutOfMemoryError 异常。

- **3. 本地方法栈**

本地方法栈（Native  Method  Stack）：与虚拟机栈所发挥的作用是非常相似的，它们之间的区别只不过是虚拟机栈为虚拟机执行Java方法（也就是字节码）服务，而本地方法栈则为虚拟机使用到的 Native 方法服务。

Java 虚拟机规范没有对本地方法栈中方法使用的语言、使用的方式和数据结构做出强制规定，因此具体的虚拟机可以自由地实现它。比如：Sun  HotSpot 虚拟机直接把Java虚拟机栈和本地方法栈合二为一。

与Java虚拟机栈一样，本地方法栈也会抛出StackOverflowError和 OutOfMemoryError 异常。

- **4. Java 堆**

Java堆（Java  Heap）：是被所有线程所共享的一块内存区域，在虚拟机启动时创建。此内存区域的唯一目的就是：存放对象实例，几乎所有的对象实例都在这里分配内存。

Java 堆是垃圾收集器管理的主要区域，因此很多时候也被称做“GC”堆（Garbage  Collected  Heap）。从内存回收的角度看，由于现在收集器基本都采用分代收集算法，所以 Java 堆中还可以细分为：新生代和老年代。从内存分配角度来看，线程共享的 Java 堆中可能划分出多个线程私有的分配缓冲区（Thread  Local  Allocation  Buffer, TLAB）。不过无论如何划分，都与存放的内容无关，无论哪个区域，存储的都仍然是对象实例，进一步划分的目的是为了更好地回收内存，或者更快地分配内存。

Java 虚拟机规定，Java 堆可以处于物理上不连续的内存空间中，只要逻辑上是连续的即可。在实现时，可以是固定大小的，也可以是可扩展的。如果在堆中没有完成实例分配。并且堆也无法扩展时，将会抛出 OutOfMemoryError 异常。

- **5. 方法区**

方法区（Method  Area）：与 Java 堆一样，是各个线程共享的内存区域，它用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。

虽然 Java 虚拟机规范把方法区描述为堆的一个逻辑部分，但是它却有一个别名叫做 Non-Heap（非堆），其目的应该就是与 Java 堆区分开来。

Java 虚拟机规范对方法区的限制非常宽松，除了和 Java 堆一样不需要连续的内存和可以选择固定大小或者可扩展外，还可以选择不实现垃圾收集。这个区域的内存回收目标主要是针对常量池的回收和对类型的卸载。

根据Java虚拟机规范规定，当方法区无法满足内存分配需求时，将抛出OutOfMemoryError异常。

> **运行时常量池**

运行时常量池（Runtime  Constant  Pool）：是方法区的一部分。Class 文件中除了有类的版本、字段、方法、接口等描述信息外，还有一些信息是常量池，用于存放编译期生成的各种字面量和符号引用，这部分内容将在类加载后进入方法区的运行时常量池中存放。

Java 虚拟机对 Class 文件每一部分（自然也包括常量池）的格式都有严格的规定，每一个字节用于存储哪种数据都必须符合规范上的要求才会被虚拟机认可、装载和执行。

> **直接内存**

直接内存（Direct  Memory）：并不是虚拟机运行时数据区的一部分，也不是 Java 虚拟机规范中定义的内存区域。但是这部分内存也频繁地使用，而且也可能导致 OutOfMemoryError 异常。

本地直接内存的分配不会受到 Java 堆大小的限制，但是，既然是内存，肯定还是会受到本机总内存大小以及处理器寻址空间的限制。如果各个内存区域总和大于物理内存限制，从而导致动态扩展时出现 OutOfMemoryError 异常。

**3、堆和栈的区别是什么？**

堆和栈（虚拟机栈）是完全不同的两块内存区域，一个是线程独享的，一个是线程共享的。二者之间最大的区别就是存储的内容不同：堆中主要存放对象实例。栈（局部变量表）中主要存放各种基本数据类型、对象的引用。

从作用来说，栈是运行时的单位，而堆是存储的单位。栈解决程序的运行问题，即程序如何执行，或者说如何处理数据。堆解决的是数据存储的问题，即数据怎么放、放在哪儿。在 Java 中一个线程就会相应有一个线程栈与之对应，因为不同的线程执行逻辑有所不同，因此需要一个独立的线程栈。而堆则是所有线程共享的。栈因为是运行单位，因此里面存储的信息都是跟当前线程（或程序）相关信息的。包括局部变量、程序运行状态、方法返回值等等；而堆只负责存储对象信息。

**4、堆中存什么？栈中存什么？**

堆中存的是对象。栈中存的是基本数据类型和堆中对象的引用。一个对象的大小是不可估计的，或者说是可以动态变化的，但是在栈中，一个对象只对应了一个 4 btye 的引用（堆栈分离的好处）。

> **为什么不把基本类型放堆中呢？**

因为基本数据类型占用的空间一般是1~8个字节，需要空间比较少，而且因为是基本类型，所以不会出现动态增长的情况，长度固定，因此栈中存储就够了。如果把它存在堆中是没有什么意义的。基本类型和对象的引用都是存放在栈中，而且都是几个字节的一个数，因此在程序运行时，它们的处理方式是统一的。但是基本类型、对象引用和对象本身就有所区别了，因为一个是栈中的数据一个是堆中的数据。最常见的一个问题就是，Java 中参数传递时的问题。

**5、 为什么要把堆和栈区分出来呢？栈中不是也可以存储数据吗？**

1. 从软件设计的角度看，栈代表了处理逻辑，而堆代表了数据。这样分开，使得处理逻辑更为清晰。分而治之的思想。这种隔离、模块化的思想在软件设计的方方面面都有体现。

2. 堆与栈的分离，使得堆中的内容可以被多个栈共享（也可以理解为多个线程访问同一个对象）。这种共享的收益是很多的。一方面这种共享提供了一种有效的数据交互方式(如：共享内存)，另一方面，堆中的共享常量和缓存可以被所有栈访问，节省了空间。

3. 栈因为运行时的需要，比如：保存系统运行的上下文，需要进行地址段的划分。由于栈只能向上增长，因此就会限制住栈存储内容的能力。而堆不同，堆中的对象是可以根据需要动态增长的，因此栈和堆的拆分，使得动态增长成为可能，相应栈中只需记录堆中的一个地址即可。

**6、Java 中的参数传递时传值呢？还是传引用？**

**要说明这个问题，先要明确两点：**

1. 不要试图与 C 进行类比，Java 中没有指针的概念。

2. 程序运行永远都是在栈中进行的，因而参数传递时，只存在传递基本类型和对象引用的问题。不会直接传对象本身。

 Java 在方法调用传递参数时，因为没有指针，所以**它都是进行传值调用**。但是传引用的错觉是如何造成的呢？在运行栈中，基本类型和引用的处理是一样的，都是传值。所以，如果是传引用的方法调用，也同时可以理解为“传引用值”的传值调用，即引用的处理跟基本类型是完全一样的。但是当进入被调用方法时，被传递的这个引用的值，被程序解释到堆中的对象，这个时候才对应到真正的对象。如果此时进行修改，修改的是引用对应的对象，而不是引用本身，即：修改的是堆中的数据。所以这个修改是可以保持的了。

对象，从某种意义上说，是由基本类型组成的。可以把一个对象看作为一棵树，对象的属性如果还是对象，则还是一颗树（即非叶子节点），基本类型则为树的叶子节点。**程序参数传递时，被传递的值本身都是不能进行修改的，但是，如果这个值是一个非叶子节点（即一个对象引用），则可以修改这个节点下面的所有内容。**

**7、Java 对象的大小是怎么计算的？**

基本数据的类型的大小是固定的。对于非基本类型的 Java 对象，其大小就值得商榷。在 Java 中，一个空 Object 对象的大小是 8 byte，这个大小只是保存堆中一个没有任何属性的对象的大小。看下面语句：

```java
Object ob = new Object();
```

这样在程序中完成了一个 Java 对象的生命，但是它所占的空间为：4 byte + 8 byte。4 byte 是上面部分所说的 Java 栈中保存引用的所需要的空间。而那 8 byte 则是 Java 堆中对象的信息。因为所有的 Java 非基本类型的对象都需要默认继承 Object 对象，因此不论什么样的 Java 对象，其大小都必须是大于 8 byte。有了 Object 对象的大小，我们就可以计算其他对象的大小了。

```java
Class MaNong {     int count;    boolean flag;    Object obj; }
```

MaNong 的大小为：空对象大小(8 byte) + int 大小(4 byte) + Boolean 大小(1 byte) + 空 Object 引用的大小（4 byte） = 17byte。但是**因为 Java 在对对象内存分配时都是以 8 的整数倍来分**，因此大于 17 byte 的最接近 8 的整数倍的是 24，因此此对象的大小为 24 byte。

这里需要注意一下基本类型的包装类型的大小。因为这种包装类型已经成为对象了，因此需要把它们作为对象来看待。包装类型的大小至少是12 byte（声明一个空 Object 至少需要的空间），而且 12 byte 没有包含任何有效信息，同时，因为 Java 对象大小是 8 的整数倍，因此一个基本类型包装类的大小至少是 16 byte。这个内存占用是很恐怖的，它是使用基本类型的 N 倍（N > 2），有些类型的内存占用更是夸张（随便想下就知道了）。因此，可能的话应尽量少使用包装类。在 JDK5 以后，因为加入了自动类型装换，因此，Java 虚拟机会在存储方面进行相应的优化。

**8、对象的访问定位的两种方式？**

Java 程序通过栈上的引用数据来操作堆上的具体对象。目前主流的对象访问方式有：句柄 和 直接指针。

- **1. 使用句柄**

如果使用句柄的话，那么 Java 堆中将会划分出一块内存来作为句柄池，引用中存储的就是对象的句柄地址，而句柄中包含了对象实例数据与类型数据各自的具体地址信息。

- **2. 直接指针**

如果使用直接指针访问，那么 Java 堆对象的布局中就必须考虑如何防止访问类型数据的相关信息，reference 中存储的直接就是对象的地址。

- **3. 各自的优点**

1. 使用句柄来访问的最大好处是引用中存储的是稳定的句柄地址，在对象被移动时只会改变句柄中的实例数据指针，而引用本身不需要修改；

2. 使用直接指针访问方式最大的好处就是速度快，它节省了一次指针定位的时间开销。

**9、判断垃圾可以回收的方法有哪些？**

垃圾收集器在对堆区和方法区进行回收前，首先要确定这些区域的对象哪些可以被回收，哪些暂时还不能回收，这就要用到判断对象是否存活的算法。

- **1. 引用计数法**

- 基本思想

引用计数是垃圾收集器中的早期策略。在这种方法中，堆中每个对象实例都有一个引用计数。当一个对象被创建时，就将该对象实例分配给一个变量，该变量计数设置为 1。当任何其它变量被赋值为这个对象的引用时，计数加1（a = b，则 b 引用的对象实例的计数器加 1），但当一个对象实例的某个引用超过了生命周期或者被设置为一个新值时，对象实例的引用计数器减 1。任何引用计数器为 0 的对象实例可以被当作垃圾收集。当一个对象实例被垃圾收集时，它引用的任何对象实例的引用计数器减 1。

- 优缺点

优点：引用计数收集器可以很快的执行，交织在程序运行中。对程序需要不被长时间打断的实时环境比较有利。

缺点：无法检测出循环引用。如父对象有一个对子对象的引用，子对象反过来引用父对象。这样，他们的引用计数永远不可能为 0。

**循环引用**

```java
public class Demo{   
    public static void main(String[]   args){       
        MyObject  object1 = new  MyObject();       
        MyObject  object2 = new  MyObject();               
        object1.object = object2;        
        object2.object = object1;        
        object1 = null;       
        object2 = null;   
    }
}
class   MyObject{   
    MyObject    object;
}
```

这段代码是用来验证引用计数算法不能检测出循环引用。最后面两句将 object1 和 object2 赋值为null，也就是说 object1 和 object2 指向的对象已经不可能再被访问，但是由于它们互相引用对方，导致它们的引用计数器都不为 0，那么垃圾收集器就永远不会回收它们。

- **2. 可达性分析算法**

可达性分析算法是从离散数学中的图论引入的，程序把所有的引用关系看作一张图，从一个节点 GC ROOT 开始，寻找对应的引用节点，找到这个节点以后，继续寻找这个节点的引用节点，当所有的引用节点寻找完毕之后，剩余的节点则被认为是没有被引用到的节点，即无用的节点，无用的节点将会被判定为是可回收的对象。

![img](https://gitee.com/xudongyin/img/raw/master/img/20201004211508)

在 Java 语言中，可作为 GC Roots 的对象包括下面几种： 

1. 虚拟机栈中引用的对象（栈帧中的本地变量表）；  
2. 方法区中类静态属性引用的对象；  
3. 方法区中常量引用的对象；  
4. 本地方法栈中 JNI（Native方法）引用的对象。

**10、垃圾回收是从哪里开始的呢？**

查找哪些对象是正在被当前系统使用的。上面分析的堆和栈的区别，其中栈是真正进行程序执行地方，所以要获取哪些对象正在被使用，则需要从 Java 栈开始。同时，一个栈是与一个线程对应的，因此，如果有多个线程的话，则必须对这些线程对应的所有的栈进行检查。

![img](https://gitee.com/xudongyin/img/raw/master/img/20201004225246)

同时，除了栈外，还有系统运行时的寄存器等，也是存储程序运行数据的。这样，以栈或寄存器中的引用为起点，我们可以找到堆中的对象，又从这些对象找到对堆中其他对象的引用，这种引用逐步扩展，最终以 null 引用或者基本类型结束，这样就形成了一颗以 Java 栈中引用所对应的对象为根节点的一颗对象树。如果栈中有多个引用，则最终会形成多颗对象树。在这些对象树上的对象，都是当前系统运行所需要的对象，不能被垃圾回收。而其他剩余对象，则可以视为无法被引用到的对象，可以被当做垃圾进行回收。

**11、被标记为垃圾的对象一定会被回收吗？**

即使在可达性分析算法中不可达的对象，也并非是“非死不可”，这时候它们暂时处于“缓刑”阶段，要**真正宣告一个对象死亡，至少要经历两次标记过程**。  

第一次标记：如果对象在进行可达性分析后发现没有与 GC Roots 相连接的引用链，那它将会被第一次标记；  

第二次标记：第一次标记后接着会进行一次筛选，筛选的条件是此对象是否有必要执行 finalize() 方法。在 finalize() 方法中没有重新与引用链建立关联关系的，将被进行第二次标记。第二次标记成功的对象将真的会被回收，如果对象在 finalize() 方法中重新与引用链建立了关联关系，那么将会逃离本次回收，继续存活。

**12、谈谈对 Java 中引用的了解？**

无论是通过引用计数算法判断对象的引用数量，还是通过可达性分析算法判断对象的引用链是否可达，判定对象是否存活都与“引用”有关。在Java语言中，将引用又分为强引用、软引用、弱引用、虚引用 4 种，这四种引用强度依次逐渐减弱。

- **1. 强引用**  

在程序代码中普遍存在的，类似 Object obj = new Object() 这类引用，只要强引用还存在，垃圾收集器永远不会回收掉被引用的对象。

- **2. 软引用**

用来描述一些还有用但并非必须的对象。对于软引用关联着的对象，如果内存的空间足够，软引用就能继续被使用，而不会被垃圾回收器回收；如果内存不够，在系统将要发生内存溢出异常之前，将会把这些对象列进回收范围之中进行第二次回收。如果这次回收后还没有足够的内存，就把这些对象进行回收。

- **3. 弱引用**

也是用来描述非必需对象的，但是它的强度比软引用更弱一些，被弱引用关联的对象只能生存到下一次垃圾收集发生之前。当垃圾收集器工作时，无论当前内存是否足够，都会回收掉只被弱引用关联的对象。

- **4. 虚引用**

也叫幽灵引用或幻影引用，是最弱的一种引用关系。一个对象是否有虚引用的存在，完全不会对其生存时间构成影响，也无法通过虚引用来取得一个对象实例。它的作用是能在这个对象被收集器回收时收到一个系统通知。  

**13、谈谈对内存泄漏的理解？**

- **内存泄露的基本概念**

在 Java 中，内存泄漏就是存在一些不会再被使用却没有被回收的对象，这些对象有下面两个特点：

1. 这些对象是可达的，即在有向图中，存在通路可以与其相连；

2. 这些对象是无用的，即程序以后不会再使用这些对象。

如果对象满足这两个条件，这些对象就可以判定为 Java 中的内存泄漏，这些对象不会被 GC 所回收，然而它却占用内存。

**14、内存泄露的根本原因是什么？**

长生命周期的对象持有短生命周期对象的引用就很可能发生内存泄漏，尽管短生命周期对象已经不再需要，但是因为长生命周期持有它的引用而导致不能被回收，这就是 Java 中内存泄漏的发生场景。

**15、举几个可能发生内存泄漏的情况？**

1. 静态集合类引起的内存泄漏；

2. 当集合里面的对象属性被修改后，再调用 remove() 方法时不起作用；

3. 监听器：释放对象的时候没有删除监听器；

4. 各种连接：比如数据库连接（dataSourse.getConnection()），网络连接(socket) 和 IO 连接，除非其显式的调用了其 close() 方法将其连接关闭，否则是不会自动被 GC 回收的；

5. 内部类：内部类的引用是比较容易遗忘的一种，而且一旦没释放可能导致一系列的后继类对象没有释放；

6. 单例模式：单例对象在初始化后将在 JVM 的整个生命周期中存在（以静态变量的方式），如果单例对象持有外部的引用，那么这个对象将不能被 JVM 正常回收，导致内存泄漏。

**16、尽量避免内存泄漏的方法？**

1. 尽量不要使用 static 成员变量，减少生命周期；

2. 及时关闭资源；

3. 不用的对象，可以手动设置为 null。

**17、常用的垃圾收集算法有哪些？**

- **1. 标记-清除算法（Mark-Sweep）**

标记-清除算法采用从根集合（GC Roots）进行扫描，对存活的对象进行标记，标记完毕后，再扫描整个空间中未被标记的对象，进行回收。标记-清除算法不需要进行对象的移动，只需对不存活的对象进行处理，在存活对象比较多的情况下极为高效，但由于标记-清除算法直接回收不存活的对象，因此会造成内存碎片。

- **2. 复制算法(Copying)**  

复制算法的提出是为了克服句柄的开销和解决内存碎片的问题。它开始时把堆分成 一个对象面和多个空闲面， 程序从对象面为对象分配空间，当对象满了，基于 copying 算法的垃圾收集就从根集合（GC Roots）中扫描活动对象，并将每个活动对象复制到空闲面（使得活动对象所占的内存之间没有空闲洞），这样空闲面变成了对象面，原来的对象面变成了空闲面，程序会在新的对象面中分配内存。

- **3. 标记-整理算法(Mark-compact)**  

标记-整理算法采用标记-清除算法一样的方式进行对象的标记，但在清除时不同，在回收不存活的对象占用的空间后，会将所有的存活对象往左端空闲空间移动，并更新对应的指针。标记-整理算法是在标记-清除算法的基础上，又进行了对象的移动，因此成本更高，但是却解决了内存碎片的问题。

- **4. 分代收集算法**

分代收集算法是目前大部分 JVM 的垃圾收集器采用的算法。它的核心思想是根据对象存活的生命周期将内存划分为若干个不同的区域。一般情况下将堆区划分为老年代（Tenured Generation）和新生代（Young Generation），在堆区之外还有一个代就是永久代（Permanet Generation）。

老年代的特点是每次垃圾收集时只有少量对象需要被回收，而新生代的特点是每次垃圾回收时都有大量的对象需要被回收，那么就可以根据不同代的特点采取最适合的收集算法。

**18、为什么要采用分代收集算法？**

分代的垃圾回收策略，是基于这样一个事实：不同的对象的生命周期是不一样的。因此，不同生命周期的对象可以采取不同的收集方式，以便提高回收效率。

在 Java 程序运行的过程中，会产生大量的对象，其中有些对象是与业务信息相关，比如 Http 请求中的 Session 对象、线程、Socket 连接，这类对象跟业务直接挂钩，因此生命周期比较长。但是还有一些对象，主要是程序运行过程中生成的临时变量，这些对象生命周期会比较短，比如：String 对象，由于其不变类的特性，系统会产生大量的这些对象，有些对象甚至只用一次即可回收。

在不进行对象存活时间区分的情况下，每次垃圾回收都是对整个堆空间进行回收，花费时间相对会长，同时，因为每次回收都需要遍历所有存活对象，但实际上，对于生命周期长的对象而言，这种遍历是没有效果的，因为可能进行了很多次遍历，但是他们依旧存在。因此，分代垃圾回收采用分治的思想，进行代的划分，把不同生命周期的对象放在不同代上，不同代上采用最适合它的垃圾回收方式进行回收。

**19、分代收集下的年轻代和老年代应该采用什么样的垃圾回收算法？**

- **1. 年轻代（Young Generation）的回收算法 (主要以 Copying 为主)** 

1. 所有新生成的对象首先都是放在年轻代的。年轻代的目标就是尽可能快速的收集掉那些生命周期短的对象。

2. 新生代内存按照 8:1:1 的比例分为一个 eden 区和两个 survivor（survivor0、 survivor1）区。大部分对象在 Eden 区中生成。回收时先将 Eden 区存活对象复制到一个 survivor0 区，然后清空 eden 区，当这个 survivor0 区也存放满了时，则将 eden 区和 survivor0 区存活对象复制到另一个 survivor1 区，然后清空 eden 区 和这个 survivor0 区，此时 survivor0 区是空的，然后将survivor0 区和 survivor1 区交换，即保持 survivor1 区为空， 如此往复。

3. 当 survivor1 区不足以存放 Eden 区 和 survivor0区 的存活对象时，就将存活对象直接存放到老年代。若是老年代也满了就会触发一次Full GC（Major GC），也就是新生代、老年代都进行回收。

4. 新生代发生的 GC 也叫做 Minor GC，MinorGC 发生频率比较高（不一定等 Eden 区满了才触发）。

- **2. 年老代（Old Generation）的回收算法（主要以 Mark-Compact 为主）**

1. 在年轻代中经历了 N 次垃圾回收后仍然存活的对象，就会被放到年老代中。因此，可以认为年老代中存放的都是一些生命周期较长的对象。

2. 内存比新生代也大很多（大概比例是1 : 2），当老年代内存满时触发 Major GC 即 Full GC，Full GC 发生频率比较低，老年代对象存活时间比较长，存活率标记高。

**20、什么是浮动垃圾？**

由于在应用运行的同时进行垃圾回收，所以有些垃圾可能在垃圾回收进行完成时产生，这样就造成了“Floating Garbage”，这些垃圾需要在下次垃圾回收周期时才能回收掉。所以，并发收集器一般需要20%的预留空间用于这些浮动垃圾。

**21、什么是内存碎片？如何解决？**

由于不同 Java 对象存活时间是不一定的，因此，在程序运行一段时间以后，如果不进行内存整理，就会出现零散的内存碎片。碎片最直接的问题就是会导致无法分配大块的内存空间，以及程序运行效率降低。所以，在上面提到的基本垃圾回收算法中，“复制”方式和“标记-整理”方式，都可以解决碎片的问题。

**22、常用的垃圾收集器有哪些？**

![img](https://gitee.com/xudongyin/img/raw/master/img/20201005131703)

- **1. Serial 收集器（复制算法)**

新生代单线程收集器，标记和清理都是单线程，优点是简单高效。是 client 级别默认的 GC 方式，可以通过 -XX:+UseSerialGC 来强制指定。

- **2. Serial Old 收集器（标记-整理算法）**

老年代单线程收集器，Serial 收集器的老年代版本。

- **3. ParNew 收集器（停止-复制算法）**

新生代收集器，可以认为是 Serial 收集器的多线程版本，在多核 CPU 环境下有着比 Serial 更好的表现。

- **4. Parallel Scavenge 收集器（停止-复制算法）**

并行收集器，追求高吞吐量，高效利用 CPU。吞吐量一般为 99%， 吞吐量= 用户线程时间 / (用户线程时间+GC线程时间)。适合后台应用等对交互相应要求不高的场景。是 server 级别默认采用的GC方式，可用 -XX:+UseParallelGC 来强制指定，用 -XX:ParallelGCThreads=4 来指定线程数。

- **5. Parallel Old 收集器（停止-复制算法）**

Parallel Old 收集器的老年代版本，并行收集器，吞吐量优先。

- **6. CMS(Concurrent Mark Sweep)收集器（标记-清除算法）**

高并发、低停顿，追求最短 GC 回收停顿时间，cpu 占用比较高，响应时间快，停顿时间短，多核 cpu 追求高响应时间的选择。

CMS 是英文 Concurrent Mark-Sweep 的简称，是以牺牲吞吐量为代价来获得最短回收停顿时间的垃圾回收器。对于要求服务器响应速度的应用上，这种垃圾回收器非常适合。在启动 JVM 的参数加上“-XX:+UseConcMarkSweepGC”来指定使用 CMS 垃圾回收器。

CMS 使用的是标记-清除的算法实现的，所以在 GC 的时候会产生大量的内存碎片，当剩余内存不能满足程序运行要求时，系统将会出现 Concurrent Mode Failure，临时 CMS 会采用 Serial Old 回收器进行垃圾清除，此时的性能将会被降低。

- **7. G1** 

G1 收集器在后台维护了一个优先列表，每次根据允许的收集时间，优先选择回收价值最大的 Region(这也就是它的名字 Garbage-First的由来。

**23、谈谈你对 CMS 垃圾收集器的理解？**

CMS 是英文 Concurrent Mark-Sweep 的简称，是以牺牲吞吐量为代价来获得最短回收停顿时间的垃圾回收器。是使用标记清除算法实现的，整个过程分为四步：

1. 初始标记：记录下直接与 root 相连的对象，暂停所有的其他线程，速度很快；

2. 并发标记：同时开启 GC 和用户线程，用一个闭包结构去记录可达对象。但在这个阶段结束，这个闭包结构并不能保证包含当前所有的可达对象。因为用户线程可能会不断的更新引用域，所以 GC 线程无法保证可达性分析的实时性。所以这个算法里会跟踪记录这些发生引用更新的地方。

3. 重新标记：重新标记阶段就是为了修正并发标记期间因为用户程序继续运行而导致标记产生变动的那一部分对象的标记记录。【这个阶段的停顿时间一般会比初始标记阶段的时间稍长，远远比并发标记阶段时间短】；

4. 并发清除：开启用户线程，同时 GC 线程开始对未标记的区域做清扫。

- **CMS 的优缺点：**

主要优点：并发收集、低停顿；

主要缺点：对 CPU 资源敏感、无法处理浮动垃圾、它使用的回收算法“标记-清除”算法会导致收集结束时会有大量空间碎片产生。 

**24、谈谈你对 G1 收集器的理解？**

**垃圾回收的瓶颈**

传统分代垃圾回收方式，已经在一定程度上把垃圾回收给应用带来的负担降到了最小，把应用的吞吐量推到了一个极限。但是他无法解决的一个问题，就是 Full GC 所带来的应用暂停。在一些对实时性要求很高的应用场景下，GC 暂停所带来的请求堆积和请求失败是无法接受的。这类应用可能要求请求的返回时间在几百甚至几十毫秒以内，如果分代垃圾回收方式要达到这个指标，只能把最大堆的设置限制在一个相对较小范围内，但是这样有限制了应用本身的处理能力，同样也是不可接受的。

分代垃圾回收方式确实也考虑了实时性要求而提供了并发回收器，支持最大暂停时间的设置，但是受限于分代垃圾回收的内存划分模型，其效果也不是很理想。

G1 可谓博采众家之长，力求到达一种完美。它吸取了增量收集优点，把整个堆划分为一个一个等大小的区域（region）。内存的回收和划分都以region为单位；同时，它也吸取了 CMS 的特点，把这个垃圾回收过程分为几个阶段，分散一个垃圾回收过程；而且，G1 也认同分代垃圾回收的思想，认为不同对象的生命周期不同，可以采取不同收集方式，因此，它也支持分代的垃圾回收。为了达到对回收时间的可预计性，G1 在扫描了 region 以后，对其中的活跃对象的大小进行排序，首先会收集那些活跃对象小的 region，以便快速回收空间（要复制的活跃对象少了），因为活跃对象小，里面可以认为多数都是垃圾，所以这种方式被称为 Garbage First（G1）的垃圾回收算法，即：垃圾优先的回收。

**25、说下你对垃圾回收策略的理解/垃圾回收时机？**

- **1. Minor / Scavenge GC**  

所有对象创建在新生代的 Eden 区，当 Eden 区满后触发新生代的 Minor GC，将 Eden 区和非空闲 Survivor 区存活的对象复制到另外一个空闲的 Survivor 区中。保证一个 Survivor 区是空的，新生代 Minor GC 就是在两个 Survivor 区之间相互复制存活对象，直到 Survivor 区满为止。

Minor/Scavenge 这种方式的 GC 是在年轻代的 Eden 区进行，不会影响到年老代。因为大部分对象都是从 Eden 区开始的，同时 Eden 区不会分配的很大，所以 Eden 区的 GC 会频繁进行。因而，一般在这里需要使用速度快、效率高的算法，使 Eden 区能尽快空闲出来。

- **2. Full GC**  

对整个堆进行整理，包括 Young、Tenured 和 Perm。Full GC 因为需要对整个堆进行回收，所以比 Minor GC 要慢，因此应该尽可能减少 Full GC 的次数。在对 JVM 调优的过程中，很大一部分工作就是对于 Full GC 的调节。

> **Minor 有如下原因可能导致 Full GC：**

1. 调用 System.gc()，会建议虚拟机执行 Full GC。只是建议虚拟机执行 Full GC，但是虚拟机不一定真正去执行。

2. 老年代空间不足，原因：老年代空间不足的常见场景为大对象直接进入老年代、长期存活的对象进入老年代等。为了避免以上原因引起的 Full GC，应当尽量不要创建过大的对象以及数组。除此之外，可以通过 -Xmn 虚拟机参数调大新生代的大小，让对象尽量在新生代被回收掉，不进入老年代。还可以通过 -XX:MaxTenuringThreshold 调大对象进入老年代的年龄，让对象在新生代多存活一段时间；

3. 空间分配担保失败：使用复制算法的 Minor GC 需要老年代的内存空间作担保，如果担保失败会执行一次 Full GC；

4. JDK 1.7 及以前的永久代空间不足。在 JDK1.7 及以前，HotSpot 虚拟机中的方法区是用永久代实现的，永久代中存放的为一些 Class 的信息、常量、静态变量等数据。当系统中要加载的类、反射的类和调用的方法较多时，永久代可能会被占满，在未配置为采用 CMS GC 的情况下也会执行 Full GC。如果经过 Full GC 仍然回收不了，那么虚拟机会抛出 java.lang.OutOfMemoryError。为避免以上原因引起的 Full GC，可采用的方法为增大永久代空间或转为使用 CMS GC。

5. Concurrent Mode Failure 执行 CMS GC 的过程中，同时有对象要放入老年代，而此时老年代空间不足（可能是 GC 过程中浮动垃圾过多导致暂时性的空间不足），便会报 Concurrent Mode Failure 错误，并触发 Full GC。

**26、谈谈你对内存分配的理解？大对象怎么分配？空间分配担保？**

1. 对象优先在 Eden 区分配：大多数情况下，对象在新生代 Eden 区分配，当 Eden 区空间不够时，发起 Minor GC。

2. 大对象直接进入老年代：大对象是指需要连续内存空间的对象，最典型的大对象是那种很长的字符串以及数组。经常出现大对象会提前触发垃圾收集以获取足够的连续空间分配给大对象。-XX:PretenureSizeThreshold，大于此值的对象直接在老年代分配，避免在 Eden 区和 Survivor 区之间的大量内存复制。

3. 长期存活的对象将进入老年代：为对象定义年龄计数器，对象在 Eden 出生并经过 Minor GC 依然存活，将移动到 Survivor 中，年龄就增加 1 岁，增加到一定年龄则移动到老年代中。-XX:MaxTenuringThreshold 用来定义年龄的阈值。

4. 动态对象年龄判定：为了更好的适应不同程序的内存情况，虚拟机不是永远要求对象年龄必须达到了某个值才能进入老年代，如果 Survivor 空间中相同年龄所有对象大小的总和大于 Survivor 空间的一半，年龄大于或等于该年龄的对象就可以直接进入老年代，无需达到要求的年龄。

5. 空间分配担保

- （1）在发生 Minor GC 之前，虚拟机先检查老年代最大可用的连续空间是否大于新生代所有对象总空间，如果条件成立的话，那么 Minor GC 可以确认是安全的；
- （2）如果不成立的话，虚拟机会查看 HandlePromotionFailure 设置值是否允许担保失败，如果允许那么就会继续检查老年代最大可用的连续空间是否大于历次晋升到老年代对象的平均大小，如果大于，将尝试着进行一次 Minor GC；如果小于，或者 HandlePromotionFailure 设置不允许冒险，那么就要进行一次 Full GC。

**27、说下你用过的 JVM 监控工具？**

1. jvisualvm：虚拟机监视和故障处理平台

2. jps ：查看当前 Java 进程

3. jstat：显示虚拟机运行数据

4. jmap：内存监控

5. jhat：分析 heapdump 文件

6. jstack：线程快照

7. jinfo：虚拟机配置信息

**28、如何利用监控工具调优**？

- **1. 堆信息查看**

1. 可查看堆空间大小分配（年轻代、年老代、持久代分配）

2. 提供即时的垃圾回收功能

3. 垃圾监控（长时间监控回收情况）

4. 查看堆内类、对象信息查看：数量、类型等

5. 对象引用情况查看

> 有了堆信息查看方面的功能，我们一般可以顺利解决以下问题：

1. 年老代年轻代大小划分是否合理

2. 内存泄漏

3. 垃圾回收算法设置是否合理



- **2. 线程监控**

线程信息监控：系统线程数量

线程状态监控：各个线程都处在什么样的状态下

Dump 线程详细信息：查看线程内部运行情况 

死锁检查

- **3. 热点分析**

1. CPU 热点：检查系统哪些方法占用的大量 CPU 时间；

2. 内存热点：检查哪些对象在系统中数量最大（一定时间内存活对象和销毁对象一起统计）这两个东西对于系统优化很有帮助。我们可以根据找到的热点，有针对性的进行系统的瓶颈查找和进行系统优化，而不是漫无目的的进行所有代码的优化。

- **4. 快照**

快照是系统运行到某一时刻的一个-*.*

+

+定格。在我们进行调优的时候，不可能用眼睛去跟踪所有系统变化，依赖快照功能，我们就可以进行系统两个不同运行时刻，对象（或类、线程等）的不同，以便快速找到问题。

举例说，我要检查系统进行垃圾回收以后，是否还有该收回的对象被遗漏下来的了。那么，我可以在进行垃圾回收前后，分别进行一次堆情况的快照，然后对比两次快照的对象情况。

- **5. 内存泄露检查**

内存泄漏是比较常见的问题，而且解决方法也比较通用，这里可以重点说一下，而线程、热点方面的问题则是具体问题具体分析了。

内存泄漏一般可以理解为系统资源（各方面的资源，堆、栈、线程等）在错误使用的情况下，导致使用完毕的资源无法回收（或没有回收），从而导致新的资源分配请求无法完成，引起系统错误。内存泄漏对系统危害比较大，因为它可以直接导致系统的崩溃。

**29、JVM 的一些参数？**

- **1. 堆设置**

-Xms：初始堆大小

-Xmx：最大堆大小

-XX:NewSize=n：设置年轻代大小

-XX:NewRatio=n：设置年轻代和年老代的比值。如:为3，表示年轻代与年老代比值为 1：3，年轻代占整个年轻代年老代和的 1/4

-XX:SurvivorRatio=n：年轻代中 Eden 区与两个 Survivor 区的比值。注意 Survivor 区有两个。如：3，表示 Eden：Survivor=3：2，一个Survivor区占整个年轻代的 1/5

-XX:MaxPermSize=n：设置持久代大小

- **2. 收集器设置**

-XX:+UseSerialGC：设置串行收集器

-XX:+UseParallelGC：设置并行收集器

-XX:+UseParalledlOldGC：设置并行年老代收集器

-XX:+UseConcMarkSweepGC：设置并发收集器

- **3. 垃圾回收统计信息**

-XX:+PrintGC：开启打印 gc 信息

-XX:+PrintGCDetails：打印 gc 详细信息

-XX:+PrintGCTimeStamps

-Xloggc:filename

- **4. 并行收集器设置**

-XX:ParallelGCThreads=n：设置并行收集器收集时使用的 CPU 数

-XX:MaxGCPauseMillis=n：设置并行收集最大暂停时间

-XX:GCTimeRatio=n：设置垃圾回收时间占程序运行时间的百分比

- **5. 并发收集器设置**

-XX:+CMSIncrementalMode：设置为增量模式。适用于单 CPU 情况

-XX:ParallelGCThreads=n：设置并发收集器年轻代收集方式为并行收集时，使用的 CPU 数。并行收集线程数

**30、谈谈你对类文件结构的理解？有哪些部分组成？**

Class 文件结构如下标所示：

![img](https://gitee.com/xudongyin/img/raw/master/img/20201005192212)

Class 文件没有任何分隔符，严格按照上面结构表中的顺序排列。无论是顺序还是数量，甚至于数据存储的字节序这样的细节，都是被严格限定的，哪个字节代表什么含义，长度是多少，先后顺序如何，都不允许改变。

1. 魔数（magic）：每个 Class 文件的头 4 个字节称为魔数（Magic  Number），它的唯一作用是确定这个文件是否为一个能被虚拟机接受的Class 文件，即判断这个文件是否符合 Class 文件规范。

2. 文件的版本：minor_version 和 major_version。

3. 常量池：constant_pool_count 和 constant_pool：常量池中主要存放两大类常量：字面量（Literal）和符号引用（Symbolic  References）。

4. 访问标志：access_flags：用于识别一些类或者接口层次的访问信息。包括：这个 Class 是类还是接口、是否定义了 Public 类型、是否定义为 abstract 类型、如果是类，是否被声明为了 final 等等。

5. 类索引、父类索引与接口索引集合：this_class、super_class和interfaces。

6. 字段表集合：field_info、fields_count：字段表（field_info）用于描述接口或者类中声明的变量；fields_count 字段数目：表示Class文件的类和实例变量总数。

7. 方法表集合：methods、methods_count

8. 属性表集合：attributes、attributes_count

**31、谈谈你对类加载机制的了解？
**虚拟机把描述类的数据从 Class 文件加载到内存，并对数据进行校验、转换解析和初始化，最终形成可以被虚拟机直接使用的 Java 类型，这就是虚拟机的类加载机制。

类从被加载到虚拟机内存中开始，到卸载出内存为止，它的整个生命周期包括：加载、验证、准备、解析、初始化、使用、卸载 7 个阶段。其中验证、准备、解析 3 个部分统称为连接，这7个阶段发生的顺序如下图所示：

![img](https://gitee.com/xudongyin/img/raw/master/img/20201006091652)

**32、类加载各阶段的作用分别是什么？**

- **1. 加载**

在加载阶段，虚拟机需要完成以下三件事情：

1. 通过一个类的全限定名来获取定义此类的二进制字节流；

2. 将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构；

3. 在内存中生成一个代表这个类的 java.lang.Class 对象，作为方法区这个类的各种数据的访问接口。

- **2. 验证**

主要是为了确保 Class 文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身的安全。验证阶段大致上分为 4 个阶段的检验动作：文件格式验证、元数据验证、字节码验证、符号引用验证。

1. 文件格式校验：验证字节流是否符合 class 文件的规范，并且能被当前版本的虚拟机处理。只有通过这个阶段的验证后，字节流才会进入内存的方法区进行存储，所以后面的3个阶段的全部是基于方法区的存储结构进行的，不会再直接操作字节流；

2. 元数据验证：对字节码描述的信息进行语义分析，以保证其描述的信息符合 Java 语言规范的要求。目的是保证不存在不符合 Java 语言规范的元数据信息；

3. 字节码验证：该阶段主要工作是进行数据流和控制流分析，保证被校验类的方法在运行时不会做出危害虚拟机安全的行为；

4. 符号引用验证：最后一个阶段的校验发生在虚拟机将符号引用转化为直接引用的时候，这个转化动作将在连接的第三个阶段——解析阶段中发生。符号引用验证的目的是确保解析动作能正常执行。

- **3. 准备**

准备阶段是正式为类变量分配内存并设置类变量初始值的阶段，这些变量所使用的内存都将在方法区中进行分配。这时候进行内存分配的仅包括类变量（被 static 修饰的变量），而不包括实例变量，实例变量将会在对象实例化时随着对象一起分配在 Java 堆中。实例化不是类加载的一个过程，类加载发生在所有实例化操作之前，并且类加载只进行一次，实例化可以进行多次。

初始值是默认值 0 或 false 或 null。如果类变量是常量（final），那么会按照表达式来进行初始化，而不是赋值为 0。public static final int value = 123;

- **4. 解析**

解析阶段是虚拟机将常量池内的符号引用替换为直接引用的过程。

- **5. 初始化**

在准备阶段，变量已经赋过一次系统要求的初始值了，而在初始化阶段，则根据程序员通过程序制定的主观计划去初始化类变量和其他资源，或者可以从另外一个角度来表达：初始化阶段是执行类构造器 <clinit>() 方法的过程。

**33、有哪些类加载器？分别有什么作用？**

1. 启动类加载器(Bootstrap  ClassLoader)：这个类加载器是由 C++ 语言实现的，是虚拟机自身的一部分。负责将存在 <JAVA_HOME>\lib 目录中的，或者被 -Xbootclasspath 参数所指定的路径中的类库加载到虚拟机内存中。启动内加载器无法被 Java 程序直接引用，用户在编写自定义类加载器时，如果需要把加载请求委派给启动类加载器，直接使用 null 即可；

2. 其他类加载器：由 Java 语言实现，独立于虚拟机外部，并且全都继承自抽象类 java.lang.ClassLoader。如扩展类加载器和应用程序类加载器：

- （1）扩展类加载器(Extension  ClassLoader)：这个类加载器由sun.misc.Launcher$ExtClassLoader 实现，它负责加载<JAVA_HOME>\lib\ext目录中的，或者被 java.ext.dirs 系统变量所指定的路径中的所有类库，开发者可以直接使用扩展类加载器。
- （2）应用程序类加载器 (Application  ClassLoader)：这个类加载器由 sun.misc.Launcher$AppClassLoder 实现。由于个类加载器是 ClassLoader 中的 getSystemClassLoader() 方法的返回值，所以一般也称之为系统类加载器。它负责加载用户路径（ClassPath）所指定的类库，开发者可以直接使用这个类加载器，如果应用程序中没有自定义过自己的类加载器，一般情况下这个就是程序中默认的类加载器。

**34、类与类加载器的关系?**

类加载器虽然只用于实现类的加载动作，但它在 Java 程序中起到的作用却远远不限于类加载阶段。对于任意一个类，都需要由加载它的类加载器和这个类本身一同确立其在 Java 虚拟机中的唯一性，每个类加载器，都拥有一个独立的类名称空间。换句话说：比较两个类是否“相等”，只有在这两个类是由同一个类加载器加载的前提下才有意义，否则，即使这两个类来源于同一个 Class 文件，被同一个虚拟机加载，只要加载它们的类加载器不同，那么这两个类就必定不相等。

**35、谈谈你对双亲委派模型的理解？工作过程？为什么要使用？**

应用程序一般是由上诉的三种类加载器相互配合进行加载的，如果有必要，还可以加入自己定义的类加载器，它们的关系如下图所示：

![img](https://gitee.com/xudongyin/img/raw/master/img/20201006111257)

- **双亲委派模型的工作过程：**

如果一个类加载器收到了类加载请求，它首先不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成。每一个层次的类加载器都是如此，因此所有的加载请求最终都应该传送到顶层的启动类加载器中，只有当父类加载器反馈自己无法完成这个加载请求（它的搜索范围中没有找到所需的类）时，子加载器才会尝试自己去加载。

- **使用双亲委派模型的好处：**

Java 类随着它的类加载器一起具备了一种带有优先级的层次关系。例如：类 java.lang.Object，它存放在 rt.jar 中，无论哪一个类加载器需要加载这个类，最终都是委派给处于模型最顶端的启动类加载器进行加载，因此 Object 类在程序的各种类加载器环境中都是同一个类（使用的是同一个类加载器加载的）。相反，如果没有使用双亲委派模型，由各个类加载器自行去加载的话，如果用户自己编写了一个 java.lang.Object 类，并放在程序的 ClassPath 中，那么系统将会出现多个不同的 Object 类，Java 类型体系中最基础的行为也就无法保证，应用程序也将变得一片混乱。

- **双亲委派模型的主要代码实现：**

实现双亲委派的代码都集中在 java.lang.ClassLoader 的 loadClass() 方法中，逻辑清晰易懂：先检查是否已经被加载过，若没有加载则调用父加载器的 loadClass() 方法，若父加载器为空则默认使用启动类加载器作为父类加载器。如果父类加载失败，抛出 ClassNotFoundException 异常后，再调用自己的 findClass() 方法进行加载。

**36、怎么实现一个自定义的类加载器？需要注意什么？**

若要实现自定义类加载器，只需要继承 java.lang.ClassLoader 类，并且重写其 findClass() 方法即可。

**37、怎么打破双亲委派模型？**

1. 自己写一个类加载器；

2. 重写 loadClass() 方法

3. 重写 findClass() 方法

这里最主要的是重写 loadClass 方法，因为双亲委派机制的实现都是通过这个方法实现的，先找父加载器进行加载，如果父加载器无法加载再由自己来进行加载，源码里会直接找到根加载器，重写了这个方法以后就能自己定义加载的方式了。

**38、有哪些实际场景是需要打破双亲委派模型的？**

 JNDI 服务，它的代码由启动类加载器去加载，但 JNDI 的目的就是对资源进行集中管理和查找，它需要调用独立厂商实现部署在应用程序的 classpath 下的 JNDI 接口提供者(SPI, Service Provider Interface) 的代码，但启动类加载器不可能“认识“这些代码，该怎么办？ 

为了解决这个困境，Java 设计团队只好引入了一个不太优雅的设计：线程上下文件类加载器(Thread Context ClassLoader)。这个类加载器可以通过 java.lang.Thread 类的 setContextClassLoader() 方法进行设置，如果创建线程时还未设置，它将会从父线程中继承一个；如果在应用程序的全局范围内都没有设置过，那么这个类加载器默认就是应用程序类加载器。有了线程上下文类加载器，JNDI 服务使用这个线程上下文类加载器去加载所需要的 SPI 代码，也就是父类加载器请求子类加载器去完成类加载动作，这种行为实际上就是打通了双亲委派模型的层次结构来逆向使用类加载器，已经违背了双亲委派模型，但这也是无可奈何的事情。Java 中所有涉及 SPI 的加载动作基本上都采用这种方式，例如 JNDI、JDBC、JCE、JAXB 和 JBI 等。 

**39、谈谈你对编译期优化和运行期优化的理解？**

- **编译期优化：**

1. 解析与填充符号表的过程

2. 插入式注解处理器的注解处理过程

3. 分析与字节码生成过程

- **编译优化：**

1. 方法内联

2. 公共子表达式消除

3. 数组范围检查消除

4. 逃逸分析

**40、为何 HotSpot 虚拟机要使用解释器与编译器并存的架构？**

解释器：程序可以迅速启动和执行，消耗内存小 （类似人工，成本低，到后期效率低）；

编译器：随着代码频繁执行会将代码编译成本地机器码 （类似机器，成本高，到后期效率高）。

在整个虚拟机执行架构中，解释器与编译器经常配合工作，两者各有优势：当程序需要迅速启动和执行的时候，解释器可以首先发挥作用，省去编译的时间，立即执行。在程序运行后，随着时间的推移，编译器逐渐发挥作用，把越来越多的代码编译成本地代码之后，可以获取更高的执行效率。当程序运行环境中内存资源限制较大（如部分嵌入式系统），可以使用解释执行节约内存，反之可以使用编译执行来提升效率。

解释执行可以节约内存，而编译执行可以提升效率。因此，在整个虚拟机执行架构中，解释器与编译器经常配合工作。

**41、说下你对 Java 内存模型的理解？**

处理器和内存不是同数量级，所以需要在中间建立中间层，也就是高速缓存，这会引出缓存一致性问题。在多处理器系统中，每个处理器都有自己的高速缓存，而它们又共享同一主内存（Main Memory），有可能操作同一位置引起各自缓存不一致，这时候需要约定协议在保证一致性。

 Java 内存模型(Java  Memory  Model，JMM)：屏蔽掉了各种硬件和操作系统的内存访问差异，以实现让 Java 程序在各种平台下都能达到一致性的内存访问效果。

![img](https://gitee.com/xudongyin/img/raw/master/img/20201006111333)

- **主内存与工作内存**

Java 内存模型的主要目标是定义程序中各个变量的访问规则，即在虚拟机中将变量存储到内存和从内存中取出变量这样的底层细节。

Java 内存模型规定了所有的变量都存储在主内存（Main Memory）中，每个线程有自己的工作线程（Working Memory），保存主内存副本拷贝和自己私有变量，不同线程不能访问工作内存中的变量。线程间变量值的传递需要通过主内存来完成。

**42、内存间的交互操作有哪些？需要满足什么规则？**

关于主内存与工作内存之间的具体的交互协议，即：一个变量如何从主内存拷贝到工作内存、如何从工作内存同步主内存之类的实现细节，Java内存模型中定义一下八种操作来完成：

1. lock(锁定)：作用于主内存的变量。它把一个变量标志为一个线程独占的状态；

2. unlock(解锁)：作用于主内存的变量，它把处于锁定状态的变量释放出来，释放后的变量才可以被其他线程锁定；

3. read(读取)：作用于主内存的变量，它把一个变量的值从主内存传输到线程的工作内存中，以便随后的load动作使用；

4. load(载入)：作用于工作内存的变量，它把read操作从主内存中得到变量值放入工作内存的变量的副本中；

5. use(使用)：作用于工作内存的变量， 它把工作内存中一个变量的值传递给执行引擎，每当虚拟机遇到一个需要使用到变量的值的字节码指令时将会执行这个操作；

6. assign(赋值)：作用于工作内存的变量。它把一个从执行引擎接收到的值赋值给工作内存的变量，每当虚拟机遇到需要给一个变量赋值的字节码时执行这个操作；

7. store(存储)：作用于工作内存的变量。它把一个工作内存中一个变量的值传递到主内存中，以便随后的write操作使用；

8. write(写入)：作用于主内存的变量。它把store操作从工作内存中得到的变量的值放入主内存的变量中。

如果要把一个变量从工作内存复制到工作内存，那就要按顺序执行 read 和 load 操作，如果要把变量从工作内存同步回主内存，就要按顺序执行 store 和 write 操作。

- **上诉 8 种基本操作必须满足的规则：**

1. 不允许 read 和 load、store 和 write 操作之一单独出现；
2. 不允许一个线程丢弃它的最近的 assign 操作，即变量在工作内存中改变之后必须把该变化同步回主内存；
3. 不允许一个线程无原因地（没有发生过任何 assign 操作）把数据从线程的工作内存同步回主内存中；
4. 一个新的变量只能在主内存中“诞生”，不允许在工作内存中直接使用一个未被初始化（load 或 assign）的变量，换句话说就是对一个变量实施 use 和 store 操作之前，必须执行过了load  和assign 操作；
5. 一个变量在同一时刻只允许一条线程对其进行 lock 操作，但 lock 操作可以被同一线程重复执行多次，多次执行 lock 后，只有执行相同次数的 unlock，变量才会被解锁；
6. 如果对一个变量执行 lock 操作，将会清空工作内存中此变量的值，在执行引擎使用这个变量前，需要重新执行 load 或 assign 操作初始化变量的值；
7. 如果一个变量事先没有被 lock 操作锁定，则不允许对它执行 unlock 操作，也不允许去 unlock 一个被其他线程锁定的变量；
8. 对一个变量执行 unlock 操作之前，必须先把此变量同步回主内存中（执行 store 和 write 操作）。



## 设计模式篇

1、说下你知道的设计模式有哪些？

下面 3 种类型中各挑几个常见的或者你用过的说就可以了。

![img](https://gitee.com/xudongyin/img/raw/master/img/20201009223515)

2、工厂方法模式和抽象工厂模式有什么区别？

- **工厂方法模式：**

一个抽象产品类，可以派生出多个具体产品类。 一个抽象工厂类，可以派生出多个具体工厂类。每个具体工厂类只能创建一个具体产品类的实例。 

- **抽象工厂模式：**

多个抽象产品类，每个抽象产品类可以派生出多个具体产品类。 一个抽象工厂类，可以派生出多个具体工厂类。每个具体工厂类可以创建多个具体产品类的实例。    

- **区别：**

工厂方法模式只有一个抽象产品类，而抽象工厂模式有多个。工厂方法模式的具体工厂类只能创建一个具体产品类的实例，而抽象工厂模式可以创建多个。

**3、JDK 中用到了哪些设计模式？**

几乎每一种设计模式都被用到了 JDK 的源码中，下面列举一些常见的：

- **抽象工厂模式**

```java
javax.xml.parsers.DocumentBuilderFactory#newInstance()
javax.xml.transform.TransformerFactory#newInstance()
```

- **建造者模式**

```java
java.lang.StringBuilder#append() 
java.lang.StringBuffer#append()
```

- **原型模式**

```java
java.lang.Object#clone()
```

- **适配器模式**

```java
java.util.Arrays#asList()
java.util.Collections#list()
```

- **装饰器模式**

```java
IO 流的子类
java.util.Collections#synchronizedXXX()
```

- **享元模式**

```java
java.lang.Integer#valueOf(int)
```

- **代理模式**

```java
java.lang.reflect.Proxy
javax.inject.Inject
```

- **责任链模式**

```java
java.util.logging.Logger#log()
javax.servlet.Filter#doFilter()
```

......

**4、Spring 中用到了哪些设计模式？**

1. 单例设计模式 : Spring 中的 Bean 默认都是单例的；

2. 代理设计模式 : Spring AOP 功能的实现；

3. 工厂设计模式 : Spring 使用工厂模式通过 BeanFactory、ApplicationContext 创建 Bean 对象；

4. 模板方法模式 : Spring 中 jdbcTemplate、hibernateTemplate 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式；

5. 装饰器设计模式 : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源；

6. 观察者模式：Spring 事件驱动模型就是观察者模式很经典的一个应用；

7. 适配器模式：Spring AOP 的增强或通知（Advice）使用到了适配器模式、SpringMVC 中也是用到了适配器模式适配 Controller。

5、设计模式六大原则是什么？

1. 单一职责原则：一个方法 一个类只负责一个职责，各个职责的程序改动，不影响其它程序。 

2. 开闭原则：对扩展开放，对修改关闭。即在不修改一个软件实体的基础上去扩展其他功能。

3. 里氏代换原则：在软件系统中，一个可以接受基类对象的地方必然可以接受一个子类对象。

4. 依赖倒转原则：针对于接口编程，依赖于抽象而不依赖于具体。

5. 接口隔离原则：使用多个隔离的接口取代一个统一的接口。降低类与类之间的耦合度。

6. 迪米特原则：一个实体应当尽量少的与其他实体之间发生相互作用，使得系统功能模块相对独立。

7. 单例模式的优缺点？

- **优点：**

由于在系统内存中只存在一个对象，因此可以节约系统资源，对于一些需要频繁创建和销毁的对象单例模式无疑可以提高系统的性能。

- **缺点：**

由于单例模式中没有抽象层，因此单例类的扩展有很大的困难。滥用单例将带来一些负面问题，如为了节省资源将数据库连接池对象设计为的单例类，可能会导致共享连接池对象的程序过多而出现连接池溢出；如果实例化的对象长时间不被利用，系统会认为是垃圾而被回收，这将导致对象状态的丢失。

==**8、请手写一下单例模式？**==

- **1. 懒汉式：用到时再去创建**

```java
public class Singleton {    
    private static  Singleton instance;    
    private Singleton(){};     
    public static synchronized Singleton getInstance(){        
        if(instance == null){            
            instance = new Singleton();        
        }        
        return instance;    
    }
}
```

- **2. 饿汉式：初始化时即创建，用到时直接返回**

```java
public class Singleton {    
    private static  Singleton instance = new Singleton();    
    private Singleton(){};     
    public static Singleton getInstance(){        
        return instance;   
    }
}
```

- **3. 静态内部类【推荐】**

```java
public class Singleton {
    private static class SingletonHolder{
        private static final Singleton INSTTANCE = new Singleton();
    }
 
    private Singleton(){};
 
    public static final Singleton getInstance(){
        return SingletonHolder.INSTTANCE;
    }
}
```

- **4. 双重校验锁【推荐】**

```java
public class Singleton {
    private volatile static Singleton singleton; 
    private Singleton(){};
    
    public static Singleton getSingleton(){
        if(singleton == null){
            synchronized(Singleton.class){
                if(singleton == null){
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    } 
}
```

9、树形文件目录采用的是哪种设计模式？

采用的是组合模式。树形结构在软件中随处可见，例如：操作系统中的目录结构、应用软件中的菜单、办公系统中的公司组织结构等等，如何运用面向对象的方式来处理这种树形结构是组合模式需要解决的问题，组合模式通过一种巧妙的设计方案使得用户可以一致性地处理整个树形结构或者树形结构的一部分，也可以一致性地处理树形结构中的叶子节点（不包含子节点的节点）和容器节点（包含子节点的节点）。