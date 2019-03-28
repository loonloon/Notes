
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
<dl>
    <dt>Understand Every API You Call</dt>
    <dd></dd>
    <dt>Multiple APIs for the Same Thing</dt>
    <dd>
        <table>
            <tr>
                <th>There are at least 9 different ways to parse XML in .NET</th>
            </tr>
            <tr>
                <td>XmlTextReader</td>
            </tr>
            <tr>
                <td>XmlValidatingReader</td>
            </tr>
            <tr>
                <td>XmlDocument</td>
            </tr>
            <tr>
                <td>XPathNavigator</td>
            </tr>
            <tr>
                <td>XPathDocument</td>
            </tr>
            <tr>
                <td>LINQ to XML</td>
            </tr>
            <tr>
                <td>DataContractSerializer</td>
            </tr>
            <tr>
                <td>XmlSerializer</td>
            </tr>
        </table>
        * XmlTextReader is very fast, but it is forward only and does no validation.
        <br />
        * XmlDocument is very convenient because it has a full object model loaded, but it is among the slowest options.
        <br />
        * Not all options will be equal, performance wise. Some will be faster, but use more memory.
    </dd>
    <dt>Collections</dt>
    <dd>
        <table>
            <tr>
                <th>Collections to Avoid</th>
            </tr>
            <tr>
                <td>ArrayList</td>
            </tr>
            <tr>
                <td>Hashtable</td>
            </tr>
            <tr>
                <td>Queue</td>
            </tr>
            <tr>
                <td>SortedList</td>
            </tr>
            <tr>
                <td>Stack</td>
            </tr>
            <tr>
                <td>ListDictionary</td>
            </tr>
            <tr>
                <td>HybridDictionary</td>
            </tr>
        </table>
        * Should never be used by new code, avoided are casting and boxing.
    </dd>
    <dt>Array</dt>
    <dd>
        * Use ArraySegment<T> and Span<T> refers to the relevant portion of the array.
        <br />
        * Jagged array is faster than Multi array (because of method call in IL).
    </dd>
    <dt>Generic Collections</dt>
    <dd>
        * Understand the BIG O notation for every collections.
        <br />
        * Understand the storage requirements for each collection, e.g. added 1000 4 byte intergers into each structure
        <table>
            <tr>
                <td></td>
                <td>Bytes</td>
            </tr>
            <tr>
                <td>List</td>
                <td>4,036</td>
            </tr>
            <tr>
                <td>Stack</td>
                <td>4,036</td>
            </tr>
            <tr>
                <td>Queue</td>
                <td>4,036</td>
            </tr>
            <tr>
                <td>SortedList</td>
                <td>8,076</td>
            </tr>
            <tr>
                <td>Dictionary</td>
                <td>22,144</td>
            </tr>
            <tr>
                <td>LinkedList</td>
                <td>24,028</td>
            </tr>
            <tr>
                <td>SortedSet</td>
                <td>24,044</td>
            </tr>
            <tr>
                <td>SortedDictionary</td>
                <td>24,076</td>
            </tr>
            <tr>
                <td>HashSet</td>
                <td>30,972</td>
            </tr>
        </table>
    </dd>
    <dt>Key Comparisons</dt>
    <dd>
        * Use StringComparer in dictionary.
    </dd>
    <dt>Strings</dt>
    <dd>
        * Use simplest way to compare.
        <br />
        * Use String.Compare method for string comparison purpose instead of ToLower and ToUpper, avoid allocating memory and putting more pressure on the garbage collector.
        <br />
        * StringBuilder with initialized capacity is fastest.
        <br />
        * String.Concat is faster than String.Format.
        <br />
        * Avoid string parsing.
        <br />
        * Use ReadOnlySpan<T> instead of Substrings.
    </dd>
    <dt>Avoid APIs that Throw Exceptions Under Normal Circumstances</dt>
    <dd></dd>
    <dt>Avoid APIs That Allocate From the Large Object Heap</dt>
    <dd>
        * Caching its results.
        <br />
        * Avoid retrieving the information you need via interop directly from the Win32 API.
    </dd>
    <dt>Use Lazy Initialization</dt>
    <dd>
        * Use Lazy<T>
    </dd>
    <dt>The Surprisingly High Cost of Enums</dt>
    <dd></dd>
    <dt>Tracking Time</dt>
    <dd></dd>
    <dt>Regular Expressions</dt>
    <dd></dd>
    <dt>LINQ</dt>
    <dd></dd>
    <dt>Reading and Writing Files</dt>
    <dd></dd>
    <dt>Optimizing HTTP Settings and Network Communication</dt>
    <dd></dd>
</dl>
