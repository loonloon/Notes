#### Episode 1 ####
* What is React?
  * A JavaScript library for building User Interfaces
* Why should we choose React?
  * UI State becomes difficult to handle with JavaScript
  * High performance
* React alternatives
  * Angular
  * React
  * Vue
* Understand Single Page Applications (SPA)

<table>
  <tr>
    <th>Single Page Applications</th>
    <th>Multiple Page Applications</th>
  </tr>
  <tr>
    <td>Only ONE HTML page, content is (re)rendered on client</td>
    <td>Multiple HTML pages, content is rendered on server</td>
  </tr>
  <tr>
    <td>Typically only ONE ReactDOM.render() call</td>
    <td>One ReactDOM.render() call per widget</td>
  </tr>
</table>

#### Episode 2 ####
* let vs const
* arrow functions
* exports vs imports (modules)

<table>
  <tr>
    <th>default export</th>
    <th>named export</th>
  </tr>
  <tr>
    <td>export default person</td>
    <td>export const baseData = 10</td>
  </tr>
  <tr>
    <td>imports default and ONLY export of the file. Name in the receiving file is up to you</td>
    <td>Use exact name defined in the file with curly braces or assign an alias</td>
  </tr>
</table>

* Classes, Properties and Methods
* Spread vs Rest operators

<table>
  <tr>
    <th>Spread</th>
    <th>Rest</th>
  </tr>
  <tr>
    <td>Used to split up array elements OR object properties</td>
    <td>Used to merge a list of function arguments into an array</td>
  </tr>
  <tr>
    <td>
     <pre lang="typescript">
const newArray = [...oldArray, 1, 2]
const newObject = {...oldObject, newProp: 5}
     </pre>
   </td>
    <td>
     <pre lang="typescript">
function sortArgs(...args) {
   return args.sort();
}
     </pre>
   </td>
  </tr>
</table>

* Destructuring
  * Easily extract array elements or object properties and store them in variables  

<table>
  <tr>
    <th>Array Destructuring</th>
    <th>Object Destructuring</th>
  </tr>
  <tr>
    <td>
     <pre lang="typescript">
[a, b] = ['Hello', 'Max'];
console.log(a); // Hello
console.log(b); // Max
     </pre>
   </td>
    <td>
     <pre lang="typescript">
{ name } = { name: 'Max', age: 29 };
console.log(name) // Max
console.log(age) // undefined
     </pre>
   </td>
  </tr>
</table>

* Objects and arrays are reference type, if you reassign them, you are copying the pointer not the value

#### Episode 3 ####
![build-workflow](https://user-images.githubusercontent.com/5309726/57570910-d52d8980-7439-11e9-8b9d-2adf45e7afdb.png)

* Understanding JSX
  * JSX will being compile from `<div>` to `React.CreateElement('<div>', null, '');`
* Why components? RE-using
* Outputting dynamic content
  * pass value from parent to child components
    * `props`, access the attribute in child that pass from the parent component
    * `props.children`, content in between the opening and closing tag either text or html
 * Understand and using state
   * Change the state will re-render and update the DOM
* `useState`, will replace instead of merge
* stateful vs stateless
* Passing function references to components
* Function binding
  * `onClick={() => printName("Max")}`, arrow function (NOT EFFICIENT)
  * `onClick={printName.bind(this, "Max")}`, bind in render
  * `this.printName = this.printName.bind(this);`, bind in constructor (BEST)

#### Episode 4 ####
* Render content conditionally
* Update array objects

```
nameChangedHandler = (event, id) => {
  const personIndex = this.state.persons.findIndex(p => p.id === id);
  
  //clone person object
  const person = { ...this.state.persons[personIndex] };
  person.name = event.target.value;

  const persons = [...this.state.persons];
  persons[personIndex] = person;

  this.setState({ persons });
};

<Person key={person.id} changed={e => this.nameChangedHandler(e, person.id)} />
```

* Unique KEY for list

#### Episode 5 ####
* Styles dynamically
* Radium (CSS selector in JSX)
* Webpack CSS module (CSS from file as object)
  * css-loader, localIdentName
* CSS pseudo selectors

#### Episode 6 ####
* Error Boundaries (ONLY working on production mode)

#### Episode 7 ####
