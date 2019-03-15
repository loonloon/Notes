* https://www.typescriptlang.org/docs/handbook/basic-types.html
* https://www.logicbig.com/tutorials/misc/typescript/getting-started.html

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
      <td>
         <pre lang="typescript">
//Normally
let output = identity&lt;string&gt;("myString");
<br />
//Type argument inference
let output = identity("myString");
          </pre>
      </td>
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
      <td>
         <pre lang="typescript">
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
          </pre>
      </td>
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
      <td>
         <pre lang="typescript">
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
          </pre>
      </td>
      </tr>
   </tbody>
</table>

#### Enums ####
<table>
   <tbody>
      <tr>
         <th>Enum Member as Type</th>
      </tr>
      <td>
         <pre lang="typescript">
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
          </pre>
      </td>
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
      <td>
         <pre lang="typescript">
enum Enum {
    A
}
<br />
let a = Enum.A;
<br/>
//"A"
let nameOfA = Enum[a];
          </pre>
      </td>
      </tr>
   </tbody>
</table>
<table>
   <tbody>
      <tr>
         <th>enum</th>
         <th>const enum</th>
      </tr>
      <td>
         <pre lang="typescript">
enum Color {Red, Green, Blue};
const enum ConstantColor {Red, Green, Blue}
<br />
var blue = Color["Blue"];  //OK
var constantBlue = ConstantColor["Blue"]; //OK
<br />
var cyanString = Color[5]; //OK
var constCyanString = ConstantColor[5]; //OK
        </pre>
      </td>
      <td>
         <pre lang="typescript">
enum Color {Red, Green, Blue};
const enum ConstantColor {Red, Green, Blue}
<br />
var blue = Color["Blue"];  //OK
var constantBlue = ConstantColor["Blue"]; //OK
<br />
var cyanString = Color[5]; //OK
var constCyanString = ConstantColor[5]; //Error
        </pre>
      </td>
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
         <td>
            <pre lang="typescript">
window.onmousedown = function(mouseEvent) {
    console.log(mouseEvent.clickTime); //Error during compile time
};
<br />
window.onmousedown = function(mouseEvent: any) {
    console.log(mouseEvent.clickTime);  //No Error during compile time
};
          </pre>
         </td>
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
         <td colspan="2">
            <pre lang="typescript">
interface Named {
    name: string;
}
<br />
let x: Named;
<br />
// y's inferred type is { name: string; location: string; }
let y = { name: "Alice", location: "Seattle" };
x = y;
        </pre>
         </td>
      </tr>
   </tbody>
</table>
<table>
   <tbody>
      <tr>
         <td>Object</td>
         <td>To check whether y can be assigned to <b>x</b>, the compiler checks each property of <b>x</b> to find a corresponding compatible property in <b>y</b>. In this case, <b>y</b> must have a member called name that is a string. It does, so the assignment is allowed.</td>
         <td>
            <pre lang="typescript">
let x: Named;
<br />
//y's inferred type is { name: string; location: string; }
let y = { name: "Alice", location: "Seattle" }; 
        </pre>
         </td>
      </tr>
      <tr>
         <td>Function</td>
         <td>To check if <b>x</b> is assignable to <b>y</b>, we first look at the parameter list. Each parameter in <b>x</b> must have a corresponding parameter in <b>y</b> with a compatible type. Note that the names of the parameters are not considered, only their types. In this case, every parameter of <b>x</b> has a corresponding compatible parameter in <b>y</b>, so the assignment is allowed. <br /><br />The second assignment is an error, because <b>y</b> has a required second parameter that <b>x</b> does not have, so the assignment is disallowed.</td>
         <td>
            <pre lang="typescript">
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
        </pre>
         </td>
      </tr>
      <tr>
         <td>Class</td>
         <td>Only members of the instance are compared. Static members and constructors do not affect compatibility.</td>
         <td>
            <pre lang="typescript">
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
        </pre>
         </td>
      </tr>
      <tr>
         <td>Private & protected members in classes</td>
         <td colspan="2">Private and protected members in a class affect their compatibility. When an instance of a class is checked for compatibility, if the target type contains a private member, then the source type must also contain a private member that originated from the same class. <br /><br /> Likewise, the same applies for an instance with a protected member. This allows a class to be assignment compatible with its super class, but not with classes from a different inheritance hierarchy which otherwise have the same shape.</td>
      </tr>
      <tr>
         <td>Generics</td>
         <td>Type parameters only affect the resulting type <b>when consumed as part of the type of a member</b>.</td>
         <td>
            <pre lang="typescript">
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
        </pre>
         </td>
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
         <td>Can be one of several types.</td>
      </tr>
      <tr>
         <td>
            <pre lang="typescript">
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
        </pre>
         </td>
         <td>
            <pre lang="typescript">
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
        </pre>
         </td>
      </tr>
   </tbody>
</table>
<table>
   <tbody>
      <tr>
         <td>User-Defined Type Guards</td>
         <td>Examples</td>
      </tr>
      <tr>
         <td>type assertion</td>
         <td>
            <pre lang="typescript">
let pet = getSmallPet();
<br />
//Each of these property accesses will cause an error
if (pet.swim) {
    pet.swim();
}
else if (pet.fly) {
    pet.fly();
}
<br />
//OK
let pet = getSmallPet();
<br />
if ((<Fish>pet).swim) {
    (<Fish>pet).swim();
}
else {
    (<Bird>pet).fly();
}
        </pre>
         </td>
      </tr>
      <tr>
         <td>is</td>
         <td>
            <pre lang="typescript">
//Not working when the return type is boolean
function isFish(pet: Fish | Bird): pet is Fish {
    return (<Fish>pet).swim !== undefined;
}
<br />
//Both calls to 'swim' and 'fly' are now okay.
if (isFish(pet)) {
    pet.swim();
}
else {
    pet.fly();
}
        </pre>
         </td>
      </tr>
      <tr>
         <td>typeof</td>
         <td>
            <pre lang="typescript">
function isNumber(x: any): x is number {
    return typeof x === "number";
}
<br />
function isString(x: any): x is string {
    return typeof x === "string";
}
<br />
function padLeft(value: string, padding: string | number) {
    if (isNumber(padding)) {
        return Array(padding + 1).join(" ") + value;
    }
    if (isString(padding)) {
        return padding + value;
    }
    throw new Error(`Expected string or number, got '${padding}'.`);
}
        </pre>
         </td>
      </tr>
      <tr>
         <td>instanceof</td>
         <td>
            <pre lang="typescript">
            interface Padder {
            getPaddingString(): string
            }
            <br />
            class SpaceRepeatingPadder implements Padder {
            constructor(private numSpaces: number) { }
            getPaddingString() {
            return Array(this.numSpaces + 1).join(" ");
            }
            }
            <br />
            class StringPadder implements Padder {
            constructor(private value: string) { }
            getPaddingString() {
            return this.value;
            }
            }
            <br />
            //Type is 'SpaceRepeatingPadder | StringPadder'
            function getRandomPadder() {
            return Math.random() < 0.5 ?
            new SpaceRepeatingPadder(4) :
            new StringPadder("  ");
            }
            <br />
            let padder: Padder = getRandomPadder();
            <br />
            if (padder instanceof SpaceRepeatingPadder) {
            padder; // type narrowed to 'SpaceRepeatingPadder'
            }
            if (padder instanceof StringPadder) {
            padder; // type narrowed to 'StringPadder'
            }
         </td>
      </tr>
   </tbody>
</table>
<table>
   <tbody>
      <tr>
         <th colspan="2">Type Aliases</th>
      </tr>
      <tr>
         <td colspan="2"><b>Objects / Functions</b> - Both can be used to describe the shape of an object or a function signature. But the syntax differs.</td>
      </tr>
      <tr>
         <th>Type Aliases</th>
         <th>Interfaces</th>
      </tr>
      <tr>
         <td>
            <pre lang="typescript">
type Point = {
  x: number;
  y: number;
};
<br />
type SetPoint = (x: number, y: number) => void;            
            </pre>
         </td>
         <td>
            <pre lang="typescript">
interface Point {
  x: number;
  y: number;
}
<br />
interface SetPoint {
  (x: number, y: number): void;
}
            </pre>
         </td>
      </tr>
      <tr>
         <td colspan="2"><b>Other Types</b> - Unlike an interface, the type alias can also be used for other types such as primitives, unions, and tuples.</td>
      </tr>
      <tr>
         <th>Type Aliases</th>
         <th>Interfaces</th>
      </tr>
      <tr>
         <td>
            <pre lang="typescript">
//primitive
type Name = string;
<br />
//object
type PartialPointX = { x: number; };
type PartialPointY = { y: number; };
<br />
//union
type PartialPoint = PartialPointX | PartialPointY;
<br />
//tuple
type Data = [number, string];      
            </pre>
         </td>
         <td>
         </td>
      </tr>
      <tr>
         <td colspan="2"><b>Extend</b> - Both can be extended, but again, the syntax differs. Additionally, note that an interface and type alias are not mutually exclusive. An interface can extend a type alias, and vice versa.</td>
      </tr>
      <tr>
         <td colspan="2">
            <pre lang="typescript">
//Interface extends interface
interface PartialPointX { x: number; }
interface Point extends PartialPointX { y: number; }
<br />
//Type alias extends type alias
type PartialPointX = { x: number; };
type Point = PartialPointX & { y: number; };
<br />
//Interface extends type alias
type PartialPointX = { x: number; };
interface Point extends PartialPointX { y: number; }
<br />
//Type alias extends interface
interface PartialPointX { x: number; }
type Point = PartialPointX & { y: number; };
            </pre>
         </td>
      </tr>
      <tr>
         <td colspan="2"><b>Implements</b> - A class can implement an interface or type alias, both in the same exact way. Note however that a class and interface are considered static blueprints. Therefore, they can not implement / extend a type alias that names a union type.</td>
      </tr>
      <tr>
         <td colspan="2">
            <pre lang="typescript">
interface Point {
  x: number;
  y: number;
}
<br />
class SomePoint implements Point {
  x: 1;
  y: 2;
}
<br />
type Point2 = {
  x: number;
  y: number;
};
<br />
class SomePoint2 implements Point2 {
  x: 1;
  y: 2;
}
<br />
type PartialPoint = { x: number; } | { y: number; };

//FIX ME: can not implement a union type
class SomePartialPoint implements PartialPoint {
  x: 1;
  y: 2;
}
            </pre>
         </td>
      </tr>
      <tr>
         <td colspan="2"><b>Declaration merging</b> - Unlike a type alias, an interface can be defined multiple times, and will be treated as a single interface (with members of all declarations being merged).</td>
      </tr>
      <tr>
         <td colspan="2">
            <pre lang="typescript">
//These two declarations become:
//interface Point { x: number; y: number; }
interface Point { x: number; }
interface Point { y: number; }
<br />
const point: Point = { x: 1, y: 2 };
            </pre>
         </td>
      </tr>
   </tbody>
</table>
<table>
   <tbody>
      <tr>
         <th>Discriminated (判别) Unions</th>
      </tr>
      <tr>
         <td>
            You can combine singleton types, union types, type guards, and type aliases to build an advanced pattern called <b>discriminated unions</b>, also known as <b>tagged unions</b> or <b>algebraic data types</b>. There are three ingredients:
            <ul>
               <li>Types that have a common, singleton type property — the discriminant.</li>
               <li>A type alias that takes the union of those types — the union.</li>
               <li>Type guards on the common property.</li>
            </ul>
         </td>
      </tr>
      <tr>
         <td>
            <pre lang="typescript">
interface Square {
    kind: "square"; //Discriminant or tag
    size: number;
}
<br />
interface Rectangle {
    kind: "rectangle"; //Discriminant or tag
    width: number;
    height: number;
}
<br />
interface Circle {
    kind: "circle"; //Discriminant or tag
    radius: number;
}
<br />
type Shape = Square | Rectangle | Circle; //Union
<br />
function area(s: Shape) {
    switch (s.kind) {
        case "square": return s.size * s.size;
        case "rectangle": return s.height * s.width;
        case "circle": return Math.PI * s.radius ** 2;
    }
}
       </pre>
         </td>
      </tr>
   </tbody>
</table>
<table>
   <tbody>
      <tr>
         <th>Polymorphic (多态) `is` types</th>
      </tr>
      <tr>
         <td>A polymorphic this type represents a type that is the subtype of the containing class or interface. This is called F-bounded polymorphism. This makes hierarchical fluent interfaces much easier to express.
         </td>
      </tr>
      <tr>
         <td>
            <pre lang="typescript">
class BasicCalculator {
    public constructor(protected value: number = 0) { }
    public currentValue(): number {
        return this.value;
    }
    public add(operand: number): this {
        this.value += operand;
        return this;
    }
    public multiply(operand: number): this {
        this.value *= operand;
        return this;
    }
}
<br />
let v = new BasicCalculator(2)
            .multiply(5)
            .add(1)
            .currentValue();
       </pre>
         </td>
      </tr>
   </tbody>
</table>
      
#### Symbols ####
TBC

#### Iterators and Generators ####
<table>
   <tbody>
      <tr>
         <th colspan="2">Iterables</th>
      </tr>
      <tr>
         <td colspan="2">An object is deemed (认为) iterable if it has an implementation for the Symbol.iterator property. Some built-in types like Array, Map, Set, String, Int32Array, Uint32Array, etc. have their Symbol.iterator property already implemented. Symbol.iterator function on an object is responsible for returning the list of values to iterate on.
         </td>
      </tr>
      <tr>
         <th>for..of</th>
         <th>for..in</th>
      </tr>
      <tr>
         <td>Returns a list of <b>values</b> of the numeric properties of the object being iterated.</td>
         <td>Returns a list of <b>keys</b> on the object being iterated.</td>
      </tr>
      <tr>
         <td>
            <pre lang="typescript">
let list = [4, 5, 6];
<br />
for (let i of list) {
   console.log(i); //"4", "5", "6"
}
<br />
let pets = new Set(["Cat", "Dog", "Hamster"]);
pets["species"] = "mammals";
<br />
for (let pet of pets) {
    console.log(pet); //"Cat", "Dog", "Hamster"
}
            </pre>
         </td>
         <td>
            <pre lang="typescript">
let list = [4, 5, 6];
<br />
for (let i in list) {
   console.log(i); //"0", "1", "2",
}
<br />
let pets = new Set(["Cat", "Dog", "Hamster"]);
pets["species"] = "mammals";
<br />
for (let pet in pets) {
   console.log(pet); //"species"
}
            </pre>
         </td>
      </tr>
   </tbody>
</table>

#### Modules ####
<table>
   <tbody>
      <tr>
         <th>Default Export</th>
         <th>Named Export</th>
      </tr>
      <tr>
         <td>Can only have one default export per file.</td>
         <td>Can multiple exports per file.</td>
      </tr>
      <tr>
         <td><pre lang="typescript">
export default class MyClass { /* ... */ }
         </pre></td>
         <td><pre lang="typescript">
export class MyClass { /* ... */ }
export class MyOtherClass { /* ... */ }      
         </pre></td>
      </tr>
      <tr>
         <td>Can give it any name you like.</td>
         <td>The name specified in the braces needs to match the name of the export.</td>
      </tr>
      <tr>
         <td>If you default export a class and rename that class, it will only rename the class in that file and not any of the other references in other files.</td>
         <td>With named exports it will rename the class and all the references to that class in all the other files.</td>
      </tr>        
      <tr>
         <td><pre lang="typescript">
import MyClassAlias from "./MyClass";
<br />
//default working with Named Export
import { default as MyClass} from "./MyClass"
         </pre></td>
         <td><pre lang="typescript">
import { MyClass } from "./MyClass";
<br />
//Rename
import { MyClass as MyOtherClass } from "./MyClass";
<br />
         </pre></td>
      </tr>
   </tbody>
</table>

<table>
   <tbody>
      <tr>
         <th colspan="2">export default</th>
      </tr>
      <tr>
         <th>calculator.ts</th>
         <th>compiled.js </th>
      </tr>
      <tr>
         <td>
            <pre lang="typescript">
export default class Calculator {
    public add(num1, num2) {
        return num1 + num2;
    }
}
            </pre>
         </td>
         <td>
            <pre lang="typescript">
var Calculator = /** @class */ (function () {
    function Calculator() {}
    Calculator.prototype.add = function (num1, num2) {
        return num1 + num2;
    };
    return Calculator;
}());
exports["default"] = Calculator;
            </pre>
         </td>
      </tr>
      <tr>
         <th colspan="2">import ... from "module"</th>
      </tr>
      <tr>
         <td colspan="2">
            <ul>
               <li>A default export can be imported with any name.</li>
               <li>Functionally equivalent to import * as Calculator from "./calculator"; and then instantiating it using new Calculator.default().
</li>
            </ul>
         </td>
      </tr>
      <tr>
         <th>importer.ts</th>
         <th>compiled.js </th>
      </tr>
      <tr>
         <td>
            <pre lang="typescript">
import Calculator from "./calculator";
<br />
let calc = new Calculator();
<br />
console.log(calc.add(2, 2));
            </pre>
         </td>
         <td>
            <pre lang="typescript">
exports.__esModule = true;
<br />
var calculator = require("./calculator");
var calc = new calculator["default"]();
<br />
console.log(calc.add(2, 2));
            </pre>
         </td>
      </tr>
   </tbody>
</table>

<table>
   <tbody>
      <tr>
         <th colspan="2">export = ...</th>
      </tr>
      <tr>
         <th>calculator.ts</th>
         <th>compiled.js </th>
      </tr>
      <tr>
         <td>
            <pre lang="typescript">
export = class Calculator {
    public add(num1, num2) {
        return num1 + num2;
    }
}  
            </pre>
         </td>
         <td>
            <pre lang="typescript">
module.exports = /** @class */ (function () {
function Calculator() {}
Calculator.prototype.add = function (num1, num2) {
 return num1 + num2;
 };
 return Calculator;
}());
            </pre>
         </td>
      </tr>
      <tr>
         <th colspan="2">import ... = require("module")</th>
      </tr>
      <tr>
         <td colspan="2">This syntax is only used when importing a CommonJS module.</td>
      </tr>
      <tr>
         <th>importer.ts</th>
         <th>compiled.js </th>
      </tr>
      <tr>
         <td>
            <pre lang="typescript">
import Calculator = require("./calculator");
<br />
let calc = new Calculator();
<br />
console.log(calc.add(2, 2));
            </pre>
         </td>
         <td>
            <pre lang="typescript">
exports.__esModule = true;
<br />
var Calculator = require("./calculator");
var calc = new Calculator();
<br />
console.log(calc.add(2, 2));
            </pre>
         </td>
      </tr>
   </tbody>
</table>

<table>
   <tbody>
       <tr>
         <th colspan="2">export ... (Named Export)</th>
      </tr>
      <tr>
         <th>calculator.ts</th>
         <th>compiled.js </th>
      </tr>
      <tr>
         <td>
            <pre lang="typescript">
export class Calculator {
    public add(num1, num2) {
        return num1 + num2;
    }
}              
            </pre>
         </td>
         <td>
            <pre lang="typescript">
exports.__esModule = true;
var Calculator = /** @class */ (function () {
    function Calculator() {}
    Calculator.prototype.add = function (num1, num2) {
        return num1 + num2;
    };
    return Calculator;
}());
exports.Calculator = Calculator;
            </pre>
         </td>
      </tr>
      <tr>
         <th colspan="2">import { ... } from "module"</th>
      </tr>
      <tr>
         <td colspan="2">
            <ul>
               <li>Named exports are useful to export several values.</li>
               <li>During the import, you must use the same name of the corresponding object.</li>
            </ul>
         </td>
      </tr>
      <tr>
         <th>importer.ts</th>
         <th>compiled.js </th>
      </tr>
      <tr>
         <td>
            <pre lang="typescript">
import { Calculator } from "./calculator";
<br />
let calc = new Calculator();
<br />
console.log(calc.add(2, 2));
            </pre>
         </td>
         <td>
            <pre lang="typescript">
exports.__esModule = true;
<br />
var calculator = require("./calculator");
var calc = new calculator.Calculator();
console.log(calc.add(2, 2));
            </pre>
         </td>
      </tr>
   </tbody>
</table>

#### Decorators ####
https://www.spectory.com/blog/A%20deep%20dive%20into%20TypeScript%20decorators

<table>
   <tr>
      <th colspan="2">What is Aspect-oriented Programing (AOP)?</th>
   </tr>
   <tr>
      <td colspan="2">
         AOP is a programming paradigm that aims to increase modularity by allowing the separation of cross-cutting concerns. It does so by adding additional behavior to existing code (an advice) without modifying the code itself.
<br />
<br />
         The biggest advantage of using AOP is the separation of irrelevant code from the main functionality of the method to a different place. The <b>best example is logging or authorization</b>. Instead of doing it inside the function which makes it clunky, we pull it out and only declare that the function is doing it. In addition, we can change it in a single place rather than in all the code base.</td>
   </tr>
   <tr>
      <th>Without AOP</th>
      <th>With AOP</th>
   </tr>
   <tr>
      <td><pre lang="typescript">
function add(x, y) {
    log("foo was called!");
<br />
    If(!validate(arguments)) {
        throw (...)
    }
<br />
    If(!authorized()) {
        throw ()
    }
<br />
    return x + y;
}
      </pre></td>
      <td><pre lang="typescript">
@log
@validate
@authorize
function add(x, y) {
return x + y;
}
      </pre></td>
   </tr>
</table>

<table>
   <tr>
      <th colspan="2">Decorators</th>
   </tr>
   <tr>
      <td colspan="2">A Decorator is a special kind of declaration that can be attached to a class declaration, method, accessor, property, or parameter. Decorators use the form @expression, where expression must evaluate to a function that will be called at runtime with information about the decorated declaration.
         <br />
         <br />
         https://www.spectory.com/blog/A%20deep%20dive%20into%20TypeScript%20decorators
      </td>
   </tr>
   <tr>
      <td>Class</td>
      <td><pre lang="typescript">
function classDecorator<T extends {new(...args:any[]):{}}>(constructor:T) {
    return class extends constructor {
        newProperty = "new property";
        hello = "override";
    }
}
<br />
@classDecorator
class Greeter {
    property = "property";
    hello: string;
    constructor(m: string) {
        this.hello = m;
    }
}
<br />
console.log(new Greeter("world"));
<br />
//printed to console :
//foo was called!
//{ hello: "override", newProperty: "new property", property: "property"  }
      </pre>
      </td>
   </tr>
   <tr>
      <td>Method</td>
      <td><pre lang="typescript">
function log(target, key, descriptor) {
    console.log(`${key} was called!`);
}
<br />
class P {
    @log
    foo() {
        console.log("Do something");
    }
}
<br />
console.log("Before instantiation");
const p = new P();
p.foo();
<br />
//printed to console :
//foo was called!
//Before instantiation
//Do something
<br />
//target:
//- The constructor function of the class for a static member
//- The prototype of the class for an instance member.
//key: The name of the member.
//descriptor: The Property Descriptor for the member.
      </pre>
      </td>
   </tr>
   <tr>
      <td>Property</td>
      <td><pre lang="typescript">
function calcCircleParams(target: any, key: string) {
    //Property value.
    let _val = this[key];
<br />
    //Property getter.
    const getter = function () {
        return _val;
    };
<br />
    //Property setter.
    const setter = function (newVal) {
        _val = newVal;
        this['Area'] = _val * _val * Math.PI;
        this['Circumference'] = 2 * _val * Math.PI;
    };
<br />
    //Delete property.
    if (delete this[key]) {
        // Create new property with getter and setter
        Object.defineProperty(target, key, {
            get: getter,
            set: setter,
            enumerable: true,
            configurable: true
        });
    }
}
<br />
class Circle {
    @calcCircleParams
    public Radius: Number;
    public Area: Number;
    public Circumference: Number;
    constructor() {
    }
}
<br />
let c = new Circle();
c.Radius = 3;
//Radius: 3, Area: 28.274333882308138, Circumference: 18.84955592153876
console.log(`Radius: ${c.Radius}, Area: ${c.Area}, Circumference: ${c.Circumference}`); 
<br />
c.Radius = 5;
//Radius: 5, Area: 78.53981633974483, Circumference: 31.41592653589793
console.log(`Radius: ${c.Radius}, Area: ${c.Area}, Circumference: ${c.Circumference}`); 
      </pre>
      </td>
   </tr>
   <tr>
      <td>Parameter</td>
      <td><pre lang="typescript">
function required(target: any, key: string, index: number) {
    var metadataKey = `__required_${key}_parameters`;
    if (Array.isArray(target[metadataKey])) {
        target[metadataKey].push(index);
    }
    else {
        target[metadataKey] = [index];
    }
}
<br />
function validate(target, key, descriptor) {
    var originalMethod = descriptor.value;
    descriptor.value = function (...args: any[]) {
        var metadataKey = `__required_${key}_parameters`;
        var indices = target[metadataKey];
        for (var i = 0; i < args.length; i++) {
            if (arguments[i] == undefined) {
                throw 'missing required parameter'
            }
        }
        var result = originalMethod.apply(this, args);
        return result;
    }
    return descriptor;
}
<br />
class Calculator {
    @validate
    add(@required a: number, @required b: number) {
        return a + b;
    }
}
<br />
const c = new Calculator();
console.log(`result is: ${c.add(2, 3)}`); //result is: 5
console.log(`result is: ${c.add(2, undefined)}`); //throw 'missing required parameter'
      </pre>
      </td>
   </tr>
</table>
