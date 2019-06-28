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
        <td>instances of "class" (object)</td>
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
* Short-lived (gen0), medium-live (gen1) and long-lived (gen2) objects.

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

### Chapter 3: A Little More Detail ###
* Often, when objects are created, only some of their member variables are created immediately, with others only instantiating much later. This means that an object can contain references to objects from younger generations, for example a Gen 2 object referencing a gen 0 one.

```
class LivesInGen2ForAges
{
    private SpannerInWorks _createLater;
    
    public void DoABitMoreWork()
    {
        _createdLater = new SpannerInWorks();
    }
}
```

#### The card table ####
* A data structure called the card table is used to record when objects are created and referenced from older objects. It's specifically designed to maintain GC performance but still allow objects references to be tracked regardless of their generation.
* When Gen 0 or Gen 1 objects are created from within Gen 2 objects, the execution engine puts an entry in the card table. This check is made before any reference is created, to see if that reference is to a "previous generation" object, and is known as the "write barrier".
* .NET stores a bit pattern, with one bit representing 128 bytes of heap memory. If an object residing within the address range of that bit has created a Gen 0 or Gen 1 object, then the bit is set.
* When a Gen 0 and Gen 1 collection takes place, any objects residing in the address range of a set bit in the card table are included in the "in use list" inspection, as are all of their child references.

![card-table](https://user-images.githubusercontent.com/5309726/56352050-03d08f80-6201-11e9-8671-b60cdc36cff8.png)

* Object B instantiates a reference to Object E, and marks the byte portion in the card table in which it itself resides. The next time a Gen 0 collection takes place, all child references for objects located within the pseudo example byte range 0 - 127 held in the flagged region of the card table will be inspected.

#### A Bit About Segments ####
* Each managed process has its own heaps for both the SOH and the LOH. Initially, at the start of a process execution, two memory segments are requested from the OS, one for the SOH and one for the LOH.
* Segments are just units of memory allocation. When the GC needs to expand the heap, it requests an additional segment. Equally, when a heap has compacted, or realizes it has excess free space, it can release segments back to the OS.
* Segment size is dynamic, is tuned by the runtime, and depends on the GC mode you are running, either Workstation or Server GC.
* At the start of a process's execution, the SOH is allocated a single segment, called the ephemeral segment, to be used for Gen 0 allocations and promotions to Gen 1 and Gen 2.
* When it's full, a GC takes place and a new segment is added. Any surviving Gen 0 objects are copied to the new segment (becoming Gen 1 objects), and the old segment is left with just Gen 2 objects.

![ephemeral-segment](https://user-images.githubusercontent.com/5309726/56353851-eef5fb00-6204-11e9-9af5-f280bb141d8e.png)

#### Segments and Pages ####
* Segments are made up from virtual memory pages requested from the Virtual Memory Manager (VMM) of the OS. The VMM maps the physical memory and the disk-based page file to a single virtual addressable space, which is subdivided into pages.
* Pages can be in one of three states: free, committed or reserved.
<table>
    <tr>
        <th>State</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>Free pages</td>
        <td>are available to be allocated to a requesting thread.</td>
    </tr>
    <tr>
        <td>Committed pages</td>
        <td>are allocated and, ultimately, translate to pages in physical memory. As such, they can be swapped to and from the page file when necessary (paging).</td>
    </tr>
    <tr>
        <td>Reserved pages</td>
        <td>are a low overhead means of reserving virtual memory for future use, but don't translate to physical memory and don't incur paging.</td>
    </tr>
</table>
* When the GC builds a segment, it reserves the required number of pages and then commits them when needed. Redundant segments can ultimately be released back to the OS.

#### Garbage Collection Performance ####
* Heap compaction, Objects are being moved around in memory so, to ensure heap integrity, the GC has to suspend the execution of threads of the executing process.
* The GC has two modes: 
<table>
    <tr>
        <th>Mode</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>Workstation mode</td>
        <td>which is tuned to give maximum UI responsiveness.</td>
    </tr>
    <tr>
        <td>Server mode</td>
        <td>which is tuned to give maximum request throughput.</td>
    </tr>
</table>
  
#### Workstation GC mode ####
* This mode is designed to give maximum possible responsiveness to the user, and cut down on any pauses due to GC.
  * Ideally, you want to <strong>avoid any perception of pauses or jerkiness in interactive applications so, to achieve this responsiveness</strong>, Workstation GC mode limits the number of thread suspensions.
* Workstation GC could run as either concurrent or non-concurrent; this simply refers to which thread the GC runs on. 
<table>
    <tr>
        <th>concurrent</th>
        <th>non-concurrent mode</th>
    </tr>
    <tr>
        <td>As multicore/multiprocessor desktop machines are now very common, <strong>concurrent Workstation GC</strong> is now the norm and the default.</td>
        <td>Thread execution of the application code is suspended, and the GC then runs on the application thread. It was designed for uni-processor machines, where running threads concurrently wasn't an option.</td>
    </tr>
</table>

#### Concurrent Workstation GC ####
* Concurrent GC has a <strong>separate thread for the GC to run on</strong>, meaning that the application can continue execution while the GC runs.
* <strong>Concurrent GC only applies to full GCs, so Gen 0 and Gen 1 GCs still cause thread suspension</strong>.

![concurrent-gc](https://user-images.githubusercontent.com/5309726/56450676-45288280-635a-11e9-83ec-a45426d1e725.png)

* The process whereby the GC domain is initially determined with a start and end position, and then the GC runs (bear in mind that it doesn't show the results of the collection). We can also see that while the GC is running, the application allocates additional objects to the heap (L, M, N and O).
* The application can continue to allocate objects right up until the ephemeral segment limit is reached, which is the size of the segment minus a bit of space that we will call the "No Go Zone." Once this limit is reached, application execution is suspended until the full GC is finished.
* Object M has become rootless, but it is outside of the current GC domain so its space can't be collected. It now contributes to the ephemeral segment usage even though it's no longer being used, and it's going to make thread suspension more likely to happen.

#### Background Workstation GC mode (.NET 4.0) ####
* With background GC, a Gen 0 or Gen 1 GC can be triggered for the newly allocated objects while a full Gen 2 GC is in progress.

#### Server GC mode ####
* Designed to give maximum throughput, scalability and performance for server environments. It achieves this by making the GC multithreaded, with <strong>multiple threads running the GC in parallel on multiprocessors/cores</strong>.
* For each process, it allocates a separate SOH and LOH per logical processor (a processor with four cores has four logical processors).

#### Configuring the GC ####
* All you need to do to configure the GC is alter your config file (ASP.NET applications use aspnet.config for your framework version).
  * To enable Server GC, just set `gcServer="true"` in the runtime section. Obviously, for Workstation, set it to `false`.
  ```
  <configuration>
    <runtime>
        <gcServer enabled="true | false"/>
    </runtime>
  </configuration>
  ```
  * if you have configured Workstation GC, then you can switch on concurrency by setting the `gcConcurrent enabled` flag to `true`.
  ```
  <configuration>
    <runtime>
    <gcConcurrent enabled="true | false"/>
    </runtime>
  </configuration>
  ```

#### Runtime GC Latency Control ####
* .NET Framework 3.5.1 allows the latency of the GC to be controlled programmatically, which ultimately overrides the gcConcurrent setting within your config file.
* To achieve this, the System.Runtime.GCSettings.LatencyMode property can be set to one of three modes.
<table>
    <tr>
        <th>Mode</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>GCLatencyMode.Batch</td>
        <td>designed to give maximum throughput and performance for sections of an app where UI responsiveness isn't important.</td>
    </tr>
    <tr>
        <td>GCLatencyMode.LowLatency</td>
        <td>this mode reduces the impact of GC to a minimum, which is ideal for times when things like UI responsiveness are critical, e.g. animation.</td>
    </tr>
    <tr>
        <td>GCLatencyMode.Interactive</td>
        <td>Workstation GC with Concurrency switched on, giving a balance between GC efficiency and app responsiveness.</td>
    </tr>
</table>

* An obvious use of LatencyMode is to <strong>change it for a short period during execution of critical code that needs maximum UI or batch processing performance, and then change it back on completion</strong>.

```
using System.Runtime;
…

// Store current latency mode
GCLatencyMode mode = GCSettings.LatencyMode;

// Set low latency mode
GCSettings.LatencyMode = GCLatencyMode.LowLatency;

try
{
    // Do some critical animation work
}
finally
{
    // Restore latency mode
    GCSettings.LatencyMode = mode;
}
```

#### GC Notifications ####
* Basically, with NET Framework 3.5.1, you can do the following things:
  * register for full GC notifications
  * determine when a full GC is approaching
  * determine when a full GC has finished
  * unregister for full GC notifications
 
#### Weak References ####
* Weak object references allow you to keep hold of objects (as they are another source of GC roots), but still allow them to be collected if the GC needs to.

#### Under the hood ####
* Weak references are classified as one of:
  * short weak references (which ignore finalization)
  * long weak references (which consider finalization)

```
//short
WeakReference wr = WeakReference(floor23, false);

//long
WeakReference wr = WeakReference(floor23, true);
```

#### Object Pinning and GC Handles ####
* The SOH is better managed than the LOH because it compacts and doesn't suffer from fragmentation issues (as mentioned in Chapter 2's LOH section). Well, that's not entirely true.
* If you want to make calls from your .NET application to other unmanaged application APIs or COM objects, you will probably want to pass data to them. If the data you pass is allocated on the heap, then it could be moved at some point during compaction as part of GC.

#### GC Handles ####
* NET uses a structure called GCHandle to keep track of heap objects, which can also be used to pass object references between managed and unmanaged domains.
* A `GCHandle` can be one of four types:

<table>
    <tr>
        <th>Type</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>Normal</td>
        <td>tracks standard heap objects</td>
    </tr>
    <tr>
        <td>Weak</td>
        <td>used to track short weak references</td>
    </tr>
    <tr>
        <td>Weak Track Resurrection</td>
        <td>used for long weak references</td>
    </tr>
    <tr>
        <td>Pinned</td>
        <td>used to fix an object at a specific address in memory</td>
    </tr>
</table>

#### Object pinning ####
* We can create GC Handles directly in code, and so can use them to create pinned references to objects we need to pass to unmanaged code.

```
byte[] buffer = new byte[512];
GCHandle h = GCHandle.Alloc(buffer, GCHandleType.Pinned);
IntPtr ptr = h.AddrOfPinnedObject();

// Call native API and pass buffer

if (h.IsAllocated) 
{
    h.Free();
}
```

* Objects will also be pinned if you use a fixed block.

```
unsafe static void Main()
{
    Person p = new Person();
    p.age = 25;
    
    // Pin p
    fixed (int* a = &p.age)
    {
        // Do something
    }
    
    // p unpinned
}
```

#### Problems with object pinning ####
* The main problem with object pinning is that it can cause SOH fragmentation. If an object is pinned during a GC then, by definition, it can't be relocated. Depending on how you use pinning, it can reduce the efficiency of compaction, leaving gaps in the heap.
* The best advice is to pin for a very short time and then release, to minimize compaction issues.

### Section 2: Troubleshooting ###
#### Best practices ####
* Use the `IDisposable` interface with the Disposable pattern to release unmanaged resources, and suppress finalization in the Dispose method if no other finalization is required.
* Use the `using` statement to define the scope of a disposable object, unless doing so could cause an exception.
* Use `StringBuilder` when the number of string concatenations is unknown or contained within a loop.
* Initialize the capacity of a `StringBuilder` to a reasonable value, if possible.
* Use <strong>lazy initialization</strong> to defer the cost of instantiating members of a class if the members are not needed when the object is created.
* Use a `struct` to represent any immutable, single value whose instance size is 16 bytes or smaller and which will be infrequently boxed.
* Override the `GetHashCode` and `Equal`s methods of a `struct`.
* Mark all fields in a `struct` as `readonly`.
* Use the `readonl`y keyword on class fields that should be immutable.
* Implement `INotifyPropertyChanged` on component classes that may be used by binding clients.
* Avoid maintaining <strong>large object graphs</strong> in memory.
* Avoid using <strong>unsigned number types</strong> (except for byte).
* Prefer <strong>CLS-compliant</strong> types.
* Be aware of <strong>variable capture</strong> when using anonymous functions.
* Do not pass delegates to `IQueryable` extension methods.
* Avoid excessive grouping or aggregate functions in LINQ queries.
* Use <strong>static methods</strong> when it is unnecessary to access or mutate state.
* Initialize collections (such as `List<T>`) with the target size, if it is known.
* Consider using content instead of embedded resources for larger artifacts.
* In an IIS hosted application, set the `//compilation/system.web/compilation/debug` attribute to `false` before releasing to production.
* Remove event handlers before disposing of an object.

### Chapter 4: Common Memory Problems ###
#### Types ####
* The type system in .NET consists of
  * value
    * is a primitive or struct
  * reference
    * is a pointer
* There are two components to the data in a reference type,
  * the reference
  * the actual data
* The reference is stored in the stack, and it contains the memory location (in the heap) where the data is actually stored.

#### Value types ####
* Value types consist of 
  * simple types
  * enum types
  * struct types
  * nullable types
* Value types do not support user-defined inheritance, but they do have an inheritance chain as follows:
  * `Enums` inherit from `System.Enum`, which inherits from `System.ValueType`
  * nullable types are structs, and structs inherit from `System.ValueType`, which inherits from `System.Object`
* Variables of these types maintain their own copies of their data so, when a value is assigned from one variable to another, the data stored in the memory at one location (i.e. the memory allocated to the initial variable) is copied into the memory allocated for the new variable.

```
int x = 8;
int y = x;

Assert.AreEqual(y, 8);
Assert.AreNotSame(x, y); 
```

* The values may be equal, but modifying the value of `y` (e.g. with the ++ operator) will not affect the `x` variable. <strong>This is often the source of bugs when developers forget that changes to a value type parameter will not carry over to the calling method</strong>.
* The exception to the One variable, one memory location rule is usage of the `ref` and `out` keywords,

```
private void Increment(int value)
{
    value++;
}

private void Increment(ref int value)
{
    value++;
}

private void IncrementOut(out int value)
{
    value = default(int);
    value++;
}

[TestMethod]
public void OutRefTest()
{
    int x = 7;
    
    Increment(x);   
    Assert.AreEqual(7, x);
    
    Increment(ref x);   
    Assert.AreEqual(8, x);
    
    IncrementOut(out x);
    Assert.AreEqual(1, x);
}
```
* The first `Increment(x)` call copies the value into the parameter for the method and, as the value is never returned, the variable `x` remains the same. The second call is to the ref version of `Increment`.
* The `ref` keyword allows a value to be passed in, modified, and then passed out. This method call works as desired and increments the number. Calling the `ref` function with an initialized variable will result in a compile-time error.
* The `out` parameter is another source of confusion for many. It works like the `ref` keyword in that it passes the value out, but the value for the incoming parameter is not copied. The parameter for the method does not need to be initialized before the call, but it must be initialized within the body of the method. This causes the `IncrementOut` method to always return 1. Calling the `out` function with an initialized variable will cause that value to be discarded.

#### Reference types ####
* Reference types include
  * class types
  * interface types
  * array types
  * delegate types
  
#### Boxing and unboxing ####
* Value types can be converted to reference types through a process known as boxing, and back into value types through unboxing.
* Boxing is sometimes necessary, but it should be avoided if at all possible, because it will slow down performance and increase memory requirements.
* Boxing can be avoided by using parameterized classes and methods, which is implemented using generics; in fact this was the motivation for adding generics.

```
public void BoxedCall(object value)
{
    // Perform operation on value
}

public void NonboxedCall<T>(T value)
{
    // Perform operation on value
}
```

#### Disposing of unmanaged resources ####
* There are two ways to do so without an exception preventing the method's execution.

```
// Way 1
FileStream stream = new FileStream("message.txt", FileMode.Open);

try
{
    StreamReader reader = new StreamReader(stream);
    
    try
    {
        Console.WriteLine(reader.ReadToEnd());
    }
    finally
    {
        reader.Dispose();
    }
}
finally
{
    stream.Dispose();
}

// Way 2
using (FileStream stream = new FileStream("message.txt", FileMode.Open))
using (StreamReader reader = new StreamReader(stream))
{
    Console.WriteLine(reader.ReadToEnd());
}
```

* In most situations, the using syntax is cleaner, but there will be times when a try block is preferable.

#### Finalization ####
* A common problem with custom implementations of disposable objects is that these implementations are not complete, and so resources are not released.
* The issue arises when the Dispose method isn't called. Despite all the best practices, it is important to realize that not everyone remembers to call the Dispose method, or to employ the using statement in conjunction with a disposable. Sometimes a developer may not have noticed the Dispose method in the Intellisense method list.
* `~Sample()`; it is a language implementation for the Finalize method.
* A destructor is responsible for reclaiming all of the resources, whereas the finalizer only needs to worry about calling the Dispose method

#### Concatenation ####
* The compiler also converts concatenation with variables and non-string literals into a call to `String.Concat`.

```
string panda = "bear";
panda += "cat";

// after compiled
string panda = "bear";
panda = String.Concat(panda, "cat");
```

* Since string is immutable, this causes a new string to be allocated at every pass through the loop. This problem can be prevented by using the `StringBuilder` class.

#### Structs ####
* Used it only when the type represents a single value consisting of 16 bytes or less. It <strong>should be</strong> immutable.
* Structs inherit from `System.Struct` and cannot be further inherited.
* A default constructor is provided by the CLR and cannot be custom defined, and field values cannot be initialized.
* Even though a struct cannot participate in inheritance, it can implement an interface.
* Type casting a struct to an interface implicitly boxes the struct, which incurs the overhead of boxing and unboxing, and also has an interesting side effect.
* This struct can be made immutable by making its fields read-only and removing the property setters.

```
public partial struct GeographicPoint
{
    private readonly float latitude;
    private readonly float longitude;
    private readonly int altitude;
    
    public float Latitude
    {
        get { return latitude; }
    }
    
    public float Longitude
    {
        get { return longitude; }
    }
    
    public int Altitude
    {
        get { return altitude; }
    }
    
    public GeographicPoint(float latitude, float longitude, int altitude)
    {
        this.latitude = latitude;
        this.longitude = longitude;
        this.altitude = altitude;
    }
    
    public GeographicPoint ChangeLatitude (float newLatitude)
    {
        return new GeographicPoint(latitude, Longitude, Altitude);
    }

    public GeographicPoint ChangeLongitude (float newLongitude)
    {
        return new GeographicPoint(Latitude, newLongitude, Altitude);
    }
}
```

* Although this struct is now immutable, it will still have an issue with Hashtables. When struct objects are used as Hashtable keys, the lookup operation for the Hashtable is slower because of the implementation of the GetHashCode method used to perform the lookup.
* By implementing our own Equals() method we can optimize it and eliminate reflection and boxing.

```
public partial struct GeographicPoint
{
    public static bool operator ==(GeographicPoint first, GeographicPoint second)
    {
        return first.Equals(second);
    }
    
    public static bool operator !=(GeographicPoint first, GeographicPoint second)
    {
        return !first.Equals(second);
    }
    
    public override int GetHashCode()
    {
        return (int)this.latitude ^ (int)this.longitude ^ this.altitude;
    }
    
    public override bool Equals(object obj)
    {
        if (obj is GeographicPoint)
        {
            return Equals((GeographicPoint)obj);
        }
        
        return false;
    }
    
    public bool Equals(GeographicPoint other)
    {
        return this.latitude == other.latitude
            && this.longitude == other.longitude
            && this.altitude == other.altitude;
    }
}
```

* We can now ensure `GeographicPoint` is thread-safe, `Hashtable` friendly, and works with equality operators.

#### Effects of Yield ####
* The yield keyword is used to generate an implementation of the Iterator pattern.
* Only one item at a time has to be in memory, which can be a dramatic improvement if only one object is used at a time.

#### Arrays and Collections ####
* If we do not properly initialize our List, we will need to perform several resizing operations as the size grows.
* With large arrays, we may actually have twice as much memory allocated as we would expect.
* Use the overloaded constructor where you can specify the initial capacity. This will reduce the number of initial resizing operations that are needed.

#### Excessive References ####
* Use LazyLoading to delay initializing related properties until they are referenced.

### Chapter 5: Application-Specific Problems ###
#### Debug ####
* This is the default setting in the web.config, but it has an unfortunate side effect from a performance perspective. This setting will inject debug symbols into the compiled assemblies.

```
<compilation debug="true">
```

#### StringBuilder ####
* When you create the StringBuilder, you can explicitly initialize the size of the internal buffer.

#### ADO.NET ####
* Explicitly wrap Connections, Commands, and Adapters with using statements.

#### LINQ ####
* The improved memory management rests in the potential of delayed execution and having to load entire results lists into memory at the same time.
* The SQL that is generated will actually exclude the filtering restrictions, which will not be interpreted until after the SQL is run and the results returned.
* Instead of having the filter use indexes in the database and call up a small subset of the data, no indexes will be used, and the entire contents of the table will be retrieved.

```
// bad
public bool PastDueAccount(Account account)
{
    return account.DueDate < DateTime.Today.AddDays(-7);
}

Order[] pastDueAccounts = null;

using (var context = new Context())
{
    pastDueAccounts = context.Accounts.Where(account => PastDueAccount(account)).ToArray();
}
```

* This way, the lambda expression will be passed to the LINQ provider, and its logic will be incorporated into the final query.

```
// good
public Expression<Func<Order, bool>> PastDueAccount
{
    get
    {
        DateTime startDate = DateTime.Today.AddDays(-7);
        return order => order.TransactionDate > startDate;
    }
}
```
#### Weak event pattern ####
* The downside is that you will generally need a `WeakEventManager` for every event that you want a weak event reference to.

#### Command bindings ####
* A good practice is to clear all of the binding information when you are through with the child. From within the context of the child window, you can easily clear this binding information.

```
BindingOperations.ClearAllBindings(this);
```

#### WCF Disposable ####
* Avoid the `Using` statement with a service proxy. If you do try it, then the `Dispose()` method of a proxy will call the `Close()` method, which will cause an exception if a fault has occurred on a channel.
* Relying on the `Using` construct may also result in orphaned service clients and unmanaged resources.

```
// best practice
var clientproxy = new UnderTheHoodClient();

try
{
    clientproxy.Method();
    clientProxy.Close();
}
catch(CommunicationException)
{
    clientProxy.Abort();
}
catch (TimeoutException)
{
    clientProxy.Abort();
}
````

### Chapter 6: A Few More Advanced Topics ###
#### 32-Bit vs. 64-Bit ####
* Any given program compiled as a 64-bit application will use more memory than the 32-bit version of the same program.
* You may want to switch some objects from classes to structs to make the most effective use of memory. However, bear in mind that this tactic may backfire if you are forced to box and unbox the new structs. Generics will help avoid boxing.
* <strong>The Large Object Heap does not change. The size threshold is the same for 64 bits as it was for 32 bits</strong> and, since your objects will be bigger in a 64-bit environment, this means that you may have objects winding up in the LOH that were normally placed in the SOH when everything was 32 bit.

#### Survey of Garbage Collection Flavors ####
<table>
    <tr>
        <th>Workstation GC</th>
        <th>Server GC</th>
    </tr>
    <tr>
        <td>is the default and is the only option available on a single processor computer.</td>
        <td>is available whenever you have more than one processor in the machine.</td>
    </tr>
    <tr>
        <td>favors responsiveness</td>
        <td>favors throughput</td>
    </tr>
    <tr>
        <td>runs on the same thread that triggered the garbage collection. This thread will typically run at normal priority, and must compete with all the other running threads for processing time. Since this thread is running at normal priority, it will easily be preempted by the other threads that the operating system is running. This can mean that garbage collecting takes longer than if it had a dedicated thread that was set at a higher priority.</td>
        <td>uses multiple threads, each running at the highest priority, making it significantly faster than Workstation GC. Each of these threads will be given the highest priority, so they will finish substantially faster than under a workstation equivalent, but they will also preempt all other threads, so all that speed is not free.</td>
    </tr>
</table>

* Essentially, while Server GC is faster than Workstation GC on the same size heap, there can be problems if you're running hundreds of instances of an app because of excessive context switching between threads.
* Concurrent GC is intended to minimize screen freezes during garbage collection and is best suited to application scenarios where a responsive UI is the most critical design goal.

```
<configuration>
    <runtime>
        <gcConcurrent enabled="true" />
    </runtime>
</configuration>
```

* .NET 4.0 introduced a new flavor to the table: Background GC. Technically this isn't really a new option, as it replaces Concurrent GC, although there have been improvements. In particular, Background GC works just like Concurrent GC except that your application isn't blocked when the ephemeral segment is full.
