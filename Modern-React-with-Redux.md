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
