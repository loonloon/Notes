#### Compilation and Execution ####
Code is compiled to a Common Intermediate Language (CIL) and stored in assemblies—files with a .dll or .exe file extension.

When an app runs, the CLR (virtual machine component) takes the assembly and uses a just-in-time compiler (JIT) to turn it into machine code that can execute on the computer.

![image](https://user-images.githubusercontent.com/5309726/126500787-61f92521-4daa-49c8-bdf7-6f8a5715abd9.png)

#### What is .NET framework? ####
it’s a virtual machine that assists with the compilation, conversion and execution of code within a development framework.

#### C# Versioning ####
<table>
    <tbody>
        <tr>
            <th>Version</th>
            <th>Features</th>
        </tr>
        <tr>
            <td>8</td>
            <td>
                <ul>
                    <li>Async Streams</li>
                    <li>Nullable reference type (Compiler warning)</li>
                    <li>Default interface implementation</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>7</td>
            <td>
                <ul>
                    <li>Numeric literals</li>
                    <li>Out variable</li>
                    <li>Pattern variable</li>
                    <li>Local method</li>
                    <li>Expression bodied</li>
                    <li>Deconstructor</li>
                    <li>Tuples</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>6</td>
            <td>
                <ul>
                    <li>Null conditions</li>
                    <li>String interpolation</li>
                    <li>Exception filter</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

#### Value and Reference Types ####
<table>
    <tbody>
        <tr>
            <th>Value</th>
            <th>Reference</th>
        </tr>
        <tr>
            <td>bool</td>
            <td>class</td>
        </tr>
        <tr>
            <td>byte</td>
            <td>interface</td>
        </tr>
        <tr>
            <td>char</td>
            <td>delegate</td>
        </tr>
        <tr>
            <td>decimal</td>
            <td>object</td>
        </tr>
        <tr>
            <td>double</td>
            <td>string</td>
        </tr>
        <tr>
            <td>enum</td>
            <td>array</td>
        </tr>
        <tr>
            <td>float</td>
            <td></td>
        </tr>
        <tr>
            <td>int</td>
            <td></td>
        </tr>
        <tr>
            <td>long</td>
            <td></td>
        </tr>
        <tr>
            <td>sbyte</td>
            <td></td>
        </tr>
        <tr>
            <td>short</td>
            <td></td>
        </tr>
        <tr>
            <td>struct</td>
            <td></td>
        </tr>
        <tr>
            <td>uint</td>
            <td></td>
        </tr>
        <tr>
            <td>ulong</td>
            <td></td>
        </tr>
        <tr>
            <td>ushort</td>
            <td></td>   
        </tr>
    </tbody>
</table>

#### What is Anonymous Type? #### 
An anonymous type is a temporary data type that is inferred based on the data that you include in an object initializer. Properties of anonymous types will be read-only properties so you cannot change their values.

#### What is Indexer? #### 
Allow instances of a class or struct to be indexed just like arrays by defined with this keyword with square bracket and parameters.

#### Basic concepts of OOPs ####
<table>
    <tbody>
        <tr>
            <td>Abstraction</td>
            <td>Present the essential features without including implementation details. (Interface)</td>
        </tr>
        <tr>
            <td>Encapsulation</td>
            <td>Information hiding, used to hide its members from outside class by using public, private, internal, protected and protected internal keywords.</td>
        </tr>
        <tr>
            <td>Inheritance</td>
            <td>Create a class from the "parent" class, extending the functionality and state of the parent in the child class.</td>
        </tr>
        <tr>
            <td>Polymorphism</td>
            <td>Classes have different functionality while sharing a common interface.</td>
        </tr>
    </tbody>
</table>

#### Retangular / Multidimentional, Jagged Array ####
* https://stackoverflow.com/questions/37717724/2d-array-vs-array-of-arrays-in-tight-loop-performance-c-sharp
<table>
    <tbody>
        <tr>
            <th>Multidimentional</th>
            <th>Jagged</th>
        </tr>
        <tr>
            <td>Slower</td>
            <td>Faster</td>
        </tr>
        <tr>
            <td></td>
            <td>Can be of different dimensions and sizes, also call as array of arrays.</td>
        </tr>
        <tr>
            <td>
                <pre lang="csharp">
const int Size = 50;
private int[,] multiArray;
<br />
void Init()
{
    this.multiArray = new int[Size, Size];
}
                </pre>
            </td>
            <td>
                <pre lang="csharp">
const int Size = 50;
private int[][] jaggedArray;
<br />
void Init()
{
    this.jaggedArray = new int[Size][];
<br />
    for(var i = 0; i < Size; i++)
    {
        this.jaggedArray[i] = new int[Size];
    }
}
                </pre>
            </td>
        </tr>
    </tbody>
</table>

![arrays](https://user-images.githubusercontent.com/5309726/52907555-dc138880-329e-11e9-9982-eb0d4fffe6cd.png)

#### What is the difference between ```System.Array.Clone()``` and ```System.Array.CopyTo()```? ####
<table>
    <tbody>
        <tr>
            <th>System.Array.Clone()</th>
            <th>System.Array.CopyTo()</th>
        </tr>
        <tr>
            <td>Used to create a new array to contain all the elements which are in the original array</td>
            <td>Used to copy the elements of one array to other</td>
        </tr>
        <tr>
            <td>Return a new array</td>
            <td>Require to have a destination array and let you specify an index (if required) to the destination array</td>
        </tr>
    </tbody>
</table>

#### What is Shallow and Deep copies? ####
<table>
    <tbody>
        <tr>
            <th>Shallow copies</th>
            <th>Deep copies</th>
        </tr>
        <tr>
            <td>Duplicate as little as possible.</td>
            <td>Duplicate everything.</td>
        </tr>
         <tr>
            <td>A shallow copy of a collection is a copy of the collection structure, not the elements. With a shallow copy, two collections now share the individual elements.</td>
            <td>A deep copy of a collection is two collections with all of the elements in the original collection duplicated.</td>
        </tr>
    </tbody>
</table>

#### What is Stack and Heap? ####
<table>
    <tbody>
        <tr>
            <th>Stack (LIFO)</th>
            <th>Heap</th>
        </tr>
        <tr>
            <td>A block of memory for storing local variables and parameters.</td>
            <td>A block of memory in which objects (reference types) reside.</td>
        </tr>
        <tr>
            <td>The stack logically grows and shrinks as a function is entered and exited.</td>
            <td>Whenever a new object is created, it is allocated on the heap, and a reference type to that object is returned.</td>
        </tr>
    </tbody>
</table>

![stack-headp](https://user-images.githubusercontent.com/5309726/52907578-475d5a80-329f-11e9-862d-886e54ecb2ef.png)

#### What is Boxing and Unboxing? ####
https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/types/boxing-and-unboxing
<table>
    <tbody>
        <tr>
            <th>Boxing</th>
            <th>Unboxing</th>
        </tr>
        <tr>
            <td>Is the process of converting a value type to the object or any interface type implemented by this value type.</td>
            <td>Extracts the value type from the object.</td>
        </tr>
        <tr>
            <td>Implicit</td>
            <td>Explicit</td>
        </tr>
    </tbody>
</table>

#### What is the difference between ```continue``` and ```break``` statement? ####
<table>
    <tbody>
        <tr>
            <th>continue</th>
            <th>break</th>
        </tr>
        <tr>
            <td>It makes the program to exit only the current iteration. It does not break the loop.</td>
            <td>It makes the program to exit the loop</td>
        </tr>
    </tbody>
</table>

#### What is difference between the ```throw``` and ```throw ex```? ####
<table>
    <tbody>
        <tr>
            <th>throw</th>
            <th>throw ex</th>
        </tr>
        <tr>
            <td>Does not reset stack trace and will get information about original exception.</td>
            <td>Will reset stack trace, so error will appear from the line where throw(ex) written. It makes hard to find the original code line number that has thrown the exception.</td>
        </tr>
    </tbody>
</table>

#### What is Extension Method? ####
Allow an existing type to be extended with new methods without altering the definition of the original type. An extension method is a static method of a static class. 

#### What is ```yield```? ####
To return each element one at a time.
 
```
public class ContactListStore : IStore<ContactModel>
{
    public IEnumerable<ContactModel> GetEnumerator()
    {
        var contacts = new List<ContactModel>();

        Console.WriteLine("ContactListStore: Creating contact 1");
        contacts.Add(new ContactModel { FirstName = "Bob", LastName = "Blue" });

        Console.WriteLine("ContactListStore: Creating contact 2");
        contacts.Add(new ContactModel { FirstName = "Jim", LastName = "Green" });

        Console.WriteLine("ContactListStore: Creating contact 3");
        contacts.Add(new ContactModel { FirstName = "Susan", LastName = "Orange" });

        return contacts;
    }
}

static void Main(string[] args)
{
    var store = new ContactListStore();
    var contacts = store.GetEnumerator();

    Console.WriteLine("Ready to iterate through the collection.");
    Console.ReadLine();
}

Console Output 
ContactListStore: Creating contact 1 
ContactListStore: Creating contact 2 
ContactListStore: Creating contact 3 
Ready to iterate through the collection.

Note: The entire collection was loaded into memory without even asking for a single item in the list
```

```
public class ContactYieldStore : IStore<ContactModel>
{
    public IEnumerable<ContactModel> GetEnumerator()
    {
        Console.WriteLine("ContactYieldStore: Creating contact 1");
        yield return new ContactModel { FirstName = "Bob", LastName = "Blue" };

        Console.WriteLine("ContactYieldStore: Creating contact 2");
        yield return new ContactModel { FirstName = "Jim", LastName = "Green" };

        Console.WriteLine("ContactYieldStore: Creating contact 3");
        yield return new ContactModel { FirstName = "Susan", LastName = "Orange" };
    }
}

static void Main(string[] args)
{
    var store = new ContactYieldStore();
    var contacts = store.GetEnumerator();

    Console.WriteLine("Ready to iterate through the collection.");
    Console.ReadLine();
}

Console Output 
Ready to iterate through the collection.
Note: The collection wasn't executed at all. This is due to the "deferred execution" nature of IEnumerable. Constructing an item will only occur when it is really required.
Let's call the collection again and obverse the behaviour when we fetch the first contact in the collection.
```

```
static void Main(string[] args)
{
    var store = new ContactYieldStore();
    var contacts = store.GetEnumerator();

    Console.WriteLine("Ready to iterate through the collection");
    Console.WriteLine("Hello {0}", contacts.First().FirstName);
    Console.ReadLine();
}

Console Output 
Ready to iterate through the collection 
ContactYieldStore: Creating contact 1 
Hello Bob

Nice! Only the first contact was constructed when the client "pulled" the item out of the collection.
```

#### Explicit Interface #### 
Implementing multiple interfaces can sometimes result in a collision between member signatures.

````
interface I1 { void Foo(); }
interface I2 { int Foo(); }

public class Widget : I1, I2
{
   public void Foo()
   {
      Console.WriteLine ("Widget's implementation of I1.Foo");
   }

   int I2.Foo()
   {
      Console.WriteLine ("Widget's implementation of I2.Foo");
      return 42;
   }
}

Widget w = new Widget();
w.Foo(); // Widget's implementation of I1.Foo
((I1)w).Foo(); // Widget's implementation of I1.Foo
((I2)w).Foo(); // Widget's implementation of I2.Foo
````

#### What is Managed and Unmanaged Code? ####
<table>
    <tbody>
        <tr>
            <th>Managed</th>
            <th>Unmanaged</th>
        </tr>
        <tr>
            <td>Any language that is written in .NET Framework is managed code and will execute only under the management of a Common Language Runtime virtual machine.</td>
            <td>The code, which is developed outside .NET framework is known as unmanaged code, such as C++.</td>
        </tr>
    </tbody>
</table>

#### What is the difference between a ```struct``` and a ```class```? ####
<table>
    <tbody>
        <tr>
            <th>struct</th>
            <th>class</th>
        </tr>
        <tr>
            <td>Is value type in C# and it inherits from System.Value Type.</td>
            <td>Is reference type in C# and it inherits from the System.Object Type.</td>
        </tr>
        <tr>
            <td>Used for smaller amounts of data.</td>
            <td>Usually used for large amounts of data.</td>
        </tr>
        <tr>
            <td>Can’t be inherited to other type.</td>
            <td>Can be inherited to other class.</td>
        </tr>
        <tr>
            <td>Can't be abstract.</td>
            <td>Can be abstract type.</td>
        </tr>
        <tr>
            <td>Do not have permission to create any default constructor.</td>
            <td>Can create a default constructor.</td>
        </tr>
    </tbody>
</table>

#### What is the difference between ```interface``` and ```abstract``` Class? ####
* An abstract class allows you to create functionality that subclasses can implement or override. An interface only allows you to define functionality, not implement it. And whereas a class can extend only one abstract class, it can take advantage of multiple interfaces.

<table>
    <tbody>
        <tr>
            <th>interface</th>
            <th>abstract (class)</th>
        </tr>
        <tr>
            <td>All the methods have to be abstract.</td>
            <td>Can have non-abstract methods.</td>
        </tr>
        <tr>
            <td>-</td>
            <td>Can declare or use any variables.</td>
        </tr>
        <tr>
            <td>-</td>
            <td>Need to use abstract keyword to declare abstract methods.</td>
        </tr>
        <tr>
            <td>Can't define constructor.</td>
            <td>Can use constructor.</td>
        </tr>
        <tr>
            <td>Can be used as multiple inheritance.</td>
            <td>Can’t be used for multiple inheritance.</td>
        </tr>
    </tbody>
</table>

#### What is the difference between override and abstract override? ####
<table>
    <tbody>
        <tr>
            <th>Virtual methods</th>
            <th>Abstract methods</th>
        </tr>
        <tr>
            <td>Usually have a default implementation, and any subclasses CAN override the method using the override modifier and provide a custom implementation.</td>
            <td>Have no actual code in them, and subclasses HAVE TO override the method.</td>
        </tr>
    </tbody>
</table>

#### Different Types of Constructor  ####
* Default constructor
* Parameterized constructor
* Copy constructor
* Static constructor
* Private constructor

#### What is the difference between ```constant``` and ```readonly```? ####
<table>
    <tbody>
        <tr>
            <th>const</th>
            <th>readonly</th>
        </tr>
        <tr>
            <td>Evaluated at compile time.</td>
            <td>Evaluated at run time.</td>
        </tr>
        <tr>
            <td>Cannot be declared as static (implicitly static).</td>
            <td>Can be initialized by code in the constructor.</td>
        </tr>
    </tbody>
</table>

#### What is the difference between ```IEnumerable``` and ```IComparable``` keywords? ####
<table>
    <tbody>
        <tr>
            <th>IEnumerable</th>
            <th>IComparable</th>
        </tr>
        <tr>
            <td>Is an interface that defines one method GetEnumerator which returns an IEnumerable interface, this in turn allows readonly access to a collection.</td>
            <td>Is implemented by types that you wish to give them the ability to compare themselves to other instances of the same type, when sorting.</td>
        </tr>
    </tbody>
</table>

#### What is the difference between ```ref```, ```out``` and ```in``` keywords? ####
<table>
    <tbody>
        <tr>
            <th>ref</th>
            <th>out</th>
            <th>in</th>
        </tr>
        <tr>
            <td colspan="3">Indicates that an argument is passed by reference</td>
        </tr>
        <tr>
            <td>Requires that the variable be initialized before it is passed.</td>
            <td>-</td>
            <td>Arguments cannot be modified by the called method.</td>
        </tr>
    </tbody>
</table>

```
//ref
void Method(ref int refArgument)
{
    refArgument = refArgument + 44;
}

int number = 1;
Method(ref number);
Console.WriteLine(number); // Output: 45

//out
int initializeInMethod;
OutArgExample(out initializeInMethod);
Console.WriteLine(initializeInMethod); // value is now 44

void OutArgExample(out int number)
{
    number = 44;
}

//in
int readonlyArgument = 44;
InArgExample(readonlyArgument);
Console.WriteLine(readonlyArgument); // value is still 44

void InArgExample(in int number)
{
   error CS8331
   //number = 19;
}
```

#### What is the difference between ```string``` and ```StringBuilder```? ####
<table>
    <tbody>
        <tr>
            <th></th>
            <th>string</th>
            <th>StringBuilder</th>
        </tr>
        <tr>
            <td>Object</td>
            <td>Immutable</td>
            <td>Mutable</td>        
        </tr>
        <tr>
            <td>Performance</td>
            <td>Is slow because its’ create a new instance to override or change the previous value.</td>
            <td>Is very fast because it will use same instance to perform any operation like insert value in existing string.</td>
        </tr>
    </tbody>
</table>

#### What is delegates and usages? ####
A delegate is an object that knows how to call a method

A delegate can be seen as a placeholder for a/some method(s). By defining a delegate, you are saying to the user of your class, "Please feel free to **assign, any method that matches this signature**, to the delegate and it will be called each time my delegate is called"

A delegate instance literally acts as a delegate for the caller: the caller invokes the delegate, and then the delegate calls the target method. This indirection decouples the caller from the target method.

#### What is multicast delegate? #### 
* Also called as combined delegate, multiple delegate objects can be assigned to one delegate instance by using the + operator (same type in return or argument). 
* The multicast delegate contains a list of the assigned delegates. When the multicast delegate is called, it invokes the delegates in the list, in order. 
* The - operator can be used to remove a component delegate from a multicast delegate.

#### What are the differences between delegates and events? ####
An Event declaration **adds a layer of abstraction and protection on the delegate instance**. This protection **prevents clients of the delegate from resetting the delegate and its invocation list** and only allows adding or removing targets from the invocation list.

* You aren't using a public property but a public field (using events, the compiler protects your fields from unwanted access)
* Events can't be assigned directly. In this case, it won't give rise to the previous error that I have showed with overriding the behavior.
* No one outside of your class can raise the event.
* Events can be included in an interface declaration, whereas a field cannot

```
//Example with Delegates (in this case, an Action - that is a kind of delegate that doesn't return a value)
public class Animal
{
    public Action Run {get; set;}

    public void RaiseEvent()
    {
        if (Run != null)
        {
            Run();
        }
    }
}

//To use the delegate, you should do something like this:
Animal animal= new Animal();
animal.Run += () => Console.WriteLine("I'm running");
animal.Run += () => Console.WriteLine("I'm still running") ;
animal.RaiseEvent();

//This code works well but you could have some weak spots.
//For example, if I write this:
animal.Run += () => Console.WriteLine("I'm running");
animal.Run += () => Console.WriteLine("I'm still running");
animal.Run = () => Console.WriteLine("I'm sleeping") ;

//with the last line of code, I have overridden the previous behaviors just with one missing + (I have used = instead of +=). 
//Another weak spot is that every class which uses your Animal class can raise RaiseEvent just calling it animal.RaiseEvent(). 

//To avoid these weak spots you can use events in c#. Your Animal class will change in this way:
public class ArgsSpecial : EventArgs
{
    public ArgsSpecial (string val)
    {
        Operation=val;
    }

    public string Operation {get; set;}
} 

public class Animal
{
    // Empty delegate. In this way you are sure that value is always != null 
    // because no one outside of the class can change it.
    public event EventHandler<ArgsSpecial> Run = delegate{} 

    public void RaiseEvent()
    {  
         Run(this, new ArgsSpecial("Run faster"));
    }
}

//to call events
Animal animal= new Animal();
animal.Run += (sender, e) => Console.WriteLine("I'm running. My value is {0}", e.Operation);
animal.RaiseEvent();
```

#### Delegate Types ####
<table>
    <tbody>
        <tr>
            <td>Action</td>
            <td>A delegate (pointer) to a method, that takes zero, one or more input parameters, but <b>does not return anything</b>.</td> 
        </tr>
        <tr>
            <td>Func</td>
            <td>A delegate (pointer) to a method, that takes zero, one or more input parameters, and <b>returns a value (or reference)</b>.</td>
        </tr>
        <tr>
            <td>Predicate</td>
            <td>Is a special kind of Func often used for comparison, and return a bool, e.g. Linq.</td>
        </tr>
    </tbody>
</table>

#### What is the difference between delegate and action / func? ####
**Simplifies** this code by instantiating the Action<T> delegate **instead of explicitly defining a new delegate** and assigning a named method to it.

![action-delegate](https://user-images.githubusercontent.com/5309726/52907978-68c24480-32a7-11e9-8db9-3e8ad32c309c.png)

#### What is the difference between dispose and finalize methods? ####
<table>
    <tbody>
        <tr>
            <th>Dispose</th>
            <th>Finalize</th>
        </tr>
        <tr>
            <td>Belongs to IDisposable interface.</td>
            <td>Belongs to System.Object class.</td>     
        </tr>
        <tr>
            <td>Called by manual user code.</td>
            <td>Called by Garbage Collector and can’t called manual by user code or any service.</td>
        </tr>
    </tbody>
</table>

####  What is the use of using statement? ####
The ```using``` block is used to obtain a resource and use it and then automatically dispose of when the execution of block completed.

#### What is the difference between Delegate.Invoke and Delegate.BeginInvoke methods? ####
<table>
    <tbody>
        <tr>
            <th>Delegate.Invoke</th>
            <th>Delegate.BeginInvoke</th>
        </tr>
        <tr>
            <td>Executes synchronously, on the same thread.</td>
            <td>Executes asynchronously, on a threadpool thread.</td>     
        </tr>
    </tbody>
</table>

#### What is the difference between ```Invoke``` and ```BeginInvoke``` methods? ####
<table>
    <tbody>
        <tr>
            <th>Control.Invoke</th>
            <th>Control.BeginInvoke</th>
        </tr>
        <tr>
            <td>Executes on the UI thread, but calling thread waits for completion before continuing.</td>
            <td>Executes on the UI thread, and calling thread doesn't wait for completion.</td>     
        </tr>
    </tbody>
</table>

#### What is ```sealed```class? ####
Restrict the inheritance feature, once a class is defined as a sealed class, the class cannot be inherited.

#### What is ```sealed```method? ####
Prevent overriding a method of a class.
    
#### What are ```partial``` classes? ####
* Create a class definition in multiple files but it will be compiled as one class at run time.
* Doesn’t allowed to create a partial class in different namespace.

```
partial class Class1
{
    public void Function1()
    {
        Console.WriteLine("Function 1");
    }
 }
 
partial class Class1
{
    public void Function2()
    {
        Console.WriteLine("Function 2");
    }
 }
 
 class Program
 {
    static void Main(string[] args)
    {
        Class1 obj = new Class1();
        obj.Function1();
        obj.Function2();
        Console.ReadLine();
    }
 }
```

#### What is difference between late binding and early binding? ####
<table>
    <tbody>
        <tr>
            <th>Late / Dynamic Binding</th>
            <th>Early / Static Binding</th>
        </tr>
        <tr>
            <td>During run time.</td>
            <td>During compile time.</td>     
        </tr>
        <tr>
            <td>Defers binding, resolving types, members, and operations from compile time to runtime.</td>
            <td>Method overloading, same methods name but different type of parameter or may be the number or parameter.</td>     
        </tr>
    </tbody>
</table>

#### What is the Constructor Chaining? #####
This is done by using the keyword ```this```. Child class constructor is mapped to parent class Constructor implicitly by base keyword so when you create an instance of child class to it’ll call parent’s class Constructor.

#### What is the difference between ```is``` and ```as``` operator? ####
<table>
    <tbody>
        <tr>
            <th>is</th>
            <th>as</th>
        </tr>
        <tr>
            <td>Checks if an object can be cast to a specific type.</td>
            <td>Attempts to cast an object to a specific type, and returns null if it fails.</td>     
        </tr>
    </tbody>
</table>

#### Different Ways of Method can be overloaded ####
<table>
    <tbody>
        <tr>
            <th>No</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>1</td>
            <td>Number of parameter can be different.</td>     
        </tr>
        <tr>
            <td>2</td>
            <td>Types of parameter can be different.</td>     
        </tr>
        <tr>
            <td>3</td>
            <td>Order of parameters can be different.</td>     
        </tr>
    </tbody>
</table>

#### What is Virtual Method? ####
Can override methods in derived classes.

#### Accessibility Modifiers ####
<table>
    <tbody>
        <tr>
            <th>Modifier</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>public</td>
            <td>No restrictions on accessing public members.</td>     
        </tr>
        <tr>
            <td>private</td>
            <td>Accessible only within the body of the class or the struct.</td>     
        </tr>
        <tr>
            <td>protected</td>
            <td>Accessible within its class and by derived class instances.</td>     
        </tr>
        <tr>
            <td>internal</td>
            <td>Accessible only within files in the same assembly.</td>     
        </tr>
    </tbody>
</table>

#### What is Serialization? #####
* Is the process of converting an object into a stream of bytes in order to store the object or transmit it to memory, a database, or a file.
* Its main purpose is to save the state of an object in order to be able to recreate it when needed.

#### What is Multithreading? ####
Enables your application to have more than one execution path at the same time.

#### What is the difference between ```string``` and ```String```? ####
Both of them are compiled to System.String in IL (Intermediate Language), so there is no difference.

<table>
    <tbody>
        <tr>
            <th>String</th>
            <th>string</th>
        </tr>
        <tr>
            <td>Stands for System.String.</td>
            <td>Is an alias in the C# language for System.String.</td>     
        </tr>
        <tr>
            <td>it is a .NET Framework type.</td>
            <td></td>     
        </tr>
    </tbody>
</table>

#### What is the difference between Xml Serializer and Data Serializer? ####
<table>
    <tbody>
        <tr>
            <th>Xml Serializer</th>
            <th>Data Serializer</th>
        </tr>
        <tr>
            <td>Everything public getter will be serialized, unless you tell not to it [XmlIgnore].</td>
            <td>Stuff mark as [DataMember] will be serialized.</td>     
        </tr>
    </tbody>
</table>

#### Difference between ```&``` and ```&&``` ####
<table>
    <tbody>
        <tr>
            <th>&</th>
            <th>&&</th>
        </tr>
        <tr>
            <td>Bitwise operator</td>
            <td>Logical operator</td>     
        </tr>
        <tr>
            <td>Both conditions are checked in every case.</td>
            <td>The first condition is evaluated and if it is false then the second condition is not checked.</td>     
        </tr>
    </tbody>
</table>

#### Difference between ```?``` and ```??``` ####
<table>
    <tbody>
        <tr>
            <th>?</th>
            <th>??</th>
        </tr>
        <tr>
            <td>Conditional operator</td>
            <td>Null-coalescing operator</td>     
        </tr>
        <tr>
            <td>-</td>
            <td>It returns the left-hand operand if the operand is not null; otherwise it returns the right hand operand.</td>     
        </tr> 
    </tbody>
</table>

#### .NET framework version ####
![dotnet-framework](https://user-images.githubusercontent.com/5309726/52908123-10407680-32aa-11e9-9583-9f29fc353e40.png)

#### Garbage Collector ####
* https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/
* Manages the allocation and release of memory for your application. 

The heap is organized into generations so it can handle short & long lived objects.
<table>
    <tbody>
        <tr>
            <th>Generation</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>0</td>
            <td>Contains short-lived objects (temporary variable) and Garbage collection occurs most frequently in this generation.</td> 
        </tr>
        <tr>
            <td>1</td>
            <td>Contains short-lived objects and serves as buffer between short-lived and long lived objects.</td>     
        </tr>
        <tr>
            <td>2</td>
            <td>Contains long-lived objects (static data) that is live for the duration of the process.</td>     
        </tr>
    </tbody>
</table>

#### Survival and Promotions ####
Objects that are not reclaimed in a garbage collection are known as survivors and are promoted to the next generation. 

Objects that survive a generation 0 garbage collection are promoted to generation 1; objects that survive a generation 1 garbage collection are promoted to generation 2; and objects that survive a generation 2 garbage collection remain in generation 2.

When the garbage collector detects that the survival rate is high in a generation, it increases the threshold of allocations for that generation, so the next collection gets a substantial size of reclaimed memory. The CLR continually balances two priorities: not letting an application's working set get too big and not letting the garbage collection take too much time.

#### Conditions for a Garbage Collection ####
* The system has low physical memory.
* The memory that is used by allocated objects on the managed heap surpasses an acceptable threshold. This threshold is continuously adjusted as the process runs.
* The System.GC.Collect method is called.

#### GC.Collect() ####
<table>
    <tbody>
        <tr>
            <th></th>
            <th>Description</th>
        </tr>
        <tr>
            <td>GC.Collect()</td>
            <td>Used to collect objects present in the generations 0, 1, 2.</td> 
        </tr>
        <tr>
            <td>GC.Collect(0)</td>
            <td>Used to collect objects present in generation 0.</td>     
        </tr>
        <tr>
            <td>GC.Collect(1)</td>
            <td>Used to collect objects present in generations 0 and 1.</td>     
        </tr>
    </tbody>
</table>

#### Drawbacks of GC.Collect() ####
* It suspends all currently executing threads. (Freezes the main and any child threads it created).
* The GC.Collect() on one application will cause a performance hit on all application in the same IIS pool.
* Expensive operation, it searches and determines whether the memory reference is pointing by any objects.

#### Weak Reference ####
Represents a weak reference, which references an object while still allowing that object to be reclaimed by garbage collection. Example usages:
* Caching (Large object)
* Event handler

#### Short vs Long Weak Reference ####
<table>
    <tbody>
        <tr>
            <th>Short Weak Reference</th>
            <th>Long Weak Reference</th>
        </tr>
        <tr>
            <td>The target of a short weak reference becomes null when the object is reclaimed by garbage collection.</td>
            <td>A long weak reference is retained after the object's Finalize method has been called. This allows the object to be recreated, but the state of the object remains unpredictable.</td> 
        </tr>
    </tbody>
</table>

#### What is Memory-Mapped Files? ####
* Efficient random access (direct access) to file data.
* Share memory between different processes on the same computer.

#### Memory-Mapped Files and Random File I/O ####
* FileStreams are ~10 times faster than memory-mapped files for sequential I/O.
* Memory-mapped files are ~10 times faster than FileStreams for random I/O.

#### What is Deadlock? ####
A deadlock is a situation where a process is not able to complete its execution because two or more processes are waiting for each other to finish.

#### What is Race Condition? ####
A race condition occurs when two threads access the same resource and are trying to change it at the same time.

#### What is the difference between Thread and Task? ####
<table>
    <tbody>
        <tr>
            <th>Thread</th>
            <th>Task</th>
        </tr>
        <tr>
            <td>There is no proper way to return a result from Thread.</td>
            <td>A Task can return a result.</td> 
        </tr>
        <tr>
            <td>Not available.</td>
            <td>Can apply chaining on multiple tasks by using ContinueWith.</td> 
        </tr>
          <tr>
            <td>Have to use event signals like AutoResetEvent and ManualResetEvent.</td>
            <td>Can wait on Tasks without using Signalling.</td> 
        </tr>
        <tr>
            <td>Not available.</td>
            <td>Can apply Parent/Child relationship in Tasks. A Task at one time becomes parent of multiple tasks. Parent Task does not complete until it's child tasks are completed.</td> 
        </tr>
        <tr>
            <td>Not available.</td>
            <td>Child Tasks can propagate their exceptions to to parent Task and all child exceptions are available in AggregateException class.</td> 
        </tr>
        <tr>
            <td>Not available.</td>
            <td>Has in-build cancellation mechanism using CancellationToken.</td> 
        </tr>
    </tbody>
</table>

#### What is the difference between AutoResetEvent and ManualResetEvent? ####
* https://stackoverflow.com/questions/153877/what-is-the-difference-between-manualresetevent-and-autoresetevent-in-net
<table>
    <tbody>
        <tr>
            <th>AutoResetEvent</th>
            <th>ManualResetEvent</th>
        </tr>
        <tr>
            <td>Unlock only one single thread from several waiting blocked threads when Set() is called.</td>
            <td>Unblocks all waiting blocked threads simultaneously when Set() is called.</td> 
        </tr>
        <tr>
            <td>Automatically</td>
            <td>Need to call Reset() method to reset the state.</td> 
        </tr>
    </tbody>
</table>

#### What is Interlocked? ####
* Provides atomic operations for variables that are shared by multiple threads.
* Atomic operations are a way to achive thread safety either by using some kind of locks like Mutexes or Semaphores which use atomic operations internally or by implementing lock free synchronization using atomics and memory fences.
* An operation is atomic if it is indivisible - in other words, nothing else can happen in the middle. So, with an atomic write, you can't have another thread reading the value half way through the write, and ending up "seeing" half of the old value and half of the new value. 
* Similarly, with an atomic read, you can't have another thread changing the value half way through the read, ending up (again) with a value which is neither the old value nor the new value.
* It depends on the underlying CPU architecture. For some CPUs, it's easy: The x86, for example, has direct support for many interlocked operations by means of the LOCK prefix in machine language.

#### What is Lock? ####
It restricts the critical region so that only one thread can enter a critical region at a time.

#### Interlocked vs Lock ####
<table>
    <tbody>
        <tr>
            <th>Interlocked</th>
            <th>Lock</th>
        </tr>
        <tr>
            <td>Good if you just care about updating the one variable.</td>
            <td>If you're doing any other work that relies on that variable's value, should use a lock block for the whole thing.
</td> 
        </tr>
    </tbody>
</table>

#### Lock vs Monitor ####
<table>
    <tbody>
        <tr>
            <th>Lock</th>
            <th>Monitor</th>
        </tr>
        <tr>
            <td>Lock is just a shortcut for Monitor statement. Compiler internally convert lock statement to Monitor.Enter and Exit statements.</td>
            <td>Provides TryEnter (useful when we need to provide timeout value), Pulse, Wait and etc methods.</td> 
        </tr>
    </tbody>
</table>

#### What is Semaphore? ####
Limits the number of threads that can access a resource or pool of resources concurrently.

```
private static readonly Semaphore semaphore = new Semaphore(5, 5);

static void Main(string[] args)
{
    Task.Run(async () =>
    {
        for (var i = 1; i <= 15; ++i)
        {
            PrintSomething(i);

            if (i % 5 == 0)
            {
                await Task.Delay(TimeSpan.FromSeconds(2));
            }
        }
    });

    Console.ReadLine();
}

public static void PrintSomething(int number)
{
    semaphore.WaitOne();
    Console.WriteLine(number);
    semaphore.Release();
}
```

#### What is Mutex? ####
A Mutex is like a lock, but it can work across multiple processes.

#### What are Concurrent Collection Classes? ####
1. ConcurrentQueue
2. ConcurrentStack
3. ConcurrentDictionary
4. ConcurrentBag

#### Lock vs ConcurrentDictionary ####
<table>
    <tbody>
        <tr>
            <th>Lock</th>
            <th>ConcurrentDictionary</th>
        </tr>
        <tr>
            <td>A store with a clerk.</td>
            <td>A vending machine.</td> 
        </tr>
    </tbody>
</table>

#### How can you make a Singleton class thread safe? ####
1. Double-Check Locking
2. Static initialization

#### What is the difference between ```IEnumerable``` and ```IQueryable```? ####
<table>
    <tbody>
        <tr>
            <th></th>
            <th>IEnumerable</th>
            <th>IQueryable</th>
        </tr>
        <tr>
            <td>Namespace</td>
            <td>System.Collections</td>
            <td>System.Linq</td> 
        </tr>
        <tr>
            <td>Derives from</td>
            <td>-</td>
            <td>IEnumerable</td> 
        </tr>
        <tr>
            <td>How does it work</td>
            <td>While querying data from database, IEnumerable executes select query on server side, load data in-memory on client side and then filter data.</td>
            <td>While querying data from database, IQueryable executes select query on server side with all filters.</td> 
        </tr>
        <tr>
            <td>When to use</td>
            <td>When querying data from in-memory collections like List, Array, etc.</td>
            <td>When querying data from out-memory (like remote database, service) collections.</td> 
        </tr>
        <tr>
            <td>Best Uses</td>
            <td>In-memory traversal.</td>
            <td>Paging</td> 
        </tr>
    </tbody>
</table>

#### What is the difference between ```Array``` and ```ArrayList```? ####
<table>
    <tbody>
        <tr>
            <th></th>
            <th>Array</th>
            <th>ArrayList</th>
        </tr>
        <tr>
            <td>Strongly/Weakly Type</td>
            <td>Strongly</td>
            <td>Weakly</td> 
        </tr>
        <tr>
            <td>Length</td>
            <td>Fixed</td>
            <td>Dynamically expanding internally.</td> 
        </tr>
        <tr>
            <td>Notes</td>
            <td>-</td>
            <td>Belongs to the days that C# didn't have generics.It's deprecated in favor of List.</td> 
        </tr>
    </tbody>
</table>

#### What is the difference between List and LinkedList? ####
<table>
    <tbody>
        <tr>
            <th>List</th>
            <th>LinkedList</th>
        </tr>
        <tr>
            <td>
                <ul>
                    <li>Basically backed by an array which is usually bigger than the current number of items. The elements are put in an array, and a new array is created when the old one runs out of space.</li>
                    <li>This is fast for access by index, but slow at removing or inserting elements within the list or at the start.</li>
                    <li>Adding/removing entries at the end of the list is reasonably cheap.</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Is a doubly-linked list - each node knows its previous entry and its next one.</li>
                    <li>This is fast for inserting after/before a particular node (or the head/tail), but slow at access by index.</li>
                </ul>
            </td> 
        </tr>
    </tbody>
</table>

#### What is the difference between Dictionary and Hashtable? ####
<table>
    <tbody>
        <tr>
            <th></th>
            <th>Dictionary</th>
            <th>Hashtable</th>
        </tr>
        <tr>
            <td>Generic Type</td>
            <td>Is generic type.</td>
            <td>Is non-generic type.</td> 
        </tr>
        <tr>
            <td>Strongly/Weakly Type</td>
            <td>Strongly, must specify the data types for key and value.</td>
            <td>Weakly, can add keys and values of any object type.</td> 
        </tr>
        <tr>
            <td>Boxing/Unboxing</td>
            <td>No</td>
            <td>Yes, values need to have boxing/unboxing. E.g. Add method arguments are object types.</td> 
        </tr>
        <tr>
            <td>Access Non Existing Key</td>
            <td>It gives runtime error.</td>
            <td>It gives null values.</td> 
        </tr>
        <tr>
            <td>Order of The Stored Values</td>
            <td>Order is maintains.</td>
            <td>Order is not maintains, rearrange the items based on their hash code.</td> 
        </tr>
        <tr>
            <td>Performance</td>
            <td>Faster</td>
            <td>Slower, values need to have boxing/unboxing.</td> 
        </tr>
    </tbody>
</table>

#### What is ISet? ####
* Implements ```IEnumerable``` and ```ICollection``` interfacs.
* Provides sets methods The HashSet<T> and SortedSet<T> classes.
    
#### What is HashSet? ####
* Provides high-performance set operation, can increase the maximum capacity to 2 billion elements on a 64-bit system by setting the enabled attribute of the ```<gcAllowVeryLargeObjects>```.
* Contains no duplicate elements, and whose elements are in no particular order.

#### What is SynchronizedCollection? ####
* It was introduced first in .NET 2.0 to provide a thread-safe collection class.
* It does this via locking so that you essentially have a ```List<T>``` where every access is wrapped in a ```lock``` statement.
