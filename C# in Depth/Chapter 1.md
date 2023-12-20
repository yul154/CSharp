![image](https://github.com/yul154/CSharp/assets/27160394/4acf2766-699d-4423-a8d0-26f3faf26d97)

# Survival of the sharpest

Delegate Types: In C#, a delegate is a type that represents references to methods. Delegate types are used to define the signature of methods that can be encapsulated by the delegate.

Method Groups: A method group is a set of methods that have the same name but different parameter lists. Method groups are created when you refer to a method without providing arguments.

```
// Example method group
void MyMethod(int x) { /* method implementation */ }

// Method group conversion to a delegate type
Action<int> myDelegate = MyMethod;
```

Anonymous methods have the additional benefit of acting as closures: they can use local variables in the context within which they’re created

```
// Example of using an anonymous method
Action<int> printNumber = delegate(int x)
{
    Console.WriteLine($"The number is: {x}");
};

// Calling the anonymous method
printNumber(42);
```

 lambda expressions, which have almost all the benefits of anonymous methods but shorter syntax:
```
//(parameters) => expression_or_statement

// Example of using a lambda expression
Action<int> printNumber = x => Console.WriteLine($"The number is: {x}");

// Calling the lambda expression
printNumber(42);
```

LINQ LINQ enables developers to express queries using a declarative syntax similar to SQL, making it easier to interact with collections
1. Standard Query Operators:
```
var result = numbers.Where(n => n % 2 == 0).OrderByDescending(n => n).ToList();
```
2. Query Expressions:
```
var result = from n in numbers
             where n % 2 == 0
             orderby n descending
             select n;
```
3. LINQ to Objects:
```
var evenNumbers = numbers.Where(n => n % 2 == 0);

```

````
var customer = new Customer();
customer.Name = "Jon";
customer.Address = "UK";
var item1 = new OrderItem();
item1.ItemId = "abcd123";
item1.Quantity = 1;
var item2 = new OrderItem();
item2.ItemId = "fghi456";
item2.Quantity = 2;
var order = new Order();
order.OrderId = "xyz";
order.Customer = customer;
order.Items.Add(item1);
order.Items.Add(item2);


// convert to LINQ
var order = new Order
{
    OrderId = "xyz",
    Customer = new Customer { Name = "Jon", Address = "UK" },
    Items =
    {
        new OrderItem { ItemId = "abcd123", Quantity = 1 },
        new OrderItem { ItemId = "fghi456", Quantity = 2 }
    }
};

````

**METHOD AND PROPERTY DECLARATIONS**

```
private string name;
public string Name
{
    get { return name; }
    set { name = value; }
}



public string Name { get; set; }
```

expression-bodied members

```
public int Count { get { return list.Count; } }
public IEnumerator<string> GetEnumerator()
{
    return list.GetEnumerator();
}

// expression-bodied members
public int Count => list.Count;
public IEnumerator<string> GetEnumerator() => list.GetEnumerator();
```

**STRING HANDLING**

1. C# 5 introduced `caller information attributes`, including the ability for the com- piler to automatically populate method and filenames as parameter values
2. C# 6 introduced the `nameof` operator, which allows names of variables, types, methods, and other members to be represented in a refactoring-friendly form.
3. C# 6 also introduced `interpolated string literals`. This isn’t a new concept, but it makes constructing a string with dynamic values much simpler.


**Asynchrony**

* Async methods produce a result representing an asynchronous operation with no effort on the part of the developer. This result type is usually `Task` or `Task<T>`.
* Async methods use `await` expressions to consume asynchronous operations. If the method tries to await an operation that hasn’t completed yet, the method
pauses asynchronously until the operation completes and then continues.


**Balancing efficiency and complexity**
