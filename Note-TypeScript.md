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
          <td>Use in variables.</td>
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

#### Generics ####
<table>
    <tbody>
        <tr>
            <th>Type argument inference</th>
        </tr>
          <td>The compiler to set the value of T for us automatically based on the type of the argument we pass in.</td>
        </tr>
        </tr>
          <td><pre lang="typescript">
//Normally
let output = identity&lt;string&gt;("myString");
<br />
//Type argument inference
let output = identity("myString");
          </pre></td>
        </tr>
    </tbody>
</table>

<table>
    <tbody>
        <tr>
            <th>Generics Constraints</th>
        </tr>
          <td>Create an interface that describes our constraint and use the extends keyword to denote(表示) our constraint.</td>
        </tr>
        </tr>
        <td><pre lang="typescript">
interface Lengthwise {
    length: number;
}
<br />
function loggingIdentity<T extends Lengthwise>(arg: T): T {
  // Now we know it has a .length property, so no more error
    console.log(arg.length);
    return arg;
}
<br />
//Error, number doesn't have a .length property
loggingIdentity(3);
<br />
//Need to pass in values whose type has all the required properties
loggingIdentity({length: 10, value: 3});
          </pre></td>
        </tr>
    </tbody>
</table>

<table>
    <tbody>
        <tr>
            <th>Type Parameters in Generic Constraints</th>
        </tr>
          <td>Declare a type parameter that is constrained by another type parameter.</td>
        </tr>
        </tr>
        <td><pre lang="typescript">
function getProperty<T, K extends keyof T>(obj: T, key: K) {
    return obj[key];
}
<br />
let x = { a: 1, b: 2, c: 3, d: 4 };
<br />
//okay
getProperty(x, "a");
<br />
// error: Argument of type 'm' isn't assignable to 'a' | 'b' | 'c' | 'd'.
getProperty(x, "m");
          </pre></td>
        </tr>
    </tbody>
</table>

#### Enums ####
<table>
    <tbody>
        <tr>
            <th>Enum Member as Type</th>
        </tr>
        <td><pre lang="typescript">
enum ShapeKind {
    Circle,
    Square,
}
<br />
interface Circle {
    kind: ShapeKind.Circle;
    radius: number;
}
<br />
interface Square {
    kind: ShapeKind.Square;
    sideLength: number;
}
<br />
let c: Circle = {
    //Error, kind is ShapeKind.Circle type in Circle interface
    kind: ShapeKind.Square,
    radius: 100,
}
          </pre></td>
        </tr>
    </tbody>
</table>

<table>
    <tbody>
        <tr>
            <th>Reverse mappings</th>
        </tr>
        <tr>
            <td>String enum members do not get a reverse mapping generated at all.</td>
        </tr>
        <td><pre lang="typescript">
enum Enum {
    A
}
<br />
let a = Enum.A;
<br/>
//"A"
let nameOfA = Enum[a];
          </pre></td>
        </tr>
    </tbody>
</table>

<table>
    <tbody>
        <tr>
            <th>enum</th>
            <th>const enum</th>
        </tr>
        <td><pre lang="typescript">
enum Color {Red, Green, Blue};
const enum ConstantColor {Red, Green, Blue}
<br />
var blue = Color["Blue"];  //OK
var constantBlue = ConstantColor["Blue"]; //OK
<br />
var cyanString = Color[5]; //OK
var constCyanString = ConstantColor[5]; //OK
        </pre></td>
        <td><pre lang="typescript">
enum Color {Red, Green, Blue};
const enum ConstantColor {Red, Green, Blue}
<br />
var blue = Color["Blue"];  //OK
var constantBlue = ConstantColor["Blue"]; //OK
<br />
var cyanString = Color[5]; //OK
var constCyanString = ConstantColor[5]; //Error
        </pre></td>
        </tr>
    </tbody>
</table>

#### Type Inference ####
<table>
    <tbody>
        <tr>
            <td>The inference (推理) takes place when initializing variables and members, setting parameter default values, and determining function return types.</td>
        </tr>
    </tbody>
</table>

<table>
    <tbody>
        <tr>
            <th>Contextual Type</th>
        </tr>
        <tr>
            <td>Contextual typing occurs when the type of an expression is implied by its location. For example: the TypeScript type checker used the type of the Window.onmousedown function to infer the type of the function expression on the right hand side of the assignment.</td>
        </tr>
        <tr>
          <td><pre lang="typescript">
window.onmousedown = function(mouseEvent) {
    console.log(mouseEvent.clickTime); //Error during compile time
};
<br />
window.onmousedown = function(mouseEvent: any) {
    console.log(mouseEvent.clickTime);  //No Error during compile time
};
          </pre></td>
       </tr>
    </tbody>
</table>

#### Type Compatibility ####
<table>
    <tbody>
      <tr>
        <th>Structural Subtyping</th>
        <th>Nominal Typing</th>
      </tr>
      <tr>
        <td>Type compatibility in TypeScript is based on structural subtyping. Structural typing is a way of <b>relating types based solely on their members</b></td>
        <td>In nominally-typed languages like C# or Java, the equivalent code would be an error because the Person class does not explicitly describe itself as being an implementer of the Named interface.</td>
      </tr>
            <tr>
        <td colspan="2"><pre lang="typescript">
interface Named {
    name: string;
}
<br />
let x: Named;
<br />
// y's inferred type is { name: string; location: string; }
let y = { name: "Alice", location: "Seattle" };
x = y;
        </pre></td>
      </tr>
    </tbody>
</table>

<table>
    <tbody>
      <tr>
        <td>Object</td>
        <td>To check whether y can be assigned to <b>x</b>, the compiler checks each property of <b>x</b> to find a corresponding compatible property in <b>y</b>. In this case, <b>y</b> must have a member called name that is a string. It does, so the assignment is allowed.</td>
        <td><pre lang="typescript">
let x: Named;
<br />
//y's inferred type is { name: string; location: string; }
let y = { name: "Alice", location: "Seattle" }; 
        </pre></td>
      </tr>
      <tr>
        <td>Function</td>
        <td>To check if <b>x</b> is assignable to <b>y</b>, we first look at the parameter list. Each parameter in <b>x</b> must have a corresponding parameter in <b>y</b> with a compatible type. Note that the names of the parameters are not considered, only their types. In this case, every parameter of <b>x</b> has a corresponding compatible parameter in <b>y</b>, so the assignment is allowed. <br /><br />The second assignment is an error, because <b>y</b> has a required second parameter that <b>x</b> does not have, so the assignment is disallowed.</td>
        <td><pre lang="typescript">
let x = (a: number) => 0;
let y = (b: number, s: string) => 0;
<br />
y = x; //OK
x = y; //Error
<br />
//different properties in object literal as return type
let x = () => ({name: "Alice"});
let y = () => ({name: "Alice", location: "Seattle"});
<br />
x = y; //OK
y = x; //Error, because x() lacks a location property
        </pre></td>
      </tr>
      <tr>
        <td>Class</td>
        <td>Only members of the instance are compared. Static members and constructors do not affect compatibility.</td>
        <td><pre lang="typescript">
class Animal {
    feet: number;
    constructor(name: string, numFeet: number) { }
}
<br />
class Size {
    feet: number;
    constructor(numFeet: number) { }
}
<br />
let a: Animal;
let s: Size;
<br />
a = s;  //OK
s = a;  //OK
        </pre></td>
      </tr>
      <tr>
        <td>Private & protected members in classes</td>
        <td colspan="2">Private and protected members in a class affect their compatibility. When an instance of a class is checked for compatibility, if the target type contains a private member, then the source type must also contain a private member that originated from the same class. <br /><br /> Likewise, the same applies for an instance with a protected member. This allows a class to be assignment compatible with its super class, but not with classes from a different inheritance hierarchy which otherwise have the same shape.</td>
      </tr>
      <tr>
        <td>Generics</td>
        <td>Type parameters only affect the resulting type <b>when consumed as part of the type of a member</b>.</td>
        <td><pre lang="typescript">
interface Empty<T> {
}
<br />
let x: Empty<number>;
let y: Empty<string>;
<br />
x = y;  //OK, because y matches structure of x
<br />
interface NotEmpty<T> {
    data: T;
}
<br />
let x: NotEmpty<number>;
let y: NotEmpty<string>;
<br />
x = y;  //Error, because x and y are not compatible
        </pre></td>
      </tr>
    </tbody>
</table>

#### Advanced Types ####

<table>
    <tbody>
      <tr>
        <td>Intersection</td>
        <td>Union</td>
      </tr>
      <tr>
        <td>Combines multiple types into one.</td>
        <td>Combines multiple types, but only allow one. E.g Either a or b</td>
      </tr>
      <tr>
        <td><pre lang="typescript">
interface X {
  c: string;
  d: string;
}
<br />
interface Y {
  c: number;
  e: string
}
<br />
type XY = X & Y;
type YX = Y & X;
<br />
let p: XY;
let q: XY;
<br />
p.c = 4; //Error
q.c = 3; //Error
<br />
p.c = 'text'; //Error
q.c = 'text'; //Error
        </pre></td>
        <td><pre lang="typescript">
interface IStudent {
    id: string;
    age: number;
}
<br />
interface IWorker {
    companyId: string;
}
<br />
type IUnionType = IStudent | IWorker;
<br />
let p: IUnionType = {
    id: 'ID3241',
    age: 21
};
<br />
p = 3; //Error
<br />
p = {
    companyId: 'cid993'
};
        </pre></td>
      </tr>
    </tbody>
</table>

* https://www.typescriptlang.org/docs/handbook/basic-types.html
* https://www.logicbig.com/tutorials/misc/typescript/getting-started.html
