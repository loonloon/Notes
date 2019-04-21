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
 * How to solve `this` content?
   * Use constructor and bind it
   * Arrow function
 * Communicating child to parent

#### Episode 8 ####
* Use `axios` or `fetch`
* Use `async await` or `then`
* Use `map` to iterate array
* https://unsplash.com/developers

#### Episode 9 ####
* A `key` is a special string attribute you need to include when creating lists of elements. `key` help React identify which items have changed, are added, or are removed.

#### Episode 10 ####
* React refs
* use `addEventListener` to wait DOM loaded.

#### Episode 12 ####
* What is Redux?
  * State management library
  * Not explicitly designed to work React
  ![redux-cycle-02](https://user-images.githubusercontent.com/5309726/55406621-04c1ba00-558f-11e9-8efd-7a3bc3d408e0.png)
  
![redux-cycle-01](https://user-images.githubusercontent.com/5309726/55406607-fbd0e880-558e-11e9-8269-fa5f9da0c980.png)

![redux-cycle-00](https://user-images.githubusercontent.com/5309726/55406662-1efb9800-558f-11e9-9b86-781baf95713a.png)

#### Episode 13 ####
* How React-Redux works?
* https://github.com/reduxjs/redux/issues/653

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
 
   // good
   return state + action (for loop / filtering)
  }
  ```
  * Do not mutate its input "state" argument (Do not modify the state reference, e.g. return new object / array will notify React re-render)
* Safe state update in Reducers

![bad-good](https://user-images.githubusercontent.com/5309726/55664357-d5e16780-585e-11e9-858a-00f961582cb6.png)

* Switch statement in Reducers
* Extracting logic to MapStateToProps
* Memorized function
* lodash library

#### Episode 16 ####
* Introducing React Router

![react-router](https://user-images.githubusercontent.com/5309726/55680695-8dee3d80-594f-11e9-8b8e-a2d6e8aa173c.png)

* How React Router Works

![react-router-care](https://user-images.githubusercontent.com/5309726/55680790-03a6d900-5951-11e9-98ef-37a2e04b8d41.png)

* How Paths Get Matched
  * is `/` in the Extracted path? -> YES, then display.
  * is `/page?` in the Extracted path? -> YES, then display.
  * is `/page/5?` in the Extracted path? -> NO, then no display.
  * If the particular route use `exact` keyword, then `===` logic will be used, else remain "contains" function.
![contains](https://user-images.githubusercontent.com/5309726/55681211-1f60ae00-5956-11e9-8125-dac77c3dc138.png)

* Do not use achor tag `<a>` to navigate, it will makes request to server return new index.html page and all the state will be lost.
 
 ![react-router-achor](https://user-images.githubusercontent.com/5309726/55684794-f0603180-5981-11e9-986a-c86e3fa49625.png)
 
* Use `Link` tag

![react-router-link](https://user-images.githubusercontent.com/5309726/55684769-ba22b200-5981-11e9-9f63-e9a4e7d4299d.png)

* Different router types
* https://www.techiediaries.com/react-router-dom-v4/

![router-types](https://user-images.githubusercontent.com/5309726/55726981-945de180-5a43-11e9-9e38-3d2c78daab82.png)

#### Episode 17 ####
* Google Auth

#### Episode 18 ####
* Redux DevTools

#### Episode 19 ####
* Redux Form

![without-redux-form](https://user-images.githubusercontent.com/5309726/56073342-c325e000-5dd4-11e9-8926-ae2fbf7446eb.png)

![redux-form](https://user-images.githubusercontent.com/5309726/56073348-cae58480-5dd4-11e9-8c9c-6ec6639c9869.png)

* Validation of Form Inputs
  * `Touched`
* `name` attribute in `Field` will looking matching fields in the object.
  * `{ title: 'My Book', age: '10' }`
  * `<Field name='title />`

#### Episode 20 ####
* JSON server
* Object based reducer
  * Key interpolation
* Intentional Navigation
  * User clicks on a `Link` component
* Programmatic Navigation
  * We run code to forcibly navigate the user through our app

![plain-router](https://user-images.githubusercontent.com/5309726/56373008-a9502700-6232-11e9-88fd-07c22fcd90f1.png)

* We can control the browser history object easily with plain router, instead of using Browser router and pass browser history to action method.
* Selection reducer vs URL based selection

![url-selection](https://user-images.githubusercontent.com/5309726/56411286-dd6b2c80-62b2-11e9-8925-f1268436d72d.png)

* Wildcard Navigation
  * /streams/edit<strong>/:id</strong>
* Route Params
  * /streams/edit<strong>/:anything/:others</strong>
* With React-Router, each component needs to be designed to work in isolation <strong>(fetch its own data!)</strong>.
* Pass object to React-Form by setting `initialValues` attribute.
  * Don't set entire object to `initialValues` attribute, set the necessary one, by 2 ways:
  * `{ title: 'My Book', description: '10' }`
  * `pick` function in lodash, `_pick(this.props.stream, 'title', 'description')`
  
![additional-props](https://user-images.githubusercontent.com/5309726/56418247-f1218d80-62c8-11e9-97a8-8ae8359f6ef3.png)

* PUT VS PATCH
  * PUT, update ALL properties of a record
  * PATCH, Update SOMR properties of a record
  
#### Episode 21 ####
* React Portals
  * provide a first-class way to <strong>render children into a DOM node that exists outside the DOM hierarchy of the parent component</strong>.
  
![react-portals](https://user-images.githubusercontent.com/5309726/56426271-415d1780-62ea-11e9-8250-4ea11046812d.png)

* Event Propagation (Default HTML / Javascript behaviour)
  * If trigger an event on some child element, and the child element does not handle the event. The event will bubble up to some parent element until it caught will event handler.
  * `e.stopPropagation()`
* `React.Fragment`
  * let you group a list of children without adding extra nodes to the DOM.
  * short hand, `<>...</>`

#### Episode 22 ####
* `Switch`,  returns only one first matching route.
