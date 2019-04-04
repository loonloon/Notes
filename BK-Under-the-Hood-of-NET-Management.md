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

