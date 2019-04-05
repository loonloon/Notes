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
