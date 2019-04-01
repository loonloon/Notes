#### Episode 1 ####
* ES5, ES2015 - ES2019
* What is Babel?
* Import vs Require
* What is React Component?
  * A component is a Function / Class that produces HTML to show the user (using JSX) and handles feedback from the user (using Event Handlers)
  
#### Episode 2 ####
* Inline Styling with JSX
* Class vs ClassName

#### Episode 3 ####
* faker.js
* What is Props? 
  * Passing data from a parent component to a child component
  * Goal is to customize or configure a child component
 * Passing custom children
   * props.children

#### Episode 4 ####
* Functional vs Class components
* Benefits of Class components
  * Easier code organization
  * Can use 'state' -> Easer to handler user input
  * Understands lifecycle events -> Easier to do things when the app first starts
  
#### Episode 5 ####
* What is state?
  * Is a JS object that contains data relevant to a component
  * Only useable with class components
  * Must be initialized when a component is created
  * Can only be updated using the function setState
   * Updating 'state' on a component causes the component to re-render
 * Do not direct assign to this.state except the only time we do for initialize
 * Condition rendering
 
#### Episode 6 ####
* Component lifecycle
  * constructor (Good place to do one-time setup)
  * render (Avoid doing anything besides returning JSX)
    * Content visible on screen
  * componentDidMount (Good place to do data-loading!)
    * Sit and wait for updates
  * componentDiUpdate (Good place to do more data-loading when state/props change)
    * Sit and wait until this component is not longer shown (When this.setState is called)
  * componentWillUnmount (Good place to do cleanup, especially for non-React stuff)
* Other lifecycle methods (rarely used)
  * shouldComponentUpdate
  * getDerivedStateFromProps
  * getSnapshotBeforeUpdate
* defaultProps
* Avoid conditional in render function, instead create helper function

#### Episode 7 ####
* Controlled vs Uncontrolled component
  * In a controlled component, form data is handled by (store in) a React component. The alternative is uncontrolled components, where form data is handled by (store in) the DOM itself.
* What is 'this' used for in a class?
  * https://stackoverflow.com/questions/20279484/how-to-access-the-correct-this-inside-a-callback
 * How to solve 'this' content?
   * Use constructor and bind it
   * Arrow function
 * Communicating child to parent

#### Episode 8 ####
* Use 'axios' or 'fetch'
* Use 'async await' or 'then'
* Use 'map' to iterate array
* https://unsplash.com/developers

#### Episode 9 ####
* A 'key' is a special string attribute you need to include when creating lists of elements. 'key' help React identify which items have changed, are added, or are removed.

#### Episode 10 ####
* React refs
* use 'addEventListener' to wait DOM loaded.

#### Episode 12 ####
* What is Redux?
  * State management library
  * Not explicitly designed to work React
* Redux Cycle
  * (Person dropping off the form) Action Creator, produces an
  * (the form) Action, gets fed to
  * (form receiver) dispatch, forwards the action to
  * (Departments) Reducers, create news
  * (Compiled department data) State 

#### Episode 13 ####
* How React-Redux works?
* Named vs Default Export
* Higher-Order Functions
