#### Basic Types ####
<table>
  <tr>
    <th>Data Type</th>
    <th>Description</th>
    <th>Examples</th>
  </tr>
  <tr>
    <td>Boolean</td>
    <td>Floating point values (hexadecimal, decimal, binary and octal)</td>
    <td>
      <pre lang="typescript">let isDone: boolean = false;</pre>
    </td>
  </tr>
  <tr>
    <td>Number</td>
    <td>Logical values: true or false</td>
    <td>
      <pre lang="typescript">
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
     </pre>
    </td>
  </tr>
  <tr>
    <td>String</td>
    <td>Textual data</td>
    <td>
      <pre lang="typescript">
let fullName: string = `Bob Bobbington`;
let age: number = 37;
let sentence: string = `Hello, my name is ${ fullName }.
     </pre>
    </td>
  </tr>
  <tr>
    <td>Array</td>
    <td></td>
    <td>
      <pre lang="typescript">
let list: number[] = [1, 2, 3];
<br />
//Generic array type
let list: Array<number> = [1, 2, 3]
     </pre>
    </td>
  </tr>
  <tr>
    <td>Tuple</td>
    <td>A data structure consisting of multiple type.</td>
    <td>
      <pre lang="typescript">
//Declare a tuple type
let x: [string, number];
<br />
//Initialize it
x = ["hello", 10]; //OK
<br />
//Initialize it incorrectly
x = [10, "hello"]; //Error
     </pre>
    </td>
  </tr>
  <tr>
    <td>Enum</td>
    <td>Friendly names to sets of numeric values, By default, enums begin numbering their members starting at 0.</td>
    <td>
      <pre lang="typescript">
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
<br />
//Displays 'Green' as its value is 2 above
let colorName: string = Color[2];
     </pre>
    </td>
  </tr>
  <tr>
    <td>Any</td>
    <td>Opt-out of type-checking and let the values pass through compile-time checks.</td>
    <td>
      <pre lang="typescript">
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false;
      </pre>
    </td>
  </tr>
  <tr>
    <td>Void</td>
    <td>The return type of functions that do not return a value.</td>
    <td>
      <pre lang="typescript">
function warnUser(): void {
    console.log("This is my warning message");
}
<br />
//Only assign undefined or null to variables
let unusable: void = undefined;
      </pre>
    </td>
  </tr>
  <tr>
    <td>Null and Undefined</td>
    <td>Are subtypes of all other types.</td>
    <td>
      <pre lang="typescript">
let u: undefined = undefined;
let n: null = null;
      </pre>
    </td>
  </tr>
  <tr>
    <td>Never</td>
    <td>Are subtypes of all other types, no type is a subtype of, or assignable to, never (except never itself).</td>
    <td>
      <pre lang="typescript">
function error(message: string): never {
    throw new Error(message);
}
      </pre>
    </td>
  </tr>
  <tr>
    <td>Object</td>
    <td>Represents the non-primitive type.</td>
    <td>
      <pre lang="typescript">
//Object literal notation
var person = { 
  firstName:"Tom",
  lastName:"J" 
};
<br />
      </pre>
    </td>
  </tr>
  <tr>
    <td>Type assertions</td>
    <td>Like a type cast in other languages, but performs no special checking or restructuring of data. It has no runtime impact, and is used purely by the compiler.</td>
    <td>
      <pre lang="typescript">
let someValue: any = "this is a string";
<br />
//Angle-bracket
let strLength: number = (<string>someValue).length;
<br />
//as-syntax
let strLength2: number = (someValue as string).length;
<br />
      </pre>
    </td>
  </tr>
</table>

https://www.typescriptlang.org/docs/handbook/interfaces.html
https://www.logicbig.com/tutorials/misc/typescript/discriminated-unions.html

#### Need to revise ####
1. Never type
2. Indexable Types
3. Difference between the static and instance sides of classes
4. Constructor functions
5. Ambient enums

