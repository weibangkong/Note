<h1 align = "center">
    JVM
</h1>
## 1.类文件结构

Class文件是一组一八字节为基础单位的二进制流，整个Class文件中存储的内容几乎全部是程序运行的必要数据，当遇到需要占用8位字节以上空间的数据项时，会按照高位在前的方式分割成若干个8位字节进行存储

Class文件中只有两种文件接口:无符号数和表

无符号数属于基本的数据类型，可以用来描述数字、索引引用、数量值或者按照UTF-8编码构成字符串值

表是由多个无符号数或者其他表作为数据项构成的符合数据类型，所有表都习惯性的以_info结尾，表用于描述有层次关系的符合结构的数据，整个Class文件本质上就是一张表

ps：

高位在前

#### 1.常量池

常量池是Class文件里占用空间最大的数据项目，是一个表，也是Class文件种出现的第一个表类型的数据项目。

常量池中常量数目是不固定的，所以常量池入口需要一个一个两字节(u2)的数据，代表常量池容量技术值。

常量池计数是从1开始的，容量为十六进制的0x0016即十进制的22，代表常量池中有21项常量(22-1),第0项常量空出来是为了满足后面某些只想常量池的索引值的数据在特定情况下需要表达“不引用任何一个常量池项目"的含义。

##### 常量池主要存放内容

- 字面量：文本字符串、声明为final的常量值等

- 符号引用：类和接口的全限定名；字段的名称和描述符；方法的名称和描述符

  ps: Java代码在进行Javac编译的时候，不像C和C++有连接这一步骤，而是在虚拟机加载Class文件的时候进行动态连接，即在Class文件中并不会保存各方法字段在内存中的最终分布信息，因此这些字段和方法的符号引用不经过运行期转换的话无法得到真正的内存入口地址，也就无法在虚拟机中使用。当虚拟机运行时，需要从常量池中获得对应的符号引用，再在类创建或运行时解析、翻译到具体的内存地址中

常量池中的每一项常量都是一个表，jdk1.7新增3各支持动态语言调用的表，共计14个表，他们都有共同的特点，即表开头的第一位是一个1字节(u1)的标志位(tag)代表这个常量属于哪种常量类型

| 类型                             | 标志 | 描述                     | 结构 |
| -------------------------------- | ---- | ------------------------ | ---- |
| CONSTANT_Utf8_info               | 1    | UTF-8编码的字符串        |      |
| CONSTANT_Integer_info            | 3    | 整型字面量               |      |
| CONSTANT_Float_info              | 4    | 浮点型字面量             |      |
| CONSTANT_Long_info               | 5    | 长整型字面量             |      |
| CONSTANT_Double_info             | 6    | 双精度浮点型字面量       |      |
| CONSTANT_Class_info              | 7    | 类或接口的符号引用       |      |
| CONSTANT_String_info             | 8    | 字符串类型字面量         |      |
| CONSTANT_Fieldref_info           | 9    | 字段的符号引用           |      |
| CONSTANT_Methdoref_info          | 10   | 类中方法的符号引用       |      |
| CONSTANT_InterfaceMethodref_info | 11   | 接口中方法的符号引用     |      |
| CONSTANT_NameAndType_info        | 12   | 字段或方法的部分符号引用 |      |
| CONSTANT_MethodHandle_info       | 15   | 方法句柄                 |      |
| CONSTANT_MethdoType_info         | 16   | 标识方法类型             |      |
| CONSTANT_InvokeDynamic_info      | 18   | 动态方法调用点           |      |



## 2.类加载

### 类加载过程:

#### 1:加载

##### 加载阶段要完成的三件事:

1. 通过一个类的全限定明来获取定义该类的二进制字节流
2. 将字节流所嗲表的静态存储结构转化为方法去的运行时的数据结构
3. 在内存中生成一个代表该类的Class对象，作为方法区这个类的各种数据的访问入口

##### **jvm规范所规定的  *有且仅有*  的5种必须执行  *初始化*  的情况：**

1. new、getstatic、putstatic、invokestatic这四个字节码指令时，如果类没有进行过初始化，则需要先触发其初始化；new对应使用new关键字实例化对象；getstatic/putstatic对应读取或设置一个类属性时(被final修饰、或者已经在编译器放入到常量池中的静态字段除外)；invokestatic对应调用一个类的静态方法

2. 使用java.lang.reflect包下的方法对类进行反射调用的时候

3. 当初始化一个类的时候，发现他的父类还没有进行过初始化，则对父类进行初始化

4. 当虚拟机(JVM)启动时,用户需要指定一个要执行的主类(包含main()),虚拟机会先初始化这个主类

5. 使用动态语言支持(JDK1.7)时,如果一个java.lang.invoke.MethdoHandle实例最后的解析结果REF_getstatic、

   REF_putstatic、REF_invokeStatic的方法句柄，并且这个句柄所对应的类没有进行过初始化，则先出发其初始化

**以上五种   *有且仅有*   的初始化情况被称为对一个类进行  *主动引用*  ,除此之外的所有引用类的方法都不会触发初始化，这种情况叫做   *被动引用***

##### 三种常见的被动引用的情况:

- 通过子类引用父类中的静态变量
- 通过数组定义类，不会触发这个类的初始化
- 常量是在编译器存入调用类的常量池中，本质上没有直接引用直接定义常量的类

ps: 接口的初始化过程和类是一致的，但是他不会去接在其父接口，而是真正使用到父接口的时候才会初始化

#### 2:连接

##### 	2.1:**验证**

验证Class文件的字节流中包含的信息是否符合当前虚拟机的要求，并且不会危害虚拟机自身的安全

1. **文件格式验证**

   是对字节流的操作，目的是为了保证输入的字节流能正确解析并存储于方法区之内，格式上满足描述Java类型信息的要求

   是否以魔数(0xCAFEBASE)开头

   主次版本号是否在当前虚拟机处理范围之内

   常量池中的常量是否油不被支持的常量类型(检查常量Tag标志)

   指向常量的各种所以只中是否油直线不存在的常量或不符合UTF8编码的数据

   CONSTANT_Utf8_info型的常量中是否油不符合UTF8编码的数据

   Class文件中各个部分及文件本身是否油被删除的或附加的其他信息

   目的:保证输入的字节流能正确的解析并存储与方法区之内，格式上符合描述一个Java类型信息的要求

   这阶段的验证是基于二进制字节流进行的，只有通过该阶段的雁阵过后，字节流才会进入内存的方法区中进行存储，其余验证阶段都不会再直接操作字节流

2. **元数据验证**:

   对字节码描述的信息进行语义分析（主要对类的元数据信息进行语义校验），来保证描述的信息符合Java语言规范(保证不存在不符合Java语言规范的元数据信息)

   可能验证的东西:

   - 是否有父类(除java.lang.Object)

   - 是否继承了被final修饰的类(不允许被继承)

   - 该类是不是抽象类，实现类是否实现了父类和接口之中所有要求实现的方法

   - 类中的字段、方法是否与父类产生矛盾(如覆盖父类final字段，出现不符合规则的重载:参数一直，但是返回值类型不一致)

3. **字节码验证**:

   该阶段是整个验证过程中最复炸的一个阶段，主要目的是通过数据流和控制流分析，确定程序语义是合法的、符合逻辑的；当元数据校验完成后，该阶段将对类的方法体进行校验分析，保证被校验的类的方法在运行时不会做出危害虚拟机的安全事件。

4. **符号引用验证**:

##### 	2.2:**准备**

##### 	2.3:**解析**

#### 3:初始化

