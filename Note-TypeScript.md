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

#### Variable Declarations ####
<table>
    <tbody>
        <tr>
            <th>Scoping</th>
            <th>var</th>
            <th>let</th>
        </tr>
        <tr>
          <td>Global</td>
          <td colspan="2">
            <pre lang="typescript">
//They are very similar when used like this outside a function block.
let me = 'go';  //globally scoped
var i = 'able'; //globally scoped
<br />
//Global variables defined with let will not be added 
//as properties on the global window object.
console.log(window.me); //undefined
console.log(window.i); //'able'
            </pre>
          </td>
        </tr>
        <tr>
          <td>Function</td>
          <td colspan="2">
            <pre lang="typescript">
//They are identical when used like this in a function block.
function ingWithinEstablishedParameters() {
    let terOfRecommendation = 'awesome worker!'; //function block scoped
    var sityCheerleading = 'go!'; //function block scoped
}
            </pre>
          </td>
        </tr>
        <tr>
          <td>Block</td>
          <td colspan="2">
            <pre lang="typescript">
//let is only visible in the for() loop and var is visible to the whole function.
function letExample() {
    //i is *not* visible out here
    for( let i = 0; i < 5; i++ ) {
        //i is only visible in here (and in the for() parentheses)
        //and there is a separate i variable for each iteration of the loop
    }
    //i is *not* visible out here
}
<br />
function varExample() {
    //j *is* visible out here
    for( var j = 0; j < 5; j++ ) {
        //j is visible to the whole function
    }
    //j *is* visible out here
}
            </pre>
          </td>
        </tr>
        <tr>
          <td>Redeclaration</td>
          <td colspan="2">
            <pre lang="typescript">
//Assuming in strict mode
'use strict';
let me = 'foo';
let me = 'bar'; //SyntaxError: Identifier 'me' has already been declared
<br />
'use strict';
var me = 'foo';
var me = 'bar'; //No problem, `me` is replaced.
            </pre>
          </td>
        </tr>
    </tbody>
</table>

<table>
    <tbody>
        <tr>
            <th>let</th>
            <th>const</th>
        </tr>
        <tr>
            <td colspan="2">
              <pre lang="typescript">
//Have the same scoping rules as let, but you can’t re-assign to them.
const numLivesForCat = 9;
const kitty = {
    name: "Aurora",
    numLives: numLivesForCat,
}
<br />
// Error
kitty = {
    name: "Danielle",
    numLives: numLivesForCat
};
<br />
// all "okay"
kitty.name = "Rory";
kitty.name = "Kitty";
kitty.numLives--;
              </pre>
          </td>
        </tr>
    </tbody>
</table>

<table>
    <tbody>
        <tr>
            <th colspan="2">Spread</th>
        </tr>
        <tr>
          <td colspan="2">The spread operator (in form of ellipsis) can be used to initialize arrays and objects from another array or object. You can also use spread operator for object destructuring.</td>
        </tr>
        <tr>
            <td>Array into another array</td>
            <td>
              <pre lang="typescript">
let first = [1, 2];
let second = [3, 4];
let bothPlus = [0, ...first, ...second, 5];
              </pre>
            </td>     
        </tr>
        <tr>
            <td>Object into another object</td>
            <td>
              <pre lang="typescript">
class C {
p = 12;
  m() {
  }
}
<br />
let c = new C();
let clone = { ...c };
clone.p; //ok
<br />
//lose methods when you spread instances of an object
clone.m(); //error!
              </pre>
            </td>     
        </tr>
    </tbody>
</table>

#### Interfaces ####
<table>
    <tbody>
        <tr>
            <th>readonly</th>
            <th>const</th>
        </tr>
          <td>Use in object properties.</td>
          <td>Use in object variables.</td>
        </tr>
    </tbody>
</table>

<table>
    <tbody>
        <tr>
            <th>Indexable Types</th>
            <th>Examples</th>
        </tr>
        <tr>
          <td>String, all properties should return the same type that the indexer returns.</td>
          <td>
            <pre lang="typescript">
interface States {
    [state: string]: boolean; //indexer
    screenName: number; //error
}
<br />
let s: States = {'enabled': true, 'maximized':false};
<br />
//false
let maximized = s['maximized'];
<br />
            </pre>
          </td>
        </tr>
        <tr>
          <td>Number</td>
          <td>
            <pre lang="typescript">
interface StringArray {
  [index: number]: string;
}
<br />
let myArray: StringArray;
myArray = ["Bob", "Fred"];
<br />
//Bob
let myStr: string = myArray[0];
            </pre>
          </td>
        </tr>
    </tbody>
</table>

<table>
    <tbody>
        <tr>
            <th>Interfaces Extending Classes</th>
        </tr>
        <tr>
          <td>
            <ul>
              <li>When you create an interface that extends a class with private or protected members, that interface type can only be implemented by that class or a subclass of it.</li>
              <li>This is useful when you have a large inheritance hierarchy, but want to specify that your code works with only subclasses that have certain properties. The subclasses don’t have to be related besides inheriting from the base class.</li>
            </ul>
          </td>
        </tr>
        <tr>
          <td>
            <pre lang="typescript">
class Control {
    private state: any;
}
<br />
interface SelectableControl extends Control {
    select(): void;
}
<br />
class Button extends Control implements SelectableControl {
    select() { }
}
<br />
class TextBox extends Control {
    select() { }
}
<br />
// Error: Property 'state' is missing in type 'Image'.
class Image implements SelectableControl {
    select() { }
}
<br />
class Location {

}
            </pre>
          </td>
        </tr>
    </tbody>
</table>

https://www.typescriptlang.org/docs/handbook/basic-types.html

https://www.logicbig.com/tutorials/misc/typescript/getting-started.html

#### Need to revise ####
3. Difference between the static and instance sides of classes
4. Constructor functions
5. Ambient enums
