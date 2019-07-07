#### Episode 1 ####
* An Evolution of data

<table>
  <tbody>
    <tr>
      <th></th>
      <th>Advantages</th>
      <th>Disadvantages</th>
    </tr>
    <tr>
      <td>CSV</td>
      <td>
        <ul>
          <li>Easy to parse</li>
          <li>Easy to read</li>
          <li>Easy to make sense of</li>
        </ul>
      </td>
      <td>
        <ul>
          <li>The data types of elements has to beinferred and is not a guarantee</li>
          <liParsing becomes tricky when data contains commas</li>
          <li>Column names may or may not be there</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Relational tables definitions</td>
      <td>
        <ul>
          <li>Data is fully typed</li>
          <li>Data fits in a tabled</li>
        </ul>
      </td>
      <td>
        <ul>
          <li>Data has to be flat</li>
          <li>Data is stored in a database, and data definition will be different for each database</li>
        </ul>
      </td>
    </tr>
        <tr>
      <td>JSON (JavaScript Object Notation)</td>
      <td>
        <ul>
          <li>Data can take any form (arrays, nested elements)</li>
          <li>Widely accepted format on the web</li>
          <li>Can be read by pretty much any language</li>
          <li>Can be easily shared over a network</li>
        </ul>
      </td>
      <td>
        <ul>
          <li>Data has no schema enforcing</li>
          <li>JSON Objects can be quite big in size because of repeated keys</li>
          <li>No comments, metadata, documentation</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Protocol Buffers</td>
      <td>
        <ul>
          <li>Data is fully typed</li>
          <li>Data is compressed automatically (less CPU usage)</li>
          <li>Schema (defined using .proto file) is needed to generate code and read the data</li>
          <li>Documentation can be embedded in the shcema</li>
          <li>Schema can evolve over time, in a safe manner (schema evolution)</li>
          <li>3-10x smaller, 20-100x faster than XML</li>
          <li>Code is generated for you automatically</li>
        </ul>
      </td>
      <td>
        <ul>
          <li>Protobuf support for some languages might be lacking</li>
          <li>Can't "open" the serialized data with text editor (because it's compressed and serialised)</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

#### Episode 3 ####
* Basics I

<table>
    <tbody>
      <tr>
        <th></th>
        <th>Description</th>
      </tr>
      <tr>
        <td>Message</td>
        <td>
          <img
            src="https://user-images.githubusercontent.com/5309726/60763573-ec8b9a80-a0a9-11e9-8bd4-b205a0752583.png"
          />
        </td>
      </tr>
      <tr>
        <td>Number</td>
        <td>
          <ul>
            <li>double</li>
            <li>float</li>
            <li>int32</li>
            <li>int64</li>
            <li>uint32</li>
            <li>uint64</li>
            <li>sint32</li>
            <li>sint64</li>
            <li>fixed32</li>
            <li>fixed64</li>
            <li>sfixed32</li>
            <li>sfixed64</li>
          </ul>
        </td>
      </tr>
          <tr>
        <td>Boolean</td>
        <td>
          <ul>
            <li>True</li>
            <li>False</li>
          </ul>
        </td>
      </tr>
              <tr>
        <td>String</td>
        <td>
          <ul>
            <li>UTF-8 encoded</li>
            <li>7-bit ASCII text</li>
          </ul>
        </td>
      </tr>
       <tr>
        <td>Bytes</td>
        <td>
          <ul>
            <li>Use these bytes to include a small image</li>
          </ul>
        </td>
      </tr>
    </tbody>
  </table>
  
* Tags
  * For protobuf the important element is the "tag"
  * Smallest tag, 1
  * Largest tag, 2^29 - 1, or 536,870,911
  * Cannot use the numbers 19000 to 19999 (reserved by google)
  * Tags numbered from 1 to 15 use 1 byte in space, 16 to 2047 use 2 bytes
* Repeated Fields
  * To make a "list" or an "array"
  * Can take any number (0 or more)
* Default values for fields
  * bool, false
  * number, 0
  * string, empty string
  * bytes, empty bytes
  * enum, first value
  * repeated, empty list
* Enums
  * The first value of an Enum is the default value
  * Enum must start by the tag 0 (which is the default value)

#### Episode 9 ####
* Data Evolution

![data-evo](https://user-images.githubusercontent.com/5309726/60764230-780c2800-a0b8-11e9-8fb6-e2d1475897ae.png)

* Updating Protocol Rules (https://developers.google.com/protocol-buffers/docs/proto#updating)
  * Don't change the numeric tags for any existing fields
  * Add new fields, an old code will just ignore them
  * If the old / new code read unknown data, the default will take place
  * Fields can be removeds, as long as the tag number is not used again in updated message type. Can use `OBSOLETE_` make the tag reserved
  * For data type changes (int32 to int64) avoid
