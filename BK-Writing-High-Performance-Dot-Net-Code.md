
#### General Coding and Class Design ####
<dl>
    <dt>Classes and Structs</dt>
    <dd>
        <table>
            <tr>
                <th></th>
                <th>Bytes</th>
            </tr>
            <tr>
                <td>Copy of pointer (passing object around)</td>
                <td>4 or 8</td>
            </tr>
            <tr>
                <td>Object fixed overhead</td>
                <td>8 for 32-bit proccess, 16 for 64-bit proccess</td>
            </tr>
            <tr>
                <td>struct</td>
                <td>No overhead at all and its memory usage is a sum of the size of all its fields</td>
            </tr>
            <tr>
                <td colspan="2">
                    Assume the data structure contains 16 bytes of data, the array length is 1,000,000 and this is a 32-bit system.
                    <br />
                    <br />
                    For an array of objects that total space usage:
                    <br />
                    (8 bytes array overhead) + ((4 byte pointer size) * 1000000) + ((8 bytes overhead + 16 bytes data) * 1000000) = 28MB
                    <br />
                    <br />
                    For an array of structs that total space usage:
                    <br />
                    (8 bytes array overhead) + (16 bytes data * 1000000) = 28MB
                </td>
            </tr>
        </table>
    </dd>
    <dt>Virtual Methods and Sealed Classes</dt>
    <dd>Do not mark methods virtual by default, "just in case".</dd>
    <dt>Override Equals and GetHashCode for Structs</dt>
    <dd>
      * Override Equals(object), still going to have wrose performance because that methods involes casting and boxing on value types.
      <br />
      * Implement IEquatable<T> interface for structs.
      <br />
      * If your type is sortable, should also implement the IComparable<T>.
    </dd>
    <dt>Tuples</dt>
    <dd>
      * Starting with .NET 4.7, there is a value type version of tuples, System.ValueTuple.
      <br />
      * Can name the property.
    </dd>
    <dt>Interface Dispatch</dt>
    <dd></dd>
    <dt>Avoid Boxing</dt>
    <dd>Boxing costs CPU time for object allocation, copying and casting, but more seriously, it results in more pressure on the GC heap.</dd>
    <dt>ref returns and locals</dt>
    <dd>Use ref-return to avoid copying struct values when you cannot use an immutable struct.</dd>
    <dt>for vs foreach</dt>
    <dd>
      * Using standard for loops is significantly faster in all the cases.
      <br />
      * foreach is an abstraction of a loop, and IEnumerable is an abstraction of a collection.
    </dd>
    <dt>Casting</dt>
    <dd>
      <table>
        <tr>
          <th></th>
          <th>Performance</th>
        </tr>
        <tr>
          <td>No cast</td>
          <td>1.00x</td>
        </tr>
        <tr>
          <td>Up cast (1 gen)</td>
          <td>1.00x</td>
        </tr>
        <tr>
          <td>Up cast (2 gen)</td>
          <td>1.00x</td>
        </tr>
        <tr>
          <td>Up cast (3 gen)</td>
          <td>1.00x</td>
        </tr>
        <tr>
          <td>Down cast (1 gen)</td>
          <td>1.23x</td>
        </tr>
        <tr>
          <td>Down cast (2 gen)</td>
          <td>1.37x</td>
        </tr>
        <tr>
          <td>Down cast (3 gen)</td>
          <td>1.37x</td>
        </tr>
        <tr>
          <td>Interface</td>
          <td>2.73x</td>
        </tr>
        <tr>
          <td>Invalid Cast</td>
          <td>14934.51x</td>
        </tr>
        <tr>
          <td>as (success)</td>
          <td>1.01x</td>
        </tr>
        <tr>
          <td>as (failure)</td>
          <td>2.60x</td>
        </tr>
        <tr>
          <td>is (success)</td>
          <td>2.00x</td>
        </tr>
        <tr>
          <td>is (failure)</td>
          <td>1.98xx</td>
        </tr>
      </table>
        * Use as instead of is
    </dd>
    <dt>P/Invoke</dt>
    <dd>Can reduce some of the cost by disabling some security checks on P/Inoke method declarations.</dd>
    <dt>Delegates</dt>
    <dd>
        <pre lang="typescript">
private delegate int MathOp(int x, int y);
private static int Add(int x, int y) { return x + y }
private static int DoOperation(MathOp op, int x, int y) { return op(x, y); }
<br />
//option 1: Slower, in IL it shows newobj inside the loop
for(var i = 0; i < 10; i++)
{
    DoOperation(Add, 1, 2);
}
<br />
//option 2: Faster, in IL it shows newobj outside the loop
MathOp op = Add;
for(var i = 0; i < 10; i++)
{
    DoOperation(Add, 1, 2);
}
<br />
//Equivalent to option 2:
for(var i = 0; i < 10; i++)
{
    DoOperation((x, y) => { return Add(x, y); }, 1, 2);
}
        </pre>
    </dd>
    <dt>Exceptios</dt>
    <dd>Exceptions should be truly exceptional. Using them as a matter of course can dstroy your performance.</dd>
    <dt>dynamic</dt>
    <dd>Using the dynamic keyword is not going to be highly optimized.</dd>
    <dt>Reflection</dt>
    <dd>
        * MethodInfo.Invoke is about 100 times slower than casting the object to an interface and executing it directly.
        <br />
        * If a common interface is not possible, then see the Code Generation to execute dynamically loaded assemblies much faster than reflection.
    </dd>
    <dt>Code Generation</dt>
    <dd>
        <table>
            <tr>
                <th>Create Instance</th>
                <th>Performance</th>
            </tr>
            <tr>
                <td>Direct ctor</td>
                <td>1.0x</td>
            </tr>
            <tr>
                <td>Activator.CreateInstance</td>
                <td>9.3x</td>
            </tr>
            <tr>
                <td>Code gen</td>
                <td>2.0x</td>
            </tr>
            <tr>
                <th>Method Invoke</th>
                <th>Performance</th>
            </tr>
            <tr>
                <td>Direct ctor</td>
                <td>1.0x</td>
            </tr>
            <tr>
                <td>Activator.CreateInstance</td>
                <td>3.0x</td>
            </tr>
            <tr>
                <td>Code gen</td>
                <td>1.0x</td>
            </tr>
        </table>
    </dd>
   <dt>Preprocessing</dt>
    <dd>If something can be preprocessed, then it must be preprocessed.</dd>
</dl>

#### Using the .NET Framework ####
* Understand Every API You Call
* Multiple APIs for the Same Thing
* Collections
* Strings
* Avoid APIs that Throw Exceptions Under Normal Circumstances
* Avoid APIs That Allocate From the Large Object Heap
* Use Lazy Initialization
* The Surprisingly High Cost of Enums
* Tracking Time
* Regular Expressions
* LINQ
* Reading and Writing Files
* Optimizing HTTP Settings and Network Communication
