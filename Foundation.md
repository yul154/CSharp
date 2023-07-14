# C# 程序的通用结构
```
// A skeleton of a C# program
using System;

// Your program starts here:
Console.WriteLine("Hello world!");

namespace YourNamespace
{
    class YourClass
    {
    }

    struct YourStruct
    {
    }

    interface IYourInterface
    {
    }

    delegate int YourDelegate();

    enum YourEnum
    {
    }

    namespace YourNestedNamespace
    {
        struct YourStruct
        {
        }
    }
}
```

## Main() 和命令行参数

Main 方法是 C# 应用程序的入口点。 （库和服务不要求使用 Main 方法作为入口点）。
- Main 方法是应用程序启动后调用的第一个方法。
- C# 程序中只能有一个入口点。 如果多个类包含 Main 方法，必须使用 StartupObject 编译器选项来编译程序，以指定将哪个 Main 方法用作入口点。
- 当且仅当 Main 返回 Task 或 Task<int> 时，Main 的声明可包括 async 修饰符。 这明确排除了 async void Main 方法。

顶级语句: 不使用`Main`方法的程序
- 一个应用程序只能有一个入口点。
- 一个项目只能有一个包含顶级语句的文件
- 顶级语句隐式位于全局命名空间中。
- 具有顶级语句的文件还可以包含命名空间和类型定义，但它们必须位于顶级语句之后
- 顶级语句可以引用`args`变量来访问输入的任何命令行参数。`args`变量永远不会为 null，但如果未提供任何命令行参数，则其 Length 将为零
----
# C# 类型系统
 - C# 是一种强类型语言。 每个变量和常量都有一个类型，每个求值的表达式也是如此
 - 每个方法声明都为每个输入参数和返回值指定名称、类型和种类（值、引用或输出)

## 在变量声明中指定类型
> 当在程序中声明变量或常量时，必须指定其类型或使用 var 关键字让编译器推断类型

```
// Declaration only:
float temperature;
string name;
MyClass myClass;

// Declaration with initializers (four examples):
char firstLetter = 'C';
var limit = 3;
int[] source = { 0, 1, 2, 3, 4, 5 };
var query = from item in source
            where item <= limit
            select item;
```

## 通用类型系统
1. 它支持继承原则, 类型可以派生自其他类型（称为基类型）。
   - 派生类型继承（有一些限制）基类型的方法、属性和其他成员。
   - 基类型可以继而从某种其他类型派生，在这种情况下，派生类型继承其继承层次结构中的两种基类型的成员。
   - 所有类型（包括 System.Int32 (C# keyword: int) 等内置数值类型）最终都派生自单个基类型，即 System.Object (C# keyword: object)。
   - 这样的统一类型层次结构称为通用类型系统 (CTS)。
2. CTS 中的每种类型被定义为值类型或引用类型。
  - 这些类型包括 .NET 类库中的所有自定义类型以及你自己的用户定义类型。
  - 使用 struct 关键字定义的类型是值类型；所有内置数值类型都是 structs。
  - 使用 class 或 record 关键字定义的类型是引用类型。
  - 引用类型和值类型遵循不同的编译时规则和运行时行为。

类和结构是 .NET 通用类型系统的两种基本构造。 C# 9 添加记录，记录是一种类。
- 每种本质上都是一种数据结构
- 类是引用类型。 创建类型的对象后，向其分配对象的变量仅保留对相应内存的引用.类通常存储计划在创建类对象后进行修改的数据
- 结构是值类型。 创建结构时，向其分配结构的变量保留结构的实际数据。 将结构分配给新变量时，会复制结构,结构通常存储不打算在创建结构后修改的数据
- 记录类型可以是引用类型 (record class) 或值类型 (record struct)。记录类型是具有附加编译器合成成员的数据结构。 记录通常存储不打算在创建对象后修改的数据

**值类型**
> 值类型派生自System.ValueType（派生自 System.Object）。 派生自 System.ValueType 的类型在 CLR 中具有特殊行为

值类型分为两类：`struct`和`enum`
- 值类型已密封。 不能从任何值类型（例如 System.Int32）派生类型
- 一个结构可以实现一个或多个接口。 可将结构类型强制转换为其实现的任何接口类型。 这将导致“装箱”操作
- 当你将值类型传递给使用 System.Object 或任何接口类型作为输入参数的方法时，就会发生装箱操作

**引用类型**
> 定义为 class、record、delegate、数组或 interface 的类型是 reference type

在声明变量 reference type 时，它将包含值 null，直到你将其分配给该类型的实例，或者使用 new 运算符创建一个
- 所有数组都是引用类型，即使元素是值类型，也不例外

**文本值的类型(string)**
> 文本值从编译器接收类型。 可以通过在数字末尾追加一个字母来指定数字文本应采用的类型

**泛型类型**
> 可使用一个或多个类型参数声明的类型，用作实际类型（具体类型）的占位符

客户端代码在创建类型实例时提供具体类型。 这种类型称为泛型类型
- 通过使用类型参数，可重新使用相同类以保存任意类型的元素，且无需将每个元素转换为对象。
- 泛型集合类称为强类型集合，因为编译器知道集合元素的具体类型，并能在编译时引发错误

**隐式类型、匿名类型和可以为 null 的值类型**
- 使用 var 关键字隐式键入一个局部变量（但不是类成员）。 变量仍可在编译时获取类型
- 不方便为不打算存储或传递外部方法边界的简单相关值集合创建命名类型。 因此，可以创建匿名类型
- 可以在类型后面追加 ?，创建可为空的值类型

### 编译时类型和运行时类型
> 变量可以具有不同的编译时和运行时类型

- 编译时类型是源代码中变量的声明或推断类型
- 行时类型是该变量所引用的实例的类型


## 声明命名空间来整理类型

```
System.Console.WriteLine("Hello World!");
```
- System 是一个命名空间，Console 是该命名空间中的一个类。 可使用 using 关键字，这样就不必使用完整的名称
- 在较大的编程项目中，声明自己的命名空间可以帮助控制类和方法名称的范围

```
namespace SampleNamespace
{
    class SampleClass
    {
        public void SampleMethod()
        {
            System.Console.WriteLine(
                "SampleMethod inside SampleNamespace");
        }
    }
}
```

## 类

### 引用类型

定义为 class 的类型是引用类型。
- 在运行时，如果声明引用类型的变量，此变量就会一直包含值 null，直到使用 new 运算符显式创建类实例
- 或直到为此变量分配可能已在其他位置创建的兼容类型的对象，
- 由于基于类的对象是通过引用来实现其引用的，因此类被称为引用类型

```
//Declaring an object of type MyClass.
MyClass mc = new MyClass();

//Declaring another object of the same type, assigning it the value of the first object.
MyClass mc2 = mc;
```
### 声明类
```
//[access modifier] - [class] - [identifier]
public class Customer
{
   // Fields, properties, methods and events go here...
}

Customer object1 = new Customer(); //object1 是对基于 Customer 的对象的引用
```

### 构造函数和初始化

```
public class Container
{
    // Initialize capacity field to a default value of 10:
    private int _capacity = 10;
}

public class Container
{
    private int _capacity;

    public Container(int capacity) => _capacity = capacity;
}

public class Container(int capacity)
{
    private int _capacity = capacity;
}

public class Person
{
    public required string LastName { get; set; } //对某个属性使用 required 修饰符，并允许调用方使用对象初始值设定项来设置该属性的初始值
    public required string FirstName { get; set; }
}

// 添加 required 关键字要求调用方必须将这些属性设置为 new 表达式的一部
var p1= new Person(){ FirstName = "Grace", LastName = "Hopper" };
```

### 类继承
```
public class Manager : Employee
{
    // Employee fields, properties, methods and events are inherited
    // New Manager fields, properties, methods and events go here...
}
```
- 类可以声明为 abstract。 抽象类包含抽象方法，抽象方法包含签名定义但不包含实现
- 抽象类不能实例化


## 记录类型
> record 修饰符指示编译器合成对主要角色存储数据的类型有用的成员

记录是一个类或结构，它为使用数据模型提供特定的语法和行为
- 你想要定义依赖值相等性的数据模型。
- 你想要定义对象不可变的类型。

**值相等性**
> 值相等性表示当类型匹配且所有属性和字段值都匹配时，记录类型的两个变量相等

对于其他引用类型（例如类），相等性是指引用相等性: 如果类的两个变量引用同一个对象，则这两个变量是相等的

**不可变性**
> 不可变类型会阻止你在对象实例化后更改该对象的任何属性或字段值

记录与类和结构的区别
- 声明和实例化类或结构时使用的语法与操作记录时的相同,只是将 class 关键字替换为 record
- 在类中指示引用相等性或不相等的方法和运算符（例如 Object.Equals(Object) 和 ==）在记录中指示值相等性或不相等。
- 可使用 with 表达式对不可变对象创建在所选属性中具有新值的副本
- 记录的 ToString 方法会创建一个格式字符串，它显示对象的类型名称及其所有公共属性的名称和值
- 记录可从另一个记录继承。 但记录不可从类继承，类也不可从记录继承

## 泛型类和方法

```
// Declare the generic class.
public class GenericList<T>
{
    public void Add(T input) { }

    public IEnumerator<T> GetEnumerator()
    {
        Node? current = head;

        while (current != null)
        {
            yield return current.Data;
            current = current.Next;
        }
    }
}
```

## 匿名类型
> 匿名类型提供了一种方便的方法，可用来将一组只读属性封装到单个对象中，而无需首先显式定义一个类型

```
var product = new Product(); //通常，当使用匿名类型来初始化变量时，可以通过使用 var 将变量作为隐式键入的本地变量来进行声明
var bonus = new { note = "You won!" };
var anonArray = new[] { new { name = "apple", diam = 4 }, new { name = "grape", diam = 1 }};

```
匿名类型是 class 类型，它们直接派生自 object，并且无法强制转换为除 object 外的任何类型
