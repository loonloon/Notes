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

#### Section 4 Make your Code Faster – General Tips for .NET Developers ####
##### Section 4.1 Value Types vs. Reference Types and Reducing Pressure on the GC #####
* Local variable: stored on the stack
* Value type: completely on the stack
* Reference type: the reference itself is on the stack, but the object itself lives on the heap

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

---

#### Section 5 ####
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
