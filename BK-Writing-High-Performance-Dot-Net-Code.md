
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
    </dd>
</dl>

* P/Invoke
* Delegates
* Exceptios
* dynamic
* Reflection
* Code Generation
* Preprocessing

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
