* The JavaScript engine actually compiles the program on the fly and then immediately runs the compiled code.
* Object property access
  * `obj.a`
  * `obj["a"]`
* Equality
  * == (loose-equals)
  * === (strict-equals)
* if you use the `==` loose-equals operator to make the comparison `"99.99" == 99.99`, JavaScript will convert the left-hand side "99.99" to its number equivalent 99.99. The comparison then becomes `99.99 == 99.99`, which is of course true.
* JavaScript uses dynamic typing.

<table>
 <tr>
   <th>Static typing</th>
  <th>Dynamic typing (Weak typing)</th>
 </tr>
 <tr>
  <td>Declare a variable (container) to hold a specific type of value such as number or string.</td>
  <td>Allows a variable to hold any type of value at any time.</td>
 </tr>
  <tr>
  <td></td>
  <td></td>
 </tr>
</table>

* The first `console.log(...)` command has to <strong>implicitly</strong> coerce that `number` value to a `string` to print it out.
* By convention, JavaScript variables as constants are usually capitalized, with underscores _ between multiple words.
* The specific list of "falsy" values in JavaScript is as follows:
  * "" (empty string)
  * 0, -0, NaN (invalid number)
  * null, undefined
  * false
* `==` checks for value equality with coercion allowed, and `===` checks for value equality without allowing coercion.
* Help you know whether to use `==` or `===` in various situations:
  * If either value (aka side) in a comparison could be the true or false value, avoid `==` and use `===`.
  * If either value in a comparison could be of these specific values (0, "", or []—empty array), avoid `==` and use `===`.
  * In all other cases, you’re safe to use `==`. Not only is it safe, but in many cases it simplifies your code in a way that improves readability.
* Take special note of the `==` and `===` comparsion rules if you're comparing two non-primitive values, like `objects` (including `function` and `array`). Because those values are actually <strong>held by reference</strong>, both `==` and `===` comparisons will simply check whether the references match, not anything about the underlying values.

```
var a = [1,2,3];
var b = [1,2,3];
var c = "1,2,3";

a == c; // true
b == c; // true
a == b; // false
```
* If both values in the `<` comparison are strings, as it is with `b < c`, the comparison is made lexicographically (aka alphabetically like a dictionary).
* If one or both is not a string, as it is with `a < b`, then both values are coerced to be numbers, and a typical numeric comparison occurs.

```
var a = 41;
var b = "42";
var c = "43";

a < b; // true
b < c; // true
```

* Certain words cannot be used as variables, but are OK as property names. These words are called "reserved words," and include the JS keywords (for, in, if, etc.) as well as null, true, and false.
* Use the `var` keyword to <strong>declare a variable that will belong to the current function scope, or the global scope if at the top level outside of any function.</strong>

```
// inner can access outer
// outer cant access inner
function foo() {
 var a = 1;
 
 function bar() {
     var b = 2;
     
     function baz() {
         var c = 3;
         console.log( a, b, c ); // 1 2 3
     }
     
     baz();
     console.log( a, b ); // 1 2
 }
 
 bar();
 console.log( a ); // 1
}

foo();
```

* Can opt in to strict mode for an individual function, or an entire file.

```
function foo() {
    "use strict";
    
    // this code is strict mode
    function bar() {
        // this code is strict mode
    }
}
// this code is not strict mode

"use strict";

function foo() {  
    // this code is strict mode
    
    function bar() {
        // this code is strict mode
    }
}
// this code is strict mode
```

* immediately invoked function
* The outer `(...)` that surrounds the `(function IIFE() {..})` function expression.

```
var a = 42;

(function IIFE(){
    var a = 10;
    console.log( a ); // 10
})();

console.log( a );
```

* Closure as a way to “remember” and continue to access a function’s scope (its variables) even once the function has finished running.

```
function makeAdder(x) {
// parameter `x` is an inner variable
// inner function `add()` uses `x`, so
// it has a "closure" over it

   function add(y) {
       return y + x;
   };
   
   return add;
}

var plusOne = makeAdder( 1 );
var plusTen = makeAdder( 10 );

plusOne( 3 ); // 4 <-- 1 + 3
plusOne( 41 ); // 42 <-- 1 + 41
plusTen( 13 ); // 23 <-- 10 + 13
```

* The most common usage of closure in JavaScript is the module pattern.

```
function User(){
    var username, password;
    
    function doLogin(user, pw) {
        username = user;
        password = pw;
        
        // do the rest of the login work
    }

var publicAPI = {
    login: doLogin
};

return publicAPI;
}

// create a `User` module instance
var fred = User();
fred.login( "fred", "12Battery34!" );
```

* If a function has a `this` reference inside it, that `this` reference usually points to an `object`. But which `object` it points to depends on how the function was called.

```
function foo() {
   console.log( this.bar );
}

var bar = "global";

var obj1 = {
    bar: "obj1",
    foo: foo
};

var obj2 = {
    bar: "obj2"
};

foo(); // "global"
obj1.foo(); // "obj1"
foo.call( obj2 ); // "obj2"
new foo(); // undefined
```

* There are four rules for how this gets set
  * `foo()` ends up setting `this` to the global object in non-strict mode—in strict mode, `this` would be `undefined` and you'd get an error in accessing the bar property—so `"global"` is the value found for `this.bar`.
  * `obj1.foo()` sets `this` to the `obj1` object.
  * `foo.call(obj2)` sets `this` to the `obj2` object.
  * `new foo()` sets `this` to a brand new empty object.
  
* The prototype mechanism in JavaScript is quite complicated.
* The latter is more appropriately named "behavior delegation."
* The `a` property doesn’t actually exist on the `bar` object, but because `bar` is prototype-linked to `foo`, JavaScript automatically falls back to looking for `a` on the `foo` object, where it’s found.

```
var foo = {
    a: 42
};

// create `bar` and link it to `foo`
var bar = Object.create( foo );
bar.b = "hello world";
bar.b; // "hello world"
bar.a; // 42 <-- delegated to `foo`
```

* There are two main techniques you can use to "bring" the newer JavaScript stuff to the older browsers: <strong>polyfilling</strong> and <strong>transpiling</strong>.
  * polyfill, taking the definition of a newer feature and producing a piece of code that's equivalent to the behavior, but is able to run in older JS environments.
  * transpiling, converts your newer code into older code equivalents. (transforming + compiling)
