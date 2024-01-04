# LINQ and everything that comes with it


## Automatically implemented properties

These are properties with no accessor bodies; the compiler provides the implementation. The whole of the preced- ing code can be replaced with a single line:

```
private string name;
public string Name
{
    get { return name; }
    set { name = value; }
}


public string Name { get; set; } // Automatically implemented properties

public string Name { get; private set; }
```

## Implicit typing

###  Typing terminology

**STATIC AND DYNAMIC TYPING**
C# is a statically typed language.
the binding process of determining the method signature all happens at compile time

Determining the meaning of something like a method call or field access is called binding

Languages that are dynamically typed leave all or most of the binding to execution time




