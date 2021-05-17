#### Section 1 ####
##### Section 1.2 .NET Core History - Performance #####

![image](https://user-images.githubusercontent.com/5309726/107108457-e73c9600-6872-11eb-8c41-905d86558a65.png)

##### Section 1.3 .NET Core Concepts and Definitions #####
###### Why .NET Standard? ######
* Code sharing
* .NET Standard 2.0 doubles the APIs than 1.0
  * Deployment Options
    * Self Contained
    * Shared Framework

---

#### Section 2 ####
##### Section 2.1 Measuring CPU #####
###### Types of profiles ######
* Sampling
* Instrumentation

<table>
  <tr>
      <th>Sampling</th>
  </tr>
  <tr>
      <td>Huge number of snapshots with callstacks (Aggrerates this data)</td>
  </tr>
  <tr>
    <td>Can be done on a running process without restart</td>
  </tr>
  <tr>
    <td>Minimal overhead</td>
  </tr>
  <tr>
    <td>Non CPU work is not visible (E.g. Thread.Sleep(), I/O operation)</td>
  </tr>
</table>
 
![image](https://user-images.githubusercontent.com/5309726/109120781-59710e00-7781-11eb-86f1-0ed294b4fb40.png)

![image](https://user-images.githubusercontent.com/5309726/109122806-1b291e00-7784-11eb-84f7-7373323a2fc2.png)

<table>
    <tr>
        <th>Instrumentation</th>
    </tr>
    <tr>
        <td>Injects code to measure methods</td>
    </tr>
    <tr>
      <td>Measures also non CPU work</td>
    </tr>
    <tr>
      <td>Can pontentiallu have more overhead</td>
    </tr>
    <tr>
      <td>No attach / detach</td>
    </tr>
</table>

![image](https://user-images.githubusercontent.com/5309726/109122903-385dec80-7784-11eb-9896-961da3218b66.png)

##### Section 2.2 Measuring Memory #####
* Utilization: Heap size, Physical memory size
* Number of objects
* Typical problems:
  * Too much work by the GC (hurts CPU)
  * Not enough work the GC (eventually out of memory)
 
###### Which GC root holds the reference to the objects? ######
* We have to search for GC roots. Allocation stcak does not help
* List all objects sorted -ones taking up the most memory are on the top
![image](https://user-images.githubusercontent.com/5309726/109128209-3bf47200-778a-11eb-970e-001055141496.png)
* Track references to GC Roots
![image](https://user-images.githubusercontent.com/5309726/109128305-53335f80-778a-11eb-9d1e-3df3439f9835.png)

###### GC Settings ######
* Server
* Concurrent

---

#### Section 3 Measure Performance During Development Part 2 ####
##### Section 3.1 Event Tracing: ETW and PerfView #####
* Event Tracing for Windows (ETW)
  * High frequency logging framework built into Windows to diagnose performance issues
  * Designed for huge number of messages
  * Windows ONLY
  * `System.Diagnostics.Tracing`

* PerfView
  * Free and just a small executable
  * Collects and visualizes ETW events

<table>
    <tr>
        <th>SOH</th>
        <th>LOH</th>
    </tr>
    <tr>
        <td>Small Object Heap, smaller than 85,000 bytes</td>
        <td>Large Object Heap, bigger than 85,000 bytes</td>
    </tr>
</table>

<table>
    <tr>
        <th>Gen 0</th>
        <td>Collected very frequently</td>
    </tr>
     <tr>
        <th>Gen 1</th>
        <td>Collected less frequently</td>
    </tr>
     <tr>
        <th>Gen 2</th>
        <td>Collected rarely (SLOW, because FULL GC (Gen 0, 1, 2)</td>
    </tr>
</table>

---

#### Section 4 Make your Code Faster – General Tips for .NET Developers ####
##### Section 4.1 Value Types vs. Reference Types and Reducing Pressure on the GC #####
<table>
    <tr>
        <th>Local variable</th>
        <td>Stored on the stack</td>
    </tr>
    <tr>
        <th>Value type</th>
        <td>Completely on the stack</td>
    </tr>
    <tr>
        <th>Reference type</th>
        <td>The reference itself is on the stack, but the object itself lives on the heap</td>
    </tr>
</table>

![image](https://user-images.githubusercontent.com/5309726/116373449-edbe2680-a83f-11eb-89b9-e0a0bfb82a7a.png)

* Instance variables (class property)

![image](https://user-images.githubusercontent.com/5309726/116374312-c1ef7080-a840-11eb-9dbb-806673da621e.png)

* Object header in Reference types
  * Per object overhead
  * Benefits for Polymorphism, Inheritance and locking

![image](https://user-images.githubusercontent.com/5309726/116374769-30ccc980-a841-11eb-84f9-6eaa3c9e0cc8.png)

##### Section 4.2 #####
* Implement `IEqualable<T>` interface for Value Type and override the `Equals` function (Avoid boxing)
* Value type can be allocated on the stack

##### Section 4.3 Saving Threads with async/await #####
In both cases threads and calculations can be started either in a synchronous or in an asynchoronous way

<table>
    <tr>
        <th>Multi Threading (Thread)</th>
        <th>Parallel Computing (Process)</th>
    </tr>
    <tr>
        <td>Multiple execution contexts exist in the application, but doesn't say anything about synchronicity</td>
        <td>Multiple calculations are carried out simultaneously</td>
    </tr>
</table>

* By using the `await` keyword the thread which starts the operation does not sit and wait until the result is there
* In ASP.NET core applications: with parallel requests you can save threads from the threadpool
* In GUI applications: no heavy work on the UI thread

##### Section 4.5 Choosing the Right Collection #####

---

#### Section 5 C# 7 Performance ####
##### Section 5.2 Avoid Heap Allocations with Local Functions #####
* Use local function instead of delegates
  * Reduce pressure on GC
  * It uses the "Call" IL instruction instead of "CallVirt"

##### Section 5.3 Make ValueTypes Faster with ref return #####
* New storage created when passing parameter into method

```
public static void Increment(int number)
{
    number++;
}

static void Main(string[] args)
{
    int i = 0;
    Increment(i);
    Console.WriteLine(i);
}

```

* ref method parameter (Before C# 7)
```
public static void Increment(ref MyBigStruct s)
{
    s.Value1++;
    s.Value2++;
}

static void Main(string[] args)
{
    MyBigStruct s;
    Increment(ref s);
}
```

* ref return (After C# 7)
* Indeal for unsafe code (returning array item)
* Improve performance (returning referenace to a big ValueType)
```
public struct Matrix
{
    int item00, item01, item02;
    int item10, item11, item12;
    ...
}

public static ref Matrix SelectMatrix(ref Matrix left, ref Matrix right, Func<bool> selectionLogic)
{
    return selectionLogic() ? ref left : ref right;
}
```

* ref local (After C# 7)
```
int x = 1;
ref int y = ref x;
y = 42;

// output: 42
Console.WriteLine($"Value of x: {x}");

```

##### Section 5.4 The Performance Characteristics of the C# 7 Tuple Feature #####
* System.Type is a reference type (Before C# 7) 

```
public static Tuple<int, int> GetMinMax(List<int> values)
{
    ...
    return Tuple.Create<int, int>(min, max);
}
```

* System.ValueType is a value type (C# 7)

```
public static (int min, int max) GetMinMax(List<int> values)
{
    ...
    return (min, max);
}
```

##### Section 5.5 Optimization of async-methods with ValueTask #####
* By default, still use `Task` and `Task<T>`
* `Task.WhenAll` and similar methods may have to convert `ValueTask<T>` to `Task<T>` (WARNING)
* Use it if you have numbers showing that `ValueTask` (struct) is faster than `Task` (heap allocation)

##### Section 5.6 Pattern matching and Performance #####
* `is pattern` is better than `is - as`

---

#### Section 6 New Performance Related APIs ####
##### Section 6.1 Reusing Arrays with `ArrayPool<T>` #####
* Prevent LOH and GC Gen2 collection
* Threadsafe
* Default size in the shared pool: 2 ^ 20
* Calling `Rent(n)` where n > 2 ^ 20, NO POOLING
* Use `ArrayPool<T>.Create(maxArrayLength, maxArraysPerBucket)` where size bigger than 2 ^ 20

```
public void MethodCalledOften()
{
    var arrayPool = new ArrayPool<int>.Shared.Rent(256 * 1024);
    Console.WriteLine(arrayPool[0].ToString());
    ArrayPool<int>.Shared.Return(arr);
}
```

##### Section 6.2 Accessing all Types of Memory Safely and Efficiently with `Span<T>` #####
* Can avoid creating different function parameters
* `ReadOnlySpan` can be used in tring slicing to improve performance

```
//Before
//Managed memory
byte[] myArray = new byte[100];

//Unmanaged memory
IntPtr myArray = Marshal.AllocHGlobal(100);
...
Marshal.FreeHGlobal(myArray);

//Stack memory
byte* myArray = new stackalloc byte[100]

//After
//Managed memory
byte[] myArray = new byte[100];
var myArraySpan = new Span<byte>(myArray);

//Unmanaged memory
IntPtr myArray = Marshal.AllocHGlobal(100);
Span<byte> nativeSpan;

unsafe
{
    nativeSpan = new Span<byte>(myArray.ToPointer(), 100);
}

...

Marshal.FreeHGlobal(myArray);

//Stack memory
Span<byte> stackSpan;

unsafe
{
    byte* myArray = new stackalloc byte[100]
    nativeSpan = new Span<byte>(myArray, 100);
}
```

---

#### Section 7 Data Access Performance: Entity Framework Core ####
* Use EF mini profiler

##### Section 7.1 Make faster Queries with Entity Framework Core #####
* Limit the number of columns and rows
* Give as much into to the EF context (Do queries on the DB), not in application
* Consider using async queries:
  * `ToListAsync()`
  * `ToArrayAsync()`
  * `SingleAsync()`

```
//Bad
var d1980 = new DateTime(1980, 1, 1);
var dbUsers = SampleDataContext.Users;

foreach (var item in dbUsers)
{
    if (item.BirthDate < d1980)
    {
        users.Add(item.UserName);
    }
}

//Good
var d1980 = new DateTime(1980, 1, 1);
var users = SampleDataContext.Users.Where(n => x.BirthDate < d1980)
            .Select(n => x.UserName).Skip(pageNumber * 10).Take(10).ToListAsync();
```

##### Section 7.2 Loading Dependent Entities Efficiently #####
* Eager Loading
* Explicit Loading
* Lazy Loading (EF Core 2.0)

```
//Bad
var d1980 = new DateTime(1980, 1, 1);
var users = SampleDataContext.Users.Where(n => x.BirthDate < d1980)
            .Select(n => x.UserName).Skip(pageNumber * 10).Take(10).ToListAsync();

foreach (var item in users)
{
    //sending more than 1 SQL query
    var comments = SampleDataContext.Comments
                   .Where(n => n.Author.UserName == item)
                   .Select(n => n.Text).ToList();

    usersWithComments.Add(new UserWithComments
    {
        UserName = item,
        Comments = comments
    });
}

//Good (Eager loading)
//All users with comment
var d1980 = new DateTime(1980, 1, 1);
var users = SampleDataContext.Users.Include(n => n.Comments)
            .Where(n => x.BirthDate < d1980).Skip(pageNumber * 10).Take(10)
            .SelectMany(n => n.Comments).Select(n => new { n.Author.UserName, n.Text })
            .GroupBy(n => n.UserName).ToListAsync();
            
foreach (var item in users)
{
    usersWithComments.Add(new UserWithComments
    {
        UserName = item.Key,
        Comments = item.Select(n => n.Text).ToList()
    });
}

//Good (Explicit loading)
//Only first user with comment
var d1980 = new DateTime(1980, 1, 1);
var users = SampleDataContext.Users.Where(n => x.BirthDate < d1980).Skip(pageNumber * 10).Take(10)
            .ToListAsync();
            
SampleDataContext.Entry(users.First()).Collection(n => n.Comments).Load();

usersWithComments.Add(new UserWithComments
{
    UserName = users.First().UserName
    Comments = users.First().Comments.Select(n => n.Text).ToList()
});
    
for (var i = 1; i < 10; i++)
{
    usersWithComments.Add(new UserWithComments
    {
        UserName = users.ElementAt(i).UserName
    });
}
```

##### Section 7.2 EF Core Performance: Maximum Length, Client Side Execution, Change Tracking #####
* Maximum Length
  * If not specified, it will use the maximum length during table creation
  
```
//Option 1
public class UserData
{
    public int Id { get; set; }

    [MaxLength(250)]
    public string FirstName { get; set; }

    [MaxLength(250)]
    public string LastName { get; set; }
}

//Option 2
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<UserData>().Property(b => b.UserName).HasMaxLength(350);
}
```

* Client side evalution

```
//issue, will get all the records from DB without filter
public async Task<IActionResul> CommentsWithDotNetCore()
{
    var comments = await SampleDataContext.Comments
        .Where(x => ContainsDotNetCore(x.Text))
        .Select(x => x.Text)
        .ToListAsync();
    return View(comments);
}

public bool ContainsDotNetCore(string text)
{
    return text.ToLower().Contains(".net core");
}

//solution, to notify and refactor the problem in the code
public override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder.ConfigureWarning(warnings => warnings.Throw(RelationalEventId.QueryClientEvalutionWarning));
}
```

* Change Tracking
  * Turn it off when it does not required
  
```
//Option 1 per query
var comment = await SampleDataContext.Comments
    .Where(x => x.Id == 123)
    .AsNoTracking()
    .SingleAsync();
    
//Option 2 global on the data context
using (var dbContext = new SampleDataContext())
{
    dbContext.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;
    var comment = await SampleDataContext.Comments
        .Where(x => x.Id == 123).SingleAsync();
}
```

---

#### Section 8 .NET Core Specific Performance Tools and Configurations ####
* CrossGen
* Mono Linker
* ASP.NET Core precompiled views

##### Section 8.1 Pre-JIT .NET Core Applications with CrossGen #####
* Ahead of time (AOT compilation)
  * ngen.exe
  * CrossGen

* CrossGen
  * Generate native code, no JIT at runtime

* Use PerfView to collect CPU Sampling

##### Section 8.2 Make your .NET Core Application Smaller with Mono’s Linker #####
* Reduce the size of applications by removing unused IL code
* Smaller deployment package (Faster deployment, e.g. microservices)
* ONLY WORKS with self contained applications

##### Section 8.3 Faster Startup with ASP.NET Core Precompiled Views #####
* Performance boost for the first request by compiling the Views when you publish the application instead of compiling them at runtime
* [ApplicationName].PrecompiledViews.dll

---

#### Section 9: Performance Monitoring in Production for .NET Core ####
* Application Insights
* Dynatrace

##### Section 9.1 Enabling Application Insights for ASP.NET Core Applications #####
* The analyze data store in Azure

##### Section 9.2 Tracking Slow Requests and Performance Testing with Application Insights #####
* Total of Server Requests by Request Performance
* Performance Testing
 
##### Section 9.3 Tracking Custom Dependencies with Application Insights #####
* `Microsoft.ApplicationInsights.TelemetryClient` class
  * `TrackPageView`
  * `TrackEvent`
  * etc

##### Section 9.4 Monitoring .NET Core Applications with Dynatrace #####

---
