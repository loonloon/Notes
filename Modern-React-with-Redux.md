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
![component-lifecycle](https://user-images.githubusercontent.com/5309726/55407149-15266480-5590-11e9-9ffd-23df78476a45.png)

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
  ![redux-cycle-02](https://user-images.githubusercontent.com/5309726/55406621-04c1ba00-558f-11e9-8efd-7a3bc3d408e0.png)
  
![redux-cycle-01](https://user-images.githubusercontent.com/5309726/55406607-fbd0e880-558e-11e9-8269-fa5f9da0c980.png)

![redux-cycle-00](https://user-images.githubusercontent.com/5309726/55406662-1efb9800-558f-11e9-9b86-781baf95713a.png)

#### Episode 13 ####
* How React-Redux works?

![architecture](https://user-images.githubusercontent.com/5309726/55408691-1d33d380-5593-11e9-9079-f502d4535cf1.png)

* Named vs Default Export
* Higher-Order Functions

#### Episode 14 ####
* Data Loading with Redux
* Redux-Thunk (Asyncrhonous action creator)
 * Middleware to help us make requests in a redux application
 
![middleware](https://user-images.githubusercontent.com/5309726/55408670-10af7b00-5593-11e9-9084-eb6e8e496813.png)

* Behind the Scenes of Redux Thunk

![redux-thunk](https://user-images.githubusercontent.com/5309726/55565205-fd83e300-572b-11e9-8d9f-401819aeda0c.png)

#### Episode 15 ####
* Rules of reducers
  * Must return any value besides "undefined"
  * Produces "state" or data to be used inside of your app using only previous state and the action <br /><br />
  ![previous-state](https://user-images.githubusercontent.com/5309726/55568327-155e6580-5732-11e9-94f3-a866d5e6fc43.png)
  * Pure function or reducer only ever going to return values that use there it's input arguments
  ```
  export default (state, action) => {
   // bad
   return document.querySelector("#input");
 
   // bad
   return axios.get("/posts");
 
   //good
   return state + action (for loop / filtering)
  }
  ```
