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

.NET has to create the object on the memory heap, determine its address on the heap (or object reference), and place that object reference within the stack frame for Method1. As long as Method1 is executing, the object allocated on the heap will have a reference held on the stack. When Method1 completes, the stack frame is removed (along with the object reference), leaving the object without a reference.

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
