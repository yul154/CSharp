# Generics

.NET 1 had three broad kinds of collections:
1. Arrays -  statically typed, size is fixed at initialization
2. ArrayList - Not refer o the element type, Adding a completely different type will case InvalidCastException
3. StringCollection - statically typed, same logic only used in one type

`List<T>` solves all the problems
1. Not fixed size
2. The exposed API uses T everywhere it needs to refer to the element type
3. Able to use it with any element type without worrying about generating code and managing the result

**TYPE PARAMETERS AND TYPE ARGUMENTS**

The declaration of a generic type or method includes type parameters in angle brackets after the name
```
public class List<T> // Type parameter

List<String> list = new List<String>(); // type arguments
```

Type parameters are placeholders for types that are used in generic types or methods.
* Type parameters are defined using angle brackets (< and >) and are typically represented by a single uppercase letter,
* Type parameters can also have constraints that specify which types can be used

Type arguments are the actual types that are provided when instantiating a generic type or invoking a generic method.

```
public class MyClass<T> where T : IComparable, new() //the type parameter T must implement the IComparable interface and have a parameterless constructor.
{
    public T MyProperty { get; set; }
    public void MyMethod(T value)
    {
        // method implementation
    }
```


generic types can use their type parameters as type arguments when declaring a base class or an implemented interface
```
public class List<T> : IEnumerable<T>
```


**ARITY OF GENERIC TYPES AND METHODS**
> Generic types or methods can declare multiple type parameters by separating them with commas within the angle brackets.

```
public class Dictionary<TKey, TValue>
```

Rule of overload
1. multiple types can have the same name but different generic arity,overloading based on the number of type parameter, can’t overload solely by type parameter name
2. can’t give two type parameters in the same declaration the same name just like you can’t declare two regular parameters the same name.



**What can be generic?**

All of the following have to be non- generic:
* Fields
* Properties
* Indexers
*  Constructors
*  Events
* Finalizers


**Type inference for type arguments to methods**

type inference applies only to methods

```
new Tuple<int, string, int>(10, "x", 20) // .net 1
Tuple.Create(10, "x", 20) // .net 4
```

**Type constraints**

1. Reference type constraint — whereT:class. The type argument must be a refer- ence type. (Don’t be fooled by the use of the class keyword; it can be any ref- erence type, including interfaces and delegates.)
2. Value type constraint — whereT:struct. The type argument must be a non- nullable value type (either a struct or an enum). Nullable value types (described in section 2.2) don’t meet this constraint.
3. Constructor constraint — whereT:new(). The type argument must have a public parameterless constructor. This enables the use of new T() within the body of the code to construct a new instance of T.
4. Conversion constraint —where T : SomeType. Here, SomeType can be a class, an interface

When multiple type parameters exist in a generic declaration,
```   
TResult Method<TArg, TResult>(TArg input)
    where TArg : IComparable<TArg>
    where TResult : class, new()
```

**The `default` and `typeof` operators**

Default : The same value you’d get if you declared a field and didn’t immediately assign a value to it 
  - For reference types, that’s a null reference;
  - For non-nullable value types, it’s the “all zeroes” value (0, 0.0, 0.0m, false, the UTF-16 code unit with a numerical value of 0, and so on)



The `typeof` operator is slightly more complex. There are four broad cases to consider:
*  No generics involved at all; for example, typeof(string)
*  Generics involved but no type parameters; for example, typeof(List<int>)
*  Just a type parameter; for example, typeof(T)
*  Generics involved using a type parameter in the operand; for example,
typeof(List<TItem>) within a generic method declaring a type parameter
called TItem
* Generics involved but no type arguments specified in the operand; for exam-
ple, typeof(List<>)


**Generic type initialization and state**

Each closed, constructed type is initialized sep- arately and has its own independent set of static fields.

```
class GenericCounter<T>
{
    private static int value;
    static GenericCounter()
    {
    Console.WriteLine("Initializing counter for {0}", typeof(T));
    }
    public static void Increment()
    {
    value++; }
    public static void Display()
    {
        Console.WriteLine("Counter for {0}: {1}", typeof(T), value);
    }
}
class GenericCounterDemo
{
    static void Main()
    {
    Triggers initialization for GenericCounter<string>
    Triggers initialization for GenericCounter<int>
    GenericCounter<string>.Increment();
    GenericCounter<string>.Increment();
    GenericCounter<string>.Display();
    GenericCounter<int>.Display();
    GenericCounter<int>.Increment();
    GenericCounter<int>.Display();
    }
}
```
the static constructor is run twice: once for each closed, constructed type. If you didn’t have a static constructor, there would be fewer timing guarantees for exactly when each type would be initialized,

---
## Nullable value types

###  the problem it’s trying to solve

1. Use a reserved value to represent missing data
2. Keep a separate Boolean flag to indicate whether another field has a real value or the value should be ignored

### The Nullable<T> struct

A primitive ver- sion of `Nullable<T> `
```
public struct Nullable<T> where T : struct //allows T to be any value type except another Nullable<T>.
{
    private readonly T value;
    private readonly bool hasValue;
    public Nullable(T value)
    {
        this.value = value;
        this.hasValue = true;
    }
    public bool HasValue { get { return hasValue; } }
    public T Value
    {
    get {
            return value;
        }
    if (!hasValue)
    {
        throw new InvalidOperationException();
    }
  }
}
```

Nullable value types behave differently than non-nullable value types when it comes to boxing
-  When a value of a non-nullable value type is boxed, the result is a reference to an object of a type that’s the boxed form of the original type.
-  Nullable value types have no boxed equivalent
-  The result of boxing a value of type Nullable<T> depends on the HasValue property
    - If HasValue is false, the result is a null reference.
    - If HasValue is true, the result is a reference to an object of type “boxed T.
- you call `GetType()` on a value type value, it always needs to be boxed first
-  With nullable value types, it’ll either cause a `NullReferenceException`

```
Nullable<int> noValue = new Nullable<int>();
Console.WriteLine(noValue.GetType()); // throw NullReferenceException
```


**THE ? TYPE SUFFIX**

Add a ? to the end of the name of a non-nullable value type that’s precisely equivalent to using Nullable<T> for the same typ
```
Nullable<int> x; // == int? x;
Nullable<Int32> x // Int32? x;
```

**THE NULL LITERAL**

Two lines are equivalent:
```
int? x = new int?();
int? x = null;
```

**CONVERSIONS**
The following conversions are also available:
-  `Nullable<S> `to` Nullable<T>` (implicit or explicit, depending on the original conversion)
- `S` to `Nullable<T>` (implicit or explicit, depending on the original conversion)
-  `Nullable<S>` to `T` (always explicit)

**LIFTED OPERATORS**
>  Operators that are automatically applied to nullable value types

```
int? a = 5;
int? b = null;
int? result = a + b; // return null
```
lifted operators restrctions
1. The true and false operators are never lifted.
2. Only operators with non-nullable value types for the operands are lifted.
3. For the unary and binary operators (other than equality and relational operators), the return type of the original operator has to be a non-nullable value type.
4. For the equality and relational operators, the return type of the original operator has to be bool.
5. The `&` and `|` operators on `Nullable<bool>` have separately defined behaviors, which we’ll consider presently.


* a null value is returned if any of the operands is a null value
* two null values are consid- ered equal, and a null value and any non-null value are considered different

for nullable object


|   A   |   B   |   A && B   |   A || B   |   A ^ B   |   !A  |
|-------|-------|------------|------------|-----------|-------|
| false | false |   false    |   false    |   false   |  true |
| false | true  |   false    |   true     |   true    |  true |
| false | null  |   false    |   null     |   null    |  true |
| true  | false |   false    |   true     |   true    | false |
| true  | true  |   true     |   true     |  false    | false |
| true  | null  |   null     |   true     |   null    | false |
| null  | false |   false    |   null     |   null    |  null |
| null  | true  |   null     |   true     |   null    |  null |
| null  | null  |   null     |   null     |   null    |  null |

## THE `AS` OPERATOR AND NULLABLE VALUE TYPES
>  used for type casting and type checking

* Allows you to safely convert an object to a specific type
* check if an object is of a certain type, without throwing an exception if the conversion is not possible.
* Using the `as` operator with nullable types is surprisingly slow,it’s slower than `is` and then a cast in all the framework and compiler combinations I’ve tried.

## THE NULL-COALESCING `??` OPERATOR

`??` is a binary operator that evaluates an expression of first `??` second by going
through the following steps (roughly speaking):
1. Evaluate first.
2. If the result is non-null, that’s the result of the whole expression.
3 Otherwise, evaluate second, and use that as the result of the whole expression.

```
int? nullableInt = null;
int regularInt = nullableInt ?? 0;
Console.WriteLine(regularInt);  // Output: 0

string nullableString = null;
string regularString = nullableString ?? "default";
Console.WriteLine(regularString);  // Output: "default"
```

## Simplified delegate creation

 basic purpose ： Encapsulate a piece of code so that it can be passed around and executed in a type-safe manner, considering the return type and parameters

 ### Method group conversions

 A method group refers to one or more methods with the same name


 In c# 1
```
private void HandleButtonClick(object sender, EventArgs e)
EventHandler handler = new EventHandler(HandleButtonClick);
```

In c#2 method group conversions :a method group is implicitly convertible to any delegate type with a signature that’s compatible with one of the overloads
```
EventHandler handler = HandleButtonClick;
button.Click += HandleButtonClick;
```

### Anonymous methods

 Anonymous methods allow you to create a delegate instance without having a real method to refer to7 just by writing some code inline wherever you want to create the instance

 just use the `delegate` keyword, optionally include some parameters, and then write some code in braces
```
public void AddClickLogger(Control control, string message)
{
    control.Click += delegate(object sender, EventArgs e)
    {
        Console.WriteLine(message);
    };
}
```

### Delegate compatibility

In c#1 you needed a method with a signature with exactly the same return type and parameter types (and ref/out modifiers) to create a delegate instance
```
public delegate void Printer(string message);

public void PrintAnything(object obj) {
    Console.WriteLine(obj);
}

```

```
Printer p1 = new Printer(PrintAnything);
Printer p2 = PrintAnything;
```

## Iterators

Implementing either the generic or nongeneric interfaces manually can be tedious and error prone, so C# 2 introduced a new feature called iterators to make it simpler.

### Introduction to iterators

An iterator is a method or property implemented with an iterator block, which is in turn just a block of code using the `yield return` or `yield break` statements.


Iterator blocks can be used only to implement methods or properties with one of the following return types
- IEnumerable
- IEnumerable<T> (where T can be a type parameter or a regular type)
- IEnumerator
- IEnumerator<T> (where T can be a type parameter or a regular type)

 Each iterator has a `yield type` based on its return type. If the return type is one of the non- generic interfaces, the yield type is `object`
 
### Lazy execution

 An`IEnumerable` is a sequence that can be iterated over, whereas an `IEnumerator` is like a cursor within a sequence
 - an IEnumerable as a book and an IEnumerator as a bookmark
 - There can be multiple bookmarks within a book at any one time.
 - Moving a bookmark to the next page doesn’t change the book or any of the other bookmarks,
 

### Evaluation of yield statements
