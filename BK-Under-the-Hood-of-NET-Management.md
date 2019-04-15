### Chapter 1: Prelude ###
#### Overview ####
When a .NET application runs, four sections of memory (heaps) are created to be used for storage:
<table>
    <tr>
        <td>Code Heap</td>
        <td>Stores the actual native cod instructions after they have been Just in Time compiled (JITed)</td>
    </tr>
    <tr>
        <td>Small Object Heap (SOH)</td>
        <td>Stores allocated objects that are less than 85k in size</td>
    </tr>
    <tr>
        <td>Large Object Heap (LOH)</td>
        <td>Stores allocated objects greater than 85k (although there are some exceptions)</td>
    </tr>
    <tr>
        <td>Process Heap</td>
        <td>That's another story</td>
    </tr>
</table>

#### Heap ####
* The stack can store variables that are the primitive data types defined by .NET
<table>
    <tr>
        <td>Byte</td>
        <td>SByte</td>
        <td>Int16</td>
    </tr>
    <tr>
        <td>Int32</td>
        <td>Int64</td>
        <td>UInt16</td>
    </tr>
    <tr>
        <td>UInt32</td>
        <td>UInt64</td>
        <td>Single</td>
    </tr>
    <tr>
        <td>Double</td>
        <td>Boolean</td>
        <td>Char</td>
    </tr>
    <tr>
        <td>Decimal</td>
        <td>IntPtr</td>
        <td>UIntPtr</td>
    </tr>
    <tr>
        <td colspan="3">Structs</td>
    </tr>
</table>

* References types are stored on the heap (the SOH or LOH, depending on their size)
<table>
    <tr>
        <td>classes</td>
    </tr>
    <tr>
        <td>interfaced</td>
    </tr>
    <tr>
        <td>delegates</td>
    </tr>
    <tr>
        <td>strings</td>
    </tr>
    <tr>
        <td>instances of "object"</td>
    </tr>
</table>

* When an instance of a reference type is created (usually involving the `new` keyword), <strong>only an object reference is stored on stack</strong>. The actual instance itself is created on the heap, and its address held on stack.

Consider the following code:
```
void Method1()
{
    MyClass myObj = new MyClass();
    Console.WriteLine(myObj.Text);
}
```
![stack-heap](https://user-images.githubusercontent.com/5309726/55542949-a1549b00-56fa-11e9-9c92-fd349016a7bb.png)

.NET has to create the object on the memory heap, determine its address on the heap (or object reference), and place that object reference within the stack frame for `Method1`. As long as `Method1` is executing, the object allocated on the heap will have a reference held on the stack. When Method1 completes, the stack frame is removed (along with the object reference), leaving the object without a reference.

* The way is which variable assignment works differs between reference and value types.
```
void ValueTest()
{
    int v1 = 12;
    int v2 = 22;
    v2 = v1;
    Console.WriteLine(v2);
}
```

![stack-heap](https://user-images.githubusercontent.com/5309726/55542850-82560900-56fa-11e9-8416-c666b6b31da2.png)

There are two separate integers on the stack, both with the same value. Notice there are two stack variables, v1 and v2, and all the assignment has done is assign the same value to both variables. 

```
void RefTest()
{
    MyClass v1 = new MyClass(12);
    MyClass v2 = new MyClass(22);
    v2 = v1;
    Console.WriteLine(v2.Value);
}
```
![stack-heap](https://user-images.githubusercontent.com/5309726/55542787-5e92c300-56fa-11e9-87c4-f155b9ff5022.png)

After v1 assigned to v2:

![stack-heap](https://user-images.githubusercontent.com/5309726/55542703-330fd880-56fa-11e9-8a23-6946ce1af4a6.png)

Both object pointers are referencing only the one class instance after the assignment. Variable assignment with reference types makes the object pointers on the stack the same, and so they both point to the same object on the heap.

#### Passing parameters ####
* When you pass a value type as a parameter, all you actually pass to the calling method is <strong>a copy of the variable</strong>. Any changes that are made to the passed variable within the method call are isolated to the method.
* Having multiple copies of the same struct created on the stack creates extra work in copying the struct each time. This might not seem like a big deal but, <strong>when magnified within a high iteration loop, it can cause a performance issuse</strong>.
* One way around this problem is to <strong>pass specific value types by reference</strong>.
```
void Method1() {
    int v1 = 22;
    Method2(v1);
    Console.WriteLine("Method1 = " + v1.ToString());
}

void Method2(int v2) {
    v2 = 12;
    Console.WriteLine("Method2 = " + v2.ToString());
}

// output
Method 2 = 12
Method 1 = 22

// passed by reference
void Method1() {
    int v1 = 22;
    Method2(ref v1);
    Console.WriteLine("Method1 = " + v1.ToString());
}

void Method2(ref int v2) {
    v2 = 12;
    Console.WriteLine("Method2 = " + v2.ToString());
}

// output
Method 2 = 12
Method 1 = 12
```

#### Boxing and unboxing ####
* Allocating onto the heap requires more work, and is therefore less efficient.
```
// Integer is created on the Stack
int stackVariable = 12;

// Integer is created on the Heap = Boxing
object boxedObject = stackVariable;

// Unboxing
int unBoxed = (int)boxedObject;
```
* An integer is declared and allocated on the stack because it's a value type. It's then assgined to a new object variable(boxed) which is a reference type, and so a new object is allocated on the heap for the integer. Finally, the integer is unboxed from the heap and assigned to an integer stack variable.

```
int i = 12;
ArrayList lst = new ArrayList();

// ArrayList Add method has the following signature
// int Add(object value)

lst.Add(i); // Boxing occurs automatically
int p = (int) lst[0]; // Unboxing occurs
```
* Adding an integer (value type) to the ArrayList will cause a boxing operation to occur because, to allow the array list to be used for all types (value and reference), the `Add` method takes an object as a parameter. So, in order to add the integer to the ArrayList, a new object has to be allocated on the heap.
* In short, a lot more work is going on that is necessary, and if you were doing this in a loop with thousands of integers, then performance woruld be significanty slower.

#### More on the Heap ####
* SOH and LOH are known as managed heaps.
* .NET manages object allocations on your behalf, freeing you from having to de-allocate everything you create.

#### Garbage collection ####
* All the GC does is look for allocated objects on the heap that aren't being referenced by anything. The most obvious source of references is ths stack. Other potential sources included:
  * global / static object references
  * CPU registers
  * object finalization references
  * Interop references (.NET objects passed to COM / API calls)
  * stack references
* There are all called <strong>root references</strong> or <strong>GC roots</strong>.
* As well as root references, an object can also be referenced by other objects.
* a stack based root reference for a `Customer` containing:
  * a reference to the orders `ArrayList` collection, which contains:
    * references to `order` objects
    
![root-reference](https://user-images.githubusercontent.com/5309726/55604710-c8b37280-57a3-11e9-9183-3da579692dd4.png)

#### Inspection and collection ####
* GC simply gets a list of all root references and, for each one, moves along its reference tree "marking" each object found as being in use. Any objects not marked as being in used, or "live", are free to be "collected".
```
// Simplified GC collection in pseudo code
void Collect() {
    List gcRoots = GetAllGCRoots();
    
    foreach (objectRef root in gcRoots)
    {
        Mark(root);
    }
    
    Cleanup();
}

void Mark(objectRef o)
{
    if(!InUseList.Exists(o))
    {
        InUseList.Add(o);
        List refs = GetAllChildReferences(o);
        
        foreach(objectRef childRef in refs)
        {
            Mark(childRef);
        }
    }
}
```
* The `Mark` operation adds an object to an "object still in use" list, and then iterates through all of its child objects references, marking each on in turn.
* Once that list is complied, the GC can then go about cleaning up the heaps.

#### SOH cleanup - heap compaction ####
* When compaction occurs, <strong>marked objects</strong> are copied over the space taken up by unmarked objects, overwriting those objects, removing any gaps, and keeping the heap contiguous, this process is known as Copy Collection.
* The advantage of this is that <strong>heap fragmentation (i.g. unusable memory gaps) is kept to a minimum</strong>.
* The main disadvantage is that <strong>compaction involves copying chunks of memory around, which requireds CPU cycles and so, depending on frequency, can cause performance problems</strong>.

#### LOH sweeping - free space tracking ####
* LOH <strong>keeps track of free and used space, and attempts to allocate new objects into the most appropriately sized free slots left behind by collected objects</strong>.
* As a result of this, the LOH is prone to fragmentation, wherein memory gaps are left behind that can only be used if large objects (e.g. > 85KB) of a similar or smaller size to those gaps are subsequently allocated.

#### Static Objects ####
* <strong>Static members don't need to be created using `new` keyword, but are accessed using the name of the class they were defined within. They are accessible by all threads in an app domain</strong>, and never garbage collected because they essentially are root references in themselves.
* Any classes that subscribe to static events <strong>will remain in memory until the event subscription is removed, or the containing app domain finishes</strong>.

#### Thread Statics ####
http://putridparrot.com/blog/using-threadstatic-and-threadlocal/

### Chapter 2: The Simple Heap Model ###
```
// 19 characters
// 86,000 bytes
class MyClass
{
    string Test = "Hello world Wazzup!";
    byte[] data = new byte[86000];
}
```

![soh-loh](https://user-images.githubusercontent.com/5309726/55607828-b2131880-57af-11e9-95a6-d3ee0d69f68b.png)

#### What is automatic garbage collection? ####
* Is juat a branch of code that runs periodically, looking for allocated objects that are no longer being used by the applications.

#### When does the GC run? ####
* GC runs on a separate thread when certain memory conditions are reached or when the applcations begins to run out of memory.

#### Small Object Heap ####
* Allocation and automatic garbage collection on the SOH is quite a complex process. Because most of the objects allocated within an application are less than 85k, the SOH is a pretty heavily used storage area.

#### Consecutive allocation on the SOH ####
* When an allocation takes place,it can take time to find a suitable gap in memory to use.
* To minimize allocation time and almost eliminate heap fragmentation, .NET allocates objects consucutively, one on top of another, and keeps track of where to allocate the next object.

![next-object](https://user-images.githubusercontent.com/5309726/55705527-90b16700-5a11-11e9-9147-e459021241d6.png)

* Object A does not have a reference that can be tracked back to a root.
* There is an aroow which represents the location where the next object will be allocated, what is known as the Next Object Pointer (NOP).
* Because the NOP position is known, when a new object is allocated, it can be instantly added at that location without the addtional overhead of looking for space in Free Space table.
* To overcome the fragmentation problem, the GC compacts the heap, and thereby removes any gaps between objects.

![after-compaction](https://user-images.githubusercontent.com/5309726/55705875-63b18400-5a12-11e9-8ff3-f7ad03f9ee60.png)

#### What's still in use? ####
* When it runs, GC gets a list of all root references and, for each one, looks at every referenced object that stems from that root, and all the object contain recuisively. 
* Every object found is added to a list of "in use objects".
* Any object not on the list is assumed to be dead and available for collection, which simply involves compacting the heap by copying live objects over the top of dead ones. The result is a compacted heap with no gaps.

#### Optimizing garbage collection ####
* There's potentially a bit of a problem with creating "still in use" lists and compacting the heap, espcially if it's very large.
* Navigating through huge objects graphs and copying lots of live objects over the top of dead ones is going to take a significant amount of processing time.
* Short-lived (gen0), medium-live (gen1) and long-lived (gen0) objects.

#### Generational garbage collection ####
* Object W must have survived at least two GCs, whereas Z and Y have yet to be looked at for the first time. Object X has survived one GC inspection.

![soh-generation](https://user-images.githubusercontent.com/5309726/55788510-076e6300-5aeb-11e9-9822-b4cd913a52ed.png)

* GC runs automatically on a separate thread under one of the conditions below:
  * When the size of objects in any generation reaches a generation specific threshold. To be precise, when:
    * Gen 0 hits ~ 256K
    * Gen 1 hits ~ 2MB (at which point the GC collects Gen 1 and 0)
    * Gen 2 hits ~ 10MB (at which point the GC collects Gen 2, 1 and 0)
  * `GC.Collection()` is called in code.
  * The OS sends a low memory notification.
  
#### Gen 0 collection ####
* Two possible options when they are finally inspected:
  * Move to Gen 1 (if they have a root reference in their hierarchy)
  * Die and be compacted (if they are rootless and therefore no longer in use)
 * Either way, the result of a GC 0 is an empty Gen 0, with all rooted objects in Gen 0 being copied to Gen 1, joining the other Gen 1 objects.
  
![soh-gen-0](https://user-images.githubusercontent.com/5309726/55788938-e8bc9c00-5aeb-11e9-9c0a-56c806e1cbda.png)

#### Gen 1 collection ####
* Instead of Object X staying in Gen 1, it is moved to Gen 2. Object Y moves from Gen 0 to Gen 1 as before and Object X is collected. Once again, Gen 0 is left empty.

![soh-gen-1](https://user-images.githubusercontent.com/5309726/55789382-d4c56a00-5aec-11e9-8210-c5bdb7a02b7c.png)

#### Gen 2 collection ####
* Gen 2 collections are known as "Full" collections because all of the generations are inspected and collected. As a result, they cause the most work and are thus the most expensive.
* A full Gen 2 collection results in the following:
  * Object W dies and is compacted
  * Object X moves to Gen 2
  * Object Y moves to Gen 1
  * Object Z dies and is compacted
  
![soh-gen-2](https://user-images.githubusercontent.com/5309726/55789878-c88ddc80-5aed-11e9-906d-04f0fe0cc26e.png)

* Gen 2 is an expensive place to keep your objects, and too frequent Gen 2 collections can really hit performance.
* The general rule of thumb is that there should be ten times more Gen 0 collections than Gen 1, and ten times more Gen 1 collections than Gen 2.

#### Performance implications ####
* As Gen 0 collections are removing a lot of objects, fewer objects are being promoted, and so the performance stays high.

#### Finalization ####
* If you put a destructor or a `Finalize` method in your class, then you will actually <strong>extend the lifetime of instances of your classes for longer than you expect</strong>.
* Microsoft took a different approach, and decided to <strong>call the finalizer on objects asynchronously and on a dedicated thread</strong>.
* How do you prevent an object that needs finalization from being compacted before its finalizer is called?
  * The answer they came up with was to <strong>keep a queue of extra root references to all finalizable objects</strong> (one reference per object) and user this to keep them alive long enough to call their finalizer.
  
![finalization](https://user-images.githubusercontent.com/5309726/55852296-ee1ef280-5b8e-11e9-8854-6ee334395234.png)

* Object Z has a finalizer method one it.
* When it's created, as well as a root reference in one of the usual places (stack, statics, etc.), an additional reference is added onto the finalization queue, which then <strong>acts as a kind of reminder to .NET that it needs to call the finalizer on this object at some point</strong>.
* When Object Z loses its root reference, it would usually become a candidate for collection but, <strong>because of the extra reference on the finalization queue, it's still viewed as "rooted"</strong>, and isn't collected when the GC next runs. Instread it's promoted to Gen 2 (assuming a Gen 1 collection occurs).

![promotion-finalizable](https://user-images.githubusercontent.com/5309726/55852455-af3d6c80-5b8f-11e9-8e8b-a2b5c3f2faeb.png)

* Object Z being promoted from Gen 1 to Gen 2, the finalization reference is also moved from the finalization queue to another queue, called the fReachable queue.
* fReachable acts as a kind of reminder list of all the objects on the heap that still need to have their finalizer called. Think of it like this:
  * The finalization queue keeps a reference to all of the live finalizable objects.
  * fReachable references dead objects that need their finalizer calling.
* Periodically, the finalization thread will run, and it will iterate through all objects pointed to by references in the fReachable queue, calling the `Finalize` method or destructor on each one and removing its reference from fReachable. Only then will the finalizble object be rootless and available for collection.
* Object Z made it to Gen 2, where it could potentially have remained for a while. What we don't know is whether Object Z was actually needed for all that time, or whether its finalization was just poorly implemented.

#### Improving finalization efficiency ####
* A simply pattern you can follow to avoid the finalization problem, is by implementing the `IDisposable` interface on your class and applying the following `Dispose` pattern:
```
// Directly from code
public void Dispose()
{
    Cleanup(true);
    GC.SuppressFinalize(this);
}

private void Cleanup(bool disposing)
{
    if(!disposing)
    {
        // Thread specific code goes here
    }
}

// From the finalizer
// Resource cleanup goes here
public void Finalize()
{
    Cleanup(false);
}
```

* A Boolean parameter (`disposing`) which is used to determine if `Cleanup` was called from the finalizer or directly from code. This is important because the finalizer is called on a separate thread, so if you are doing any thread-specific cleanup, then you need to avoid doing it if it's the finalizer running it.
* `GC.SuppressFinalize(this)`, which deletes the reference in the finalization queue and gets around the problem.
* The `Finalize` method also calls the `Cleanup` method but just passes `false`, so it can avoid executing any thread specific code.

```
// Explicitly calling Dispose
FinObj myFinObj = new FinObj();
myFinObj.DoSomething();
myFinObj.Dispose();

// Implicitly calling Dispose
using (FinObj myFinObj = new FinObj()) 
{
    myFinObj.DoSomething();
}
```

#### Large Object Heap ####
* Objects large than 85KB are allocated onto the Large Object Heap (LOH).
* Objects on the LOH aren't compacted, <strong>because of the overhead of copying large chunks of memory</strong>.
* When a full Gen 2 GC takes place, <strong>the address ranges of any LOH objects not in use are recorded in a "free space" allocation table</strong>.

![free-space](https://user-images.githubusercontent.com/5309726/56125164-dbe7ee80-5faa-11e9-9915-c1aee11418f2.png)

* When a new object is allocated onto the LOH, the Free Space table is checked to see if there is an address range large enough to hold the object.
  * If there is, then an object is allocated at <strong>the start byte position and the free space entry is amended</strong>.
  * If there isn't, then the object will be allocated at the next free space above, which, in this case, is above Object D.
  
  ![next-free-space](https://user-images.githubusercontent.com/5309726/56125385-692b4300-5fab-11e9-95a8-c0721ccb3d75.png)
  
* It is very unlikely that the objects which will be allocated will be of a size that exactly matches an address range in the Free Space table. As a result, small chuncks of memory will almost always be left between objects, <strong>resulting in fragmentation</strong>.
* If the chuncks are < 85k, they will be left with no possibility of reuse, as objects are of that size obviously never make it onto the LOH. The result is that, as <strong>allocation demand increases, new segments are reserved for the LOH, even though space, albeit fragmented space, is still available</strong>.
* When a large object needs to be allocated, .NET has a choice:
  * Run a Gen2 GC and identify free space on the LOH
  * Append the object to the end (which may involve extending the heap with additional segments).
