# 编程概念 (C#)

## .NET 中的程序集

程序集是为协同工作而生成的类型和资源的集合，这些类型和资源构成了一个逻辑功能单元
- 程序集采用可执行文件 (.exe) 或动态链接库文件 (.dll) 的形式，是 .NET 应用程序的构建基块
- 程序集可以包含一个或多个模块
- 由多个开发者单独开发各源代码文件或模块，最后整合所有这些内容以创建一个程序集

程序集具有以下属性
1. 程序集以 .exe 或 .dll 文件的形式实现 。
2. 可通过将程序集放入全局程序集缓存 (GAC)，在应用程序之间共享程序集。 必须先对程序集进行强命名，然后才能将它们包含到 GAC 中
3. 只有在需要使用时才会将程序集加载到内存中。
4. 可以使用反射，以编程方式获取程序集的相关信息
5. 可加载一个程序集，使用 .NET 和 .NET Framework 中的`MetadataLoadContext`类来检查该程序集


## 异步编程

> 如果需要 I/O 绑定（例如从网络请求数据、访问数据库或读取和写入到文件系统），则需要利用异步编程。 还可以使用 CPU 绑定代码（例如执行成本高昂的计算），对编写异步代码而言，这是一个不错的方案

异步编程的核心是`Task`和`Task<T>`对象，这两个对象对异步操作建模
- 对于 I/O 绑定代码，等待一个在 async 方法中返回 Task 或 Task<T> 的操作。
- 对于 CPU 绑定代码，等待一个使用 Task.Run 方法在后台线程启动的操作。
- `await`关键字有这奇妙的作用。 它控制执行`await`的方法的调用方，且它最终允许UI具有响应性或服务具有灵活性

```
private readonly HttpClient _httpClient = new HttpClient();

downloadButton.Clicked += async (o, e) =>
{
    // This line will yield control to the UI as the request
    // from the web service is happening.
    //
    // The UI thread is now free to perform other work.
    var stringData = await _httpClient.GetStringAsync(URL);
    DoSomethingWithData(stringData);
};

private DamageResult CalculateDamageDone()
{
    // Code omitted:
    //
    // Does an expensive calculation and returns
    // the result of that calculation.
}

calculateButton.Clicked += async (o, e) =>
{
    // This line will yield control to the UI while CalculateDamageDone()
    // performs its work. The UI thread is free to perform other work.
    var damageResult = await Task.Run(() => CalculateDamageDone());
    DisplayDamage(damageResult);
};
```

### 内部原理
> 在 C# 方面，编译器将代码转换为状态机，它将跟踪类似以下内容：到达 await 时暂停执行以及后台作业完成时继续执行。

#### 识别 CPU 绑定和 I/O 绑定工作

* 如果会“等待”某些内容，例如数据库中的数据： I/O
  * 如果你的工作为 I/O 绑定，请使用 async 和 await（而不使用 Task.Run）。 不应使用任务并行库
* 要执行开销巨大的计算：CPU
  *  如果你的工作属于 CPU 绑定，并且你重视响应能力，请使用 async 和 await，但在另一个线程上使用 Task.Run 生成工作

如果你的工作为 I/O 绑定，请使用 async 和 await（而不使用 Task.Run）。 不应使用任务并行库

* `async`方法需要在主体中有`await`关键字，否则它们将永不暂停！
* 添加“Async”作为编写的每个异步方法名称的后缀
* `async void`应仅用于事件处理程序。async void 是允许异步事件处理程序工作的唯一方法
* LINQ 中的 Lambda 表达式使用延迟执行，这意味着代码可能在你并不希望结束的时候停止执行
* 采用非阻止方式编写等待任务的代码

|使用以下方式|而不是|若要执行此操作|
|------------|-----|-----------|
|await	Task.Wait|Task.Result |检索后台任务的结果|
|await Task.WhenAny|Task.WaitAny|等待任何任务完成|
|await Task.WhenAll|	Task.WaitAll|等待所有任务完成|
|await Task.Delay|Thread.Sleep|等待一段时间|




#### 等待多个任务完成

Task API 包含两种方法（即`Task.WhenAll`和`Task.WhenAny`）执行非阻止等待的异步代码

```
public async Task<User> GetUserAsync(int userId)
{
    // Code omitted:
    //
    // Given a user Id {userId}, retrieves a User object corresponding
    // to the entry in the database with {userId} as its Id.
}

public static async Task<IEnumerable<User>> GetUsersAsync(IEnumerable<int> userIds)
{
    var getUserTasks = new List<Task<User>>();
    foreach (int userId in userIds)
    {
        getUserTasks.Add(GetUserAsync(userId));
    }

    return await Task.WhenAll(getUserTasks);
}

public static async Task<User[]> GetUsersAsync(IEnumerable<int> userIds)
{
    var getUserTasks = userIds.Select(id => GetUserAsync(id)).ToArray();
    return await Task.WhenAll(getUserTasks);
}
```
---
# Collections

## Simple Collection

```
var salmons = new List<string>();
salmons.Add("chinook");

var salmon = new List<string>{"",""};

salmons.Remove("coho");

foreach (var salmon in salmons)
{
    Console.Write(salmon + " ");
}

for(var index = 0; index < salmon.Count; index++){
  
}

// Collections of object
var theGalaxies = new List<Galaxy>{
  new Galaxy() { Name="Tadpole", MegaLightYears=400},
  new Galaxy() { Name="Pinwheel", MegaLightYears=25},
}

foreach (Galaxy theGalaxy in theGalaxies){
}
```

从一个泛型列表中删除元素。 使用以降序进行循环访问的 for 语句，而非 foreach 语句。 这是因为 RemoveAt 方法将导致已移除的元素后的元素的索引值减小

```
var numbers = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };

// Remove odd numbers.
for (var index = numbers.Count - 1; index >= 0; index--)
{
    if (numbers[index] % 2 == 1)
    {
        // Remove the element by specifying
        // the zero-based index in the list.
        numbers.RemoveAt(index);
    }
}

// Iterate through the list.
// A lambda expression is placed in the ForEach method
// of the List(T) object.
numbers.ForEach(
    number => Console.Write(number + " "));
// Output: 0 2 4 6 8
```

## 集合的类型

### `System.Collections.Generic` 泛型集合

`System.Collections.Generic`命名空间中的一些常用类
* `Dictionary<TKey,TValue>`	表示基于键进行组织的键/值对的集合。
* `List<T>`	表示可按索引访问的对象的列表。 提供用于对列表进行搜索、排序和修改的方法。
* `Queue<T>`	表示对象的先进先出 (FIFO) 集合。
* `SortedList<TKey,TValue>`	表示基于相关的 IComparer<T> 实现按键进行排序的键/值对的集合。
* `Stack<T>`	表示对象的后进先出 (LIFO) 集合。

### System.Collections.Concurrent 多个线程访问集合项
* `BlockingCollection<T>`: 为实现 IProducerConsumerCollection<T> 的线程安全集合提供阻塞和限制功能。
* `ConcurrentDictionary<TKey,TValue>`、
* `ConcurrentQueue<T>`
* `ConcurrentStack<T>`
* `ConcurrentBag<T>` : 表示对象的线程安全的无序集合。
* `OrderablePartitioner<TSource>`: 表示将可排序数据源拆分为多个分区的特定方式。
* `Partitioner`: 提供针对数组、列表和可枚举项的常见分区策略。
* `Partitioner<TSource>`: 表示将数据源拆分为多个分区的特定方式。

### `System.Collections` 
> 作为`Object`类型的对象存储。

* `ArrayList`	: 表示对象的数组，这些对象的大小会根据需要动态增加。
* `Hashtable`	: 表示根据键的哈希代码进行组织的键/值对的集合。
* `Queue`	: 表示对象的先进先出 (FIFO) 集合。
* `Stack`	: 表示对象的后进先出 (LIFO) 集合。

### 使用 LINQ 访问集合
> 可以使用 LINQ（语言集成查询）来访问集合。 LINQ 查询提供筛选、排序和分组功能

```
List<Element> elements = BuildList();

// LINQ Query.
var subset = from theElement in elements
             where theElement.AtomicNumber < 22
             orderby theElement.Name
             select theElement;

foreach (Element theElement in subset)
{
    Console.WriteLine(theElement.Name + " " + theElement.AtomicNumber);
}

```

### 对集合排序
> 实现`IComparable<T>`接口，此操作需要实现`CompareTo`方法

每次对`CompareTo`方法的调用均会执行用于排序的单一比较。 
- `CompareTo`方法中用户编写的代码针对当前对象与另一个对象的每个比较返回一个值
- 当前对象小于另一个对象，则返回的值小于零

```
public int CompareTo(Car other)
    {
        // A call to this method makes a single comparison that is
        // used for sorting.

        // Determine the relative order of the objects being compared.
        // Sort by color alphabetically, and then by speed in
        // descending order.

        // Compare the colors.
        int compare;
        compare = String.Compare(this.Color, other.Color, true);

        // If the colors are the same, compare the speeds.
        if (compare == 0)
        {
            compare = this.Speed.CompareTo(other.Speed);

            // Use descending order for speed.
            compare = -compare;
        }

        return compare;
    }
```

### 定义自定义集合
> 可以通过实现`IEnumerable<T>`或`IEnumerable`接口来定义集合

```
Color[] _colors =
    {
        new Color() { Name = "red" },
        new Color() { Name = "blue" },
        new Color() { Name = "green" }
    };

    public System.Collections.IEnumerator GetEnumerator()
    {
        return new ColorEnumerator(_colors);

        // Instead of creating a custom enumerator, you could
        // use the GetEnumerator of the array.
        //return _colors.GetEnumerator();
    }

    // Custom enumerator.
    private class ColorEnumerator : System.Collections.IEnumerator
    {
        private Color[] _colors;
        private int _position = -1;

        public ColorEnumerator(Color[] colors)
        {
            _colors = colors;
        }

        object System.Collections.IEnumerator.Current
        {
            get
            {
                return _colors[_position];
            }
        }

        bool System.Collections.IEnumerator.MoveNext()
        {
            _position++;
            return (_position < _colors.Length);
        }

        void System.Collections.IEnumerator.Reset()
        {
            _position = -1;
        }
    }
```

### 迭代器
> 迭代器用于对集合执行自定义迭代。 迭代器可以是一种方法，或是一个 get 访问器

通过使用 foreach 语句调用迭代器。 foreach 循环的每次迭代都会调用迭代器
```
private static IEnumerable<int> EvenSequence(
    int firstNumber, int lastNumber)
{
    // Yield even numbers in the range.
    for (var number = firstNumber; number <= lastNumber; number++)
    {
        if (number % 2 == 0)
        {
            yield return number;
        }
    }
}
```
