# C# 语言介绍
* C# 是面向对象的、面向组件的编程语言
* 垃圾回收自动回收不可访问的未用对象所占用的内存。 可以为 null 的类型可防范不引用已分配对象的变量。
* 异常处理提供了一种结构化且可扩展的方法来进行错误检测和恢复。
* 语言集成查询 (LINQ) 语法创建一个公共模式，用于处理来自任何源的数据
*  所有 C# 类型（包括 int 和 double 等基元类型）均继承自一个根 object 类型
* C# 允许动态分配轻型结构的对象和内嵌存储。
* C# 支持泛型方法和类型，因此增强了类型安全性和性能

----
# .NET 体系结构
> .NET 是名为公共语言运行时 (CLR) 的虚执行系统和一组类库

 * CLR 是 Microsoft 对公共语言基础结构 (CLI) 国际标准的实现。
 * CLI 是创建执行和开发环境的基础，语言和库可以在其中无缝地协同工作。
 * 用 C# 编写的源代码被编译成符合 CLI 规范的中间语言 (IL)
 * IL 代码和资源（如位图和字符串）存储在扩展名通常为 .dll 的程序集中

执行 C# 程序时
1. 程序集将加载到 CLR,CLR 会直接执行实时 (JIT) 编译，将 IL 代码转换成本机指令
2. CLR 可提供其他与自动垃圾回收、异常处理和资源管理相关的服务
3. CLR 执行的代码有时称为“托管代码”。而“非托管代码”被编译成面向特定平台的本机语言
---
# Hello world
```
using System; //引用 System 命名空间的 using 指令
class Hello{ //“Hello, World”程序声明的 Hello 类只有一个成员，即 Main 方法
    static void Main(){// Main 静态方法是 C# 程序的入口点
        Console.WriteLine("Hey You");
    }
}
```

命名空间提供了一种用于组织 C# 程序和库的分层方法。 命名空间包含类型和其他命名空间

实例方法可以使用关键字`this`引用特定的封闭对象实例，而静态方法则可以在不引用特定对象的情况下运行

----
# 类型和变量

**类型定义 C# 中的任何数据的结构和行为**
- 类型的声明可以包含其成员、基类型、它实现的接口和该类型允许的操作
- C# 程序使用类型声明创建新类型。 类型声明指定新类型的名称和成员
- C# 有两种类型：值类型和引用类型
  - 值类型的变量直接包含它们的数据,借助值类型，每个变量都有自己的数据副本
  - C# 的值类型进一步分为：简单类型、枚举类型、结构类型,可以为 null 的值类型和元组值类型
  - 引用类型的变量存储对数据（称为“对象”）的引用,对于引用类型，两个变量可以引用同一个对象；对一个变量执行的运算可能会影响另一个变量引用的对象
  - C# 引用类型又细分为类类型、接口类型、数组类型和委托类型。

值类型
- 简单类型
  - 有符号整型：sbyte、short、int、long
  - 无符号整型：byte、ushort、uint、ulong
  - Unicode 字符：char，表示 UTF-16 代码单元
  - IEEE 二进制浮点：float、double
  - 高精度十进制浮点数：decimal
  - 布尔值：bool，表示布尔值（true 或 false）
- 枚举类型
  - enum E {...} 格式的用户定义类型。 enum 类型是一种包含已命名常量的独特类型。 每个 enum 类型都有一个基础类型（必须是八种整型类型之一）。 enum 类型的值集与基础类型的值集相同。
- 结构类型
  - 格式为 struct S {...} 的用户定义类型
- 可以为 null 的值类型
  - 值为 null 的其他所有值类型的扩展
- 元组值类型
  - 格式为 (T1, T2, ...) 的用户定义类型

C# 程序使用类型声明创建新类型。 类型声明指定新类型的名称和成员

引用类型， 用户可定义以下六种 C# 类型：类类型、结构类型、接口类型、枚举类型、委托类型和元组值类型

- 类类型
  - 其他所有类型的最终基类：object
  - class 类型定义包含数据成员（字段）和函数成员（方法、属性及其他）的数据结构。 类类型支持单一继承和多形性，即派生类可以扩展和专门针对基类的机制。
- 结构类型
  - struct 类型定义包含数据成员和函数成员的结构，这一点与类类型相似。
  - 与类不同的是，结构是值类型，通常不需要进行堆分配。 结构类型不支持用户指定的继承，并且所有结构类型均隐式继承自类型object
- Unicode 字符串：string，表示 UTF-16 代码单元序列
  - 格式为 class C {...} 的用户定义类型
- 接口类型
  - 格式为 interface I {...} 的用户定义类型
  - interface 类型将协定定义为一组已命名的公共成员。
  - 实现 interface 的 class 或 struct 必须提供接口成员的实现代码
  -  interface 可以继承自多个基接口
  -  class 和 struct 可以实现多个接口
- 数组类型
  - 一维、多维和交错。 例如：int[]、int[,] 和 int[][]
- 委托类型
  - 格式为 delegate int D(...) 的用户定义类型
  - delegate 类型表示引用包含特定参数列表和返回类型的方法。
  - 通过委托，可以将方法视为可分配给变量并可作为参数传递的实体
  - 委托类同于函数式语言提供的函数类型
  - 它们还类似于其他一些语言中存在的“函数指针”概念。 与函数指针不同，委托是面向对象且类型安全的。

class、struct、interface 和 delegate 类型全部都支持泛型

数组类型是通过在类型名称后面添加方括号构造而成
```
int[]   //是 int 类型的一维数组
int[,] // 是 int 类型的二维数组，
int[][] // 是由 int 类型的一维数组或“交错”数组构成的一维数组
```
可以为 null 的类型不需要单独定义。 对于所有不可以为 null 的类型 T，都有对应的可以为 null 的类型 T?
* int? 是可保存任何 32 位整数或 null 值的类型，string? 是可以保存任何 string 或 null 值的类型

将值类型的值分配给 object 对象引用时，会分配一个“箱”来保存此值
- 该箱是引用类型的实例，此值会被复制到该箱
- 当 object 引用被显式转换成值类型时，将检查引用的 object 是否是具有正确值类型的箱


**变量: 是用于引用特定类型的实例的标签**
- 标识符是变量名称。 标识符是不包含任何空格的 unicode 字符序列
- 如果标识符的前缀为`@`，则该标识符可以是 C# 保留字
- C# 有多种变量，其中包括字段、数组元素、局部变量和参数
- 每个变量都具有一种类型，用于确定可以在变量中存储哪些值

- 不可以为 null 的值类型: 具有精确类型的值
- 可以为 null 的值类型: null 值或具有精确类型的值
- object: null 引用、对任意引用类型的对象的引用，或对任意值类型的装箱值的引用
- 类类型: null 引用、对类类型实例的引用，或对派生自类类型的类实例的引用
- 接口类型: null 引用、对实现接口类型的类类型实例的引用，或对实现接口类型的值类型的装箱值的引用
- 数组类型: null 引用、对数组类型实例的引用，或对兼容的数组类型实例的引用
- 委托类型: null 引用或对兼容的委托类型实例的引用
  
----
# 程序结构

```
namespace Acme.Collections;// 此类的完全限定的名称Acme.Collections.Stack

public class Stack<T> //Stack 是泛型类。 它具有一个类型参数 T，在使用时替换为具体类型
{
    Entry _top; //一个 _top 字段

    public void Push(T data)
    {
        _top = new Entry(_top, data);
    }

    public T Pop()
    {
        if (_top == null)
        {
            throw new InvalidOperationException();
        }
        T result = _top.Data;
        _top = _top.Next;

        return result;
    }

    class Entry// 嵌套类
    {
        public Entry Next { get; set; }// 属性
        public T Data { get; set; }//属性

        public Entry(Entry next, T data)// 构造函数
        {
            Next = next;
            Data = data;
        }
    }
}
```

**属性（Property）和字段（Field）的区别**
- Field是一种表示与对象或类关联的变量的成员，字段声明用于引入一个或多个给定类型的字段。
  - 字段是类内部用的，private类型的变量(字段)
  - 通常字段写法都是加个"_"符号，然后声明只读属性，字段用来储存数据
- Property是另一种类型的类成员，定义属性的目的是在于便于一些私有字段的访问。
  - 类提供给外部调用时用的可以设置或读取一个值，属性则是对字段的封装，将字段和访问自己字段的方法组合在一起，提供灵活的机制来读取、编写或计算私有字段的值。
  - 属性有自己的名称，并且包含get访问器和set访问器

----
# C# 类型和成员

C# 支持封装、继承和多态性这些概念。 
- 类可能会直接继承一个父类，并且可以实现任意数量的接口。
- 若要用方法重写父类中的虚方法，必须使用`override`关键字，以免发生意外重定义
-  C# 提供了`record class`和`record struct`类型，这些类型的目的主要是存储数据值

所有类型都通过构造函数（负责初始化实例的方法）进行初始化。 两个构造函数声明具有唯一的行为
- 无参数构造函数，该构造函数将所有字段初始化为其默认值。
- 主构造函数，该构造函数声明该类型的实例的必需参数。

## 类和对象

类: 最基本的 C# 类型。 类是一种数据结构，可在一个单元中就将状态（字段）和操作（方法和其他函数成员）结合起来
- 类为类实例（亦称为“对象”）提供了定义 。
- 类支持继承和多形性，即派生类可以扩展和专门针对基类的机制。

新类使用类声明进行创建。 类声明以标头开头。 标头指定以下内容：
1. 类的特性和修饰符
2. 类的名称
3. 基类（从基类继承时）
4. 接口由该类实现。

标头后面是类主体，由在分隔符 `{  }`内编写的成员声明列表组成

## 类型参数

类型参数是用尖括号括起来的类型参数名称列表
```
public class Pair<TFirst, TSecond>
{
    public TFirst First { get; }
    public TSecond Second { get; }
    
    public Pair(TFirst first, TSecond second) => 
        (First, Second) = (first, second);
}
```
声明为需要使用类型参数的类类型被称为泛型类类型。
- 结构、接口和委托类型也可以是泛型。
- 使用泛型类时，必须为每个类型参数提供类型自变量

## 基类
> 类声明可以指定基类。 在类名和类型参数后面加上冒号和基类的名称
```
public class Point3D : Point
{
    public int Z { get; set; }
    
    public Point3D(int x, int y, int z) : base(x, y)
    {
        Z = z;
    }
}
```
继承意味着一个类隐式包含其基类的几乎所有成员
- 派生类可以在其继承的成员中添加新成员，但无法删除继承成员的定义
- 可以将类类型隐式转换成其任意基类类型
- 类类型的变量可以引用相应类的实例或任意派生类的实例
```
Point a = new Point3D(1,2,3);
```

## 结构
> 存储数据值

- 结构不能声明基类型；它们从`System.ValueType`隐式派生。 
- 不能从`struct`类型派生其他`struct`类型。 这些类型已隐式密封。

## 接口
> 接口定义了可由类和结构实现的协定,定义接口来声明在不同类型之间共享的功能

 - 接口可以包含方法、属性、事件和索引器
 - 接口通常不提供所定义成员的实现

接口可以采用“多重继承”
```
interface IControl
{
    void Paint();
}

interface ITextBox : IControl
{
    void SetText(string text);
}

interface IListBox : IControl
{
    void SetItems(string[] items);
}

interface IComboBox : ITextBox, IListBox { }
```

类和结构可以实现多个接口
```
interface IDataBound
{
    void Bind(Binder b);
}

public class EditBox : IControl, IDataBound
{
    public void Paint() { }
    public void Bind(Binder b) { }
}
```

当类或结构实现特定接口时，此类或结构的实例可以隐式转换成相应的接口类型。
```
EditBox ed = new ();
IControl control = ed;
IDataBound id = ed;
```


## 枚举
```
public enum SomeRootVegetable
{
    HorseRadish,
    Radish,
    Turnip
}
```

还可以定义一个 enum 作为标志组合使用
```
public enum Seasons
{
    None = 0,
    Summer = 1,
    Autumn = 2,
    Winter = 4,
    Spring = 8,
    All = Summer | Autumn | Winter | Spring
}
```

## 可为 null 的类型
> 任何类型的变量都可以声明为“不可为 null”或“可为 null”_

- 可为 null 的值类型（结构或枚举）由 `System.Nullable<T>`表示
- 不可为 null 和可为 null 的引用类型都由基础引用类型表示
-  当可为 null 的引用在没有先对照 null 检查其值的情况下取消引用时，编译器会发出警告

```
optionalInt = 5;
string? optionalText = default;
optionalText = "Hello World.";
```

## 元组
> 提供了简洁的语法来将多个数据元素分组成一个轻型数据结构

通过声明 ( 和 ) 之间的成员的类型和名称来实例化元组
```
(int a, int b) t2 = (1,2)
```
---
# C# 程序构建基

## 成员

class 的成员要么是静态成员，要么是实例成员
-  静态成员属于类
-  实例成员则属于对象（类实例）

Terms
- 常量：与类相关联的常量值
- 字段：与类关联的变量
- 方法：类可执行的操作
- 属性：与读取和写入类的已命名属性相关联的操作
- 索引器：与将类实例编入索引（像处理数组一样）相关联的操作
- 事件：类可以生成的通知
- 运算符：类支持的转换和表达式运算符
- 构造函数：初始化类实例或类本身所需的操作
- 终结器：永久放弃类的实例之前完成的操作
- 类型：类声明的嵌套类型

### 字段
> 字段是与类或类实例相关联的变量。

使用`static`修饰符声明的字段定义的是静态字段。 静态字段只指明一个存储位置。 无论创建多少个类实例，永远只有一个静态字段副本。

不使用静态修饰符声明的字段定义的是实例字段。每个类实例均包含相应类的所有实例字段的单独副本

```
public class Color{
      public static readonly Color Black = new(0,0,0); //使用`readonly`修饰符声明只读字段。只能在字段声明期间或在同一个类的构造函数中向只读字段赋值
      public static readonly Color White = new(255,255,255);

      public byte R;
      public byte G;
      public byte B;
      public Color(byte r, byte g, byte b) => (R,G,B) = (r,g,b);
}
```

### 方法
> 方法是实现对象或类可执行的计算或操作的成员

 静态方法是通过类进行访问。 实例方法是通过类实例进行访问

 当方法主体是单个表达式时，可使用紧凑表达式格式定义方法
```
public override string ToString() => "This is an object";

```

参数: 用于将值或变量引用传递给方法
-  有四类参数：值参数、引用参数、输出参数和参数数组
-  值参数用于传递输入自变量,修改值形参不会影响为其传递的实参
-  值参数：修改值形参不会影响为其传递的实参
-  引用参数：指出的存储位置与自变量相同，引用参数使用`ref`修饰符进行声明
-  输出参数:用于按引用传递自变量,不要求向调用方提供的自变量显式赋值,输出参数使用 out 修饰符进行声明
-  参数数组允许向方法传递数量不定的自变量, 参数数组使用 params 修饰符进行声明。 参数数组只能是方法的最后一个参数，且参数数组的类型必须是一维数组类型

```
static void Divide(int x, int y, out int quotient, out int remainder)
{
    quotient = x / y;
    remainder = x % y;
}

public static void OutUsage()
{
    Divide(10, 3, out int quo, out int rem);
    Console.WriteLine($"{quo} {rem}");	// "3 1"
}
``` 

### 可访问性
- public：访问不受限制。
- private：访问仅限于此类。
- protected：访问仅限于此类或派生自此类的类。
- internal：仅可访问当前程序集（.exe 或 .dll）。
- protected internal：仅可访问此类、从此类中派生的类，或者同一程序集中的类。
- private protected：仅可访问此类或同一程序集中从此类中派生的类。

### 方法主体和局部变量
> 方法主体指定了在调用方法时执行的语句,局部变量是方法主体可以声明特定于方法调用的变量

C# 要求必须先明确赋值局部变量，然后才能获取其值


### 虚方法、重写方法和抽象方法
- 虚方法是在基类中声明和实现的方法，其中任何派生类都可提供更具体的实现
  - 调用虚方法时，为其调用方法的实例的运行时类型决定了要调用的实际方法实现代码。
  - 调用非虚方法时，实例的编译时类型是决定性因素
  - 可以在派生类中重写虚方法。 如果实例方法声明中有`override`修饰符，那么实例方法可以重写签名相同的继承虚方法
- 重写方法是在派生类中实现的方法，可修改基类实现的行为
- 抽象方法是在基类中声明的方法，必须在所有派生类中重写，抽象方法不在基类中定义实现.
  - 抽象方法是没有实现代码的虚方法。 抽象方法使用`abstract`修饰符进行声明，仅可在抽象类中使用

变量的类型确定了其编译时类型。 编译时类型是编译器用于确定其成员的类型
- 可将变量分配给从其编译时类型派生的任何类型的实例
- 运行时间类型是变量所引用的实际实例的类型

```
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```

方法重载: 同一类中可以有多个同名的方法，只要这些方法具有唯一签名即可
- 编译器使用重载决策来确定要调用的特定方法。 重载决策会查找与自变量匹配度最高的一种方法。 如果找不到任何最佳匹配项，则会报告错误
- 

### 构造函数
C# 支持实例和静态构造函数
- 实例构造函数是实现初始化类实例所需执行的操作的成员
- 静态构造函数是实现在首次加载类时初始化类本身所需执行的操作的成员。

构造函数的声明方式与方法一样，都没有返回类型，且与所含类同名。
- 如果构造函数声明包含`static`修饰符，则声明的是静态构造函数。 否则，声明的是实例构造函数。


### “属性”

属性是字段的自然扩展。 两者都是包含关联类型的已命名成员，用于访问字段和属性的语法也是一样的
- 与字段不同的是，属性不指明存储位置
- 属性包含访问器，用于指定在读取或写入属性值时执行的语句。 get 访问器读取该值。 set 访问器写入该值
- 属性的声明方式与字段相似，区别是属性声明以在分隔符 { 和 } 之间写入的 get 访问器或 set 访问器结束，而不是以分号结束
- 同时具有 get 访问器和 set 访问器的属性是“读写属性”。 只有 get 访问器的属性是“只读属性”。 只有 set 访问器的属性是“只写属性”


### 索引器
> 可以将对象编入索引（像处理数组一样）

索引器的声明方式与属性类似，不同之处在于，索引器成员名称格式为`this`后跟在分隔符 [ 和 ] 内写入的参数列表
- 索引器可被重载。 一个类可声明多个索引器，只要其参数的数量或类型不同即可

### 事件
> 借助事件成员，类或对象可以提供通知

事件声明包括`event`关键字，且类型必须是委托类型
- 在声明事件成员的类中，事件的行为与委托类型的字段完全相同（前提是事件不是抽象的，且不声明访问器）。
- 字段存储对委托的引用，委托表示已添加到事件的事件处理程序。 如果没有任何事件处理程序，则字段为 null。

### 运算符

所有运算符都必须声明为 public 和 static
```
public static bool operator ==(MyList<T> a, MyList<T> b) =>
        Equals(a, b);

public static bool operator !=(MyList<T> a, MyList<T> b) =>
        !Equals(a, b);
```
----
# C# 主要语言区域

## 数组、集合和 LINQ

- 泛型集合类型列在 System.Collections.Generic 命名空间中。
- 专用集合包括 System.Span<T>（用于访问堆栈帧上的连续内存），
- System.Memory<T>（用于访问托管堆上的连续内存）。
- 所有集合（包括数组、Span<T> 和 Memory<T>）都遵循一种统一的迭代原则.使用 System.Collections.Generic.IEnumerable<T> 接口

### 数组
> 数组中的变量（亦称为数组的“元素”）均为同一种类型

数组类型是引用类型
- 声明数组变量只是为引用数组实例预留空间
- 实际的数组实例是在运行时使用 new 运算符动态创建而成

包含数组类型元素的数组有时称为“交错数组”,因为元素数组的长度不必全都一样
```
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20]
```

### 字符串内插
```
Console.WriteLine($"The low and high temperature on {weatherData.Date:MM-dd-yyyy}");
Console.WriteLine($"    was {weatherData.LowTemp} and {weatherData.HighTemp}.");
```
内插字符串通过 $ 标记来声明。 字符串插内插计算 { 和 } 之间的表达式


### 委托和 Lambda 表达式

委托类型表示对具有特定参数列表和返回类型的方法的引用。 通过委托，可以将方法视为可分配给变量并可作为参数传递的实体

```
delegate double Function(double x);

class Multiplier
{
    double _factor;

    public Multiplier(double factor) => _factor = factor;

    public double Multiply(double x) => x * _factor;
}

class DelegateExample
{
    static double[] Apply(double[] a, Function f)
    {
        var result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    public static void Main()
    {
        double[] a = { 0.0, 0.5, 1.0 };
        double[] squares = Apply(a, (x) => x * x);
        double[] sines = Apply(a, Math.Sin);
        Multiplier m = new(2.0);
        double[] doubles = Apply(a, m.Multiply);
    }
}
```
