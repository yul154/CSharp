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




