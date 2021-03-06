                                                 Core-Java
1.核心概念
    抽象，继承，封装，多态
2.数据类型:
    基本类型:
        整型:byte(1字节)、short(2字节)、int(4字节)、long(8字节)
        浮点型:float(单精度浮点型),double(双精度浮点型)
        字符型:char
        boolean型
    ps:
        整型和boolean之间不能进行相互转换

3.== 与 equals
    ==:
        ==作用与基本数据类型比较的是变量所保存的值，作用于引用类型，比较的则时引用类型所引用对象的内存地址
    equals:
        equals不能作用于基本数据类型的变量，作用于引用类型的话，如有类型没有对equals方法进行重写的话比较的认识引用类型所引用对象的内存地址，
        如果重写，比较的则是变量所引用对象的值

4.String等引用对象的声明，引用，==
    引用对象在new是会实例化一个新的对象用以保存不同的内存地址

6.重写与重载:
    都是方法的多态性实现，区别在于重载是编译时多态，重写是运行时多态；重载发生在同一个类中，通过参数类别或数量不同来实现，重写发生在子类与父类之间，
    重写要求子类与父类具有相同的参数类型、数量与返回类型，要求被重写方法要比父类中更好访问，不能比原来有更多的异常(里氏替换原则)，重载对参数类型没有要求

7.abstract class(抽象类) 与 interface(接口)异同:
    都不可以实例化，都可以定义引用，一个类如果继承某个抽象类或者实现对口都需要对其中的抽象方法进行全部实现，否则该类仍需被声明为抽象类；
    接口比抽象类更抽象，在抽象类中可以定义构造器，可以有抽象方法和具体方法，而接口中不能定义构造器而且其中的方法必须为抽象方法，JDK1.8开始允许有一个默认实现；
    抽象类中访问修饰符可以是private、默认、protected、public，接口中只能是public；
    抽象类中可以定义成员变量，而接口中定义的成员变量实际都是常量；

    ps:
        有抽象方法的类必须是抽象类，但是抽象类不是必须有抽象方法。
8.静态变量(类变量)和非静态变量(实例变量):
    静态变量:被static修饰的变量，静态变量属于类，不属于任何一个对象(实例),可以由同一个类的对象共享，不论实例化了对少对象，其在内存中只存在一个拷贝
    非静态变量:必须依存以一个已经实例化的对象,通过对象访问

    ps:
        例如:在上下文或者工具类中大量使用静态变量
9.静态方法和非静态方法:
    在静态方法中不可以调用非静态方法

9.Static Nested Class (静态嵌套类)与Inner Class(内部类)不同
    前者是被声明成为静态的内部类，在使用时不依赖外部类实例被实例化,而通常内部类需要在外部类实例化后才能实例化
    例:
        public class OuterClass {
            private String say = "fun?";
            public String getSay() {
                return say;
            }

            class InnerClass {
                private String say = "Im not fun!";
                public String getSay() {
                    return say;
                }
            }

            static class StaticInnerClass {
                private String say = "what`s wrong?";

                public String getSay() {
                    return say;
                }
            }

        }


        public class Main {
            public static void main(String[] args) {
                OuterClass outerClass = new OuterClass();
                //内部类的实例化
                OuterClass.InnerClass innerClass = outerClass.new InnerClass();
                System.out.println(outerClass.getSay());
                System.out.println(innerClass.getSay());
                //静态嵌套类的实例化
                OuterClass.StaticInnerClass staticInnerClass = new OuterClass.StaticInnerClass();
                System.out.println(staticInnerClass.getSay());
            }
        }

8.类加载机制:
    类加载的三个步骤:load(装载)、link(链接)、Initialize(初始化)
    JVM中类的装载是由ClassLoader(类加载器)及其子类实现的;
    由于Java的跨平性，经过编译的java源程序并不是一个可执行的程序，而是一个类或多个文件，当Java程序需要使用某个类时，JVM会确保这个类已经被加载、连接、初始化
        类的装载:指把类的.class文件中的数据以字节数组的形式读到内存中，然后产生与所加载类对应的Class对象。完成类的加载，Class对象并不完整，仍是不可运行的。
        类的链接:类被加载完成后就进入了连接阶段，该阶段包括验证、准备、和解析三个步骤。
            验证是指指确保被加载类信息符合JVM规范、不存在安全性的问题；
            准备是指为静态变量分配内存并设置默认的初始值；
            解析是指将虚拟机常量池中的符号引用替换为直接引用。(该处常量值时以类为单位，虚拟机为每个加载的类维护一个常量池，该常量池只是该类的字面量(如类名，方法名)
            ，不同于字符串的常量池是整个JVM共享的)
        类的初始化:检测类是否存在直接父类未被初始化，如果存在，则将直接父类初始化；检测是类中否存在初始化语句，如果存在则按顺序执行初始化语句。

    ps:
        类加载器包括:         (大致分为两种，第一种是系统提供，另一种是开发人员自定义的)
            引导类加载器:
                BootStrapClassLoader:用以加载Java的核心ku(jre/lib/rt.jar),C源实现，不继承自ClassLoader
                加载扩展类加载器和应用类加载器，并为其指定父类加载器，在Java中不可获取
            扩展类加载器:
                ExtensionClassLoader:用以从jre/lib/ext目录加载 “标准的扩展”
            系统类加载器:
                SystemClassLoader:用于加载应用类,从环境变量classpath或者系统属性java.class.path所指定的目录中加载类
            自定义类加载器:
                默认以系统类加载器作为其父加载器

Out of memory(内存溢出)与memory leak(内存泄漏):
    溢出:程序在申请内存时，没有足够的空间供其使用(简单说就是内存不足)。
    泄露:指无用对象(无引用对象)持续占用内存或者内存得不到释放。在程序申请到内存后，无法释放已申请申请到的内存空间,按照发生的方式来区分，分为内存泄漏分为四种:
        常发性内存泄漏:发生内存泄漏的代码会被多次执行，没吃执行都会引发一块内存泄漏。
        偶发性内存泄漏:可发生内存泄漏的代码只有在某些特定情况或操作才会发生内存泄漏。
        一次性内存泄漏:发生内存泄漏的代码只会执行一次，或是由于算法缺陷，导致总会有一块且仅有一块内存发生泄漏。例如在构造函数中分配内存，却没有在析构函数中释放
        隐式内存泄漏:程序在运行过程中不断分配内存，但是知道程序结束时才释放内存。(严格说这并没有发生内存泄漏，因为最终还是释放掉了，因此成为隐式泄漏)

    ps:
        内存泄漏最终会导致内存移除。
        内存泄漏本身并不会产生什么危害，但是如果内存泄漏堆积最终导致内存溢出，会耗尽系统所有内存，产生严重危害。
        一次性内存泄漏危害性并不大，隐式泄漏危害最大，因为与常见性和偶发性相比较难发现。


String 、StringBuilder 、StringBuffer的区别
    String是定长的，赋值后不可修改的，StringBuilder和StringBuffer是可变长的
    StringBuilder与StringBuffer是使用的缓冲技术的，具有自动扩容的功能
    StringBuffer是线程安全的StringBuilder是线程不安全的

    ps:
        String 是使用享元模式实现的，每次拼接都要重新分配空间，StringBuffer与StringBuilder是可以把先拼接的字符串保存起来，到最后调用
        toString()时一次分配空间

        String str = "abcde" 等价于String str = "abc" + "d" +"e" ,二者没有区别
        String str = "abc"; str = str + "de"; 前后两个str的对象并不是一个，且慢

java深拷贝与浅拷贝和值传递和引用传递:
     忘了
