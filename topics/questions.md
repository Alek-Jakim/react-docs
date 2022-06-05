# React Q&A

### 1. What is state in React ?

* State of a component is an object that holds some information that may change over the lifetime of the component. We should always try to make our state as simple as possible and minimize the number of stateful components.

### 2a. What are props in React?

* Props are inputs to components. They are single values or objects containing a set of values that are passed to components on creation using a naming convention similar to HTML-tag attributes. They are data passed down from a parent component to a child component.


### 2b. What is the difference between state and props?

States are values we can read and update in our React components.

Props are values that are passed to React components and are read only (they should not be updated).


### 3. Why should we not update the state directly?

If you try to update the state directly then it won't re-render the component.

Instead use `setState()` method. It schedules an update to a component's state object. When state changes, the component responds by re-rendering.

### 4. How to bind methods or event handlers in JSX callbacks?

i. Binding in Constructor

```jsx
class Foo extends Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() {
    console.log('Click happened');
  }
  render() {
    return <button onClick={this.handleClick}>Click Me</button>;
  }
}
```

ii. Public class fields syntax

```jsx
handleClick = () => {
  console.log('this is:', this)
}
```

iii. Arrow functions in callbacks

```jsx
handleClick() {
    console.log('Click happened');
}
render() {
    return <button onClick={() => this.handleClick()}>Click Me</button>;
}
```

### 5. What is the purpose of callback function as an argument of `setState()`?

The callback function is invoked when `setState` finished and the component gets rendered. Since `setState()` is asynchronous the callback function is used for any post action.

Note: It is recommended to use lifecycle method rather than this callback function.

```jsx
setState({ name: 'John' }, () => console.log('The name has updated and component re-rendered'))
```


### 6. What is Lifting State Up in React?

When several components need to share the same changing data then it is recommended to lift the shared state up to their closest common ancestor. That means if two child components share the same data from its parent, then move the state to parent instead of maintaining local state in both of the child components.

### 7. What are Higher-Order Components?

A **higher-order component** (HOC) is a function that takes a component and returns a new component. Basically, it's a pattern that is derived from React's compositional nature.

We call them **pure components** because they can accept any dynamically provided child component but they won't modify or copy any behavior from their input components.


### 8. What are portals in React?

Portal is a recommended way to render children into a DOM node that exists outside the DOM hierarchy of the parent component.

```jsx
ReactDOM.createPortal(child, container)
//The first argument is any render-able React child, such as an element, string, or fragment. The second argument is a DOM element.
```

### 9. What are the differences between stateful and stateless components ?

If the behavior is independent of its state then it is a stateless component. Use function components.

If the behaviour of a component is dependent on the state of the component then it can be termed as stateful component. Always class components and have a state that gets initialized in the `constructor`.

```jsx
class App extends Component {
  constructor(props) {
    super(props)
    this.state = { count: 0 }
  }

  render() {
    // ...
  }
}
```

**React 16.8 Update**

Hooks let you use state and other React features without writing classes.

```jsx
import React, {useState} from 'react';

 const App = (props) => {
   const [count, setCount] = useState(0);

   return (
     // JSX
   )
 }
```

### 10. Why do we need keys for React lists?

Keys are a unique value that we must pass to the key prop when we are using the `.map()` function to loop over an element or a component.

```jsx
posts.map(post => <li key={post.id}>{post.title}</li>)
```

We need to add a key that is a unique value because keys tell React which element or component is which in a list. Keys are necessary for React to update the DOM (using Virtual DOM) properly.


### 11. Why can’t browsers read JSX?

Browsers can only read JavaScript objects but JSX in not a regular JavaScript object. Thus to enable a browser to read JSX, first, we need to transform JSX file into a JavaScript object using JSX transformers like Babel and then pass it to the browser.


