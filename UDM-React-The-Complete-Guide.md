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
* Why components?
  * RE-using
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
* Stateless (Function) vs Stateful (Class) component

![class-vs-functional](https://user-images.githubusercontent.com/5309726/58560244-5321cb00-8257-11e9-9594-772cf2fd5eac.png)

* Side effects likes sending HTTP request
![component-lifecycle](https://user-images.githubusercontent.com/5309726/58560403-a136ce80-8257-11e9-8b43-93fccc5327bd.png)

![component-lifecycle](https://user-images.githubusercontent.com/5309726/58565701-bdd80400-8261-11e9-8f8b-60562db53794.png)

* `return () => { xxx };` inside `useEffect` in functional component as cleanup
* `shouldComponentUpdate` for class component optimization (Can use React.PureComponent if doing full props comparison)
* `React.memo` for functional component optimization
  * Pass length to child component instead of array reference
* `React.Fragment`
* High order function (HOC), a component that wraps other components that add something (styling, addtional HTML / logic)
* Use `prevState` to update the state that is depending the old state
* 2 ways to use Ref in class component
  * `React.createRef()`
  * Passing function, `ref={(inputElement) => {this.inputElement = inputElement}}`
* Use `useRef` in functional component
* Use context (`createContext`) to bypassing components and directly passing data from A to D
  * Provider and Consumer pattern (way 1)
  * `useContext`, use in functional component (way 2)
  * `static contextType = xxxContext;`, use in class component (way 2)

#### Episode 8 ####
* Prop type validation
* Why sometimes avoid to use PureComponent because we don't want to check all the props

#### Episode 9 & 10 ####
* Axios
* Interceptors in Axios
* 2 ways to configure Axios
  * Default global configuration (`axios.defaults.baseURL = "https://xxx.com"`)
  * Axios instances (`axios.create({baseURL: "https://xxx.com"})`)
* When using high order component, may consider implement the `componentWillUnmount` method to prevent memory leak.

#### Episode 11 ####
* `exact` keyword -> is my complete path like this?
* `<a>` tag will refresh the page and lost the state after user clicked.
* `react-router` will pass additional props to the 1st lv component but not component tree.
* 2 ways to pass additional router props to component tree
  * `withRouter`
  * `...props`
* Can use `this.props.match.url` to build relative path
* `NavLink` same as `Link` but added an active class.
* Passing route parameters, `/:id`
* `Switch` tell react-router please only load one of the route. (The first one actually you find that matches from a given set of routes).

```
Option 1:
<Switch>
    <Route path="/" exact component={Posts} />
    <Route path="/new-post" component={NewPost} />
    <Route path="/:id" exact component={FullPost} />
</Switch>

Option 2:
<Route path="/" exact component={Posts} />
<Switch>
    <Route path="/new-post" component={NewPost} />
    <Route path="/:id" exact component={FullPost} />
</Switch>
```

* Navigation is a stack of pages
* Different ways navigate to other page
  * `Link` (way 1, push)
    * `this.props.history.push("'/' + id");`
    * `this.props.history.push({pathname: '/' + id});`
  * `<Redirect from="/" to="/posts" />` (way 2, replace)
    * Can be used as conditional redirects
    * `this.props.history.replace("/posts");`
* Unknown route
  * Without specify the `path`
  * `<Route render={() => <h1>Not found</h1>}/>`
* Code splitting
  * `import("./math").then(math => { console.log(math.add(16, 26)); });` (way 1)
  * `const OtherComponent = React.lazy(() => import('./OtherComponent'));` (way 2)
* Remember to configure production server to always load the `index.html`
  * If user try to send navigate request (http://hello.com/movies) to the server, server does not know the requested path other than `index.html` or `/` at first.
* `<BrowserRouter basename="" />`

#### Episode 12 ####
* `WithRouter` gives access to match and location 
* `this.props.history.push({ pathname: "/checkout", search: "?" + queryString })`

#### Episode 14 ####
![complex-state](https://user-images.githubusercontent.com/5309726/64627422-32563d80-d422-11e9-85c9-406d1e3a7989.png)
