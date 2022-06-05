# React Hooks

Hooks are the new feature introduced in the React 16.8 version. It allows you to use state and other React features without writing a class. Hooks are the functions which "hook into" React state and lifecycle features from function components. It does not work inside classes.

### When to use hooks

If you write a function component, and then you want to add some state to it, previously you do this by converting it to a class. But, now you can do it by using a Hook inside the existing function component.

### Rules of Hooks

Hooks are similar to JavaScript functions, but you need to follow these two rules when using them:

1. Only call Hooks at the top level - Do not call Hooks inside loops, conditions, or nested functions

2. Only call Hooks from React functions - You cannot call Hooks from regular JS functions, only function components


### Most Important Hooks

## 1. `useState` Hook:

The purpose of this hook to handle reactive data, any data that changes in the application is called state, when any of the data changes, React re-renders the UI.

```jsx
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

## 2. `useEffect` Hook:

Allows you to perform side effects in your components. It allows us to implement all of the lifecycle hooks from within a single function API.

```jsx
// this will run when the component mounts and anytime the stateful data changes
React.useEffect(() => {
    alert('Hey, Alek here!');
});

// this will run, when the component is first initialized
React.useEffect(() => {
    alert('Hey, Alek here!');
}, []);

// this will run only when count state changes
React.useEffect(() => {
    fetch('alek').then(() => setLoaded(true));
}, [count]);

// this will run when the component is destroyed or before the component is removed from UI.
React.useEffect(() => {
    alert('Hey, Alek here');

    return () => alert('Goodbye Component');
});
```

## 3. `useContext` Hook:

Allows us to work with React's Context API, which itself a mechanism to allow us to share data within it's component tree without passing through props. It basically removes `prop-drilling`.


```jsx
const ans = {
    right: '✅',
    wrong: '❌'
}

const AnsContext = createContext(ans);

function Exam(props) {
    return (
        // Any child component inside this component can access the value which is sent.
        <AnsContext.Provider value={ans.right}>
            <RightAns />
        </AnsContext.Provider>
    )
}

function RightAns() {
    // it consumes value from the nearest parent provider.
    const ans = React.useContext(AnsContext);
    return <p>{ans}</p>
    // previously we were required to wrap up inside the AnsContext.Consumer
    // but this useContext hook, get rids that.
}
```

## 4. `useRef` Hook:

This hook allows us to create a mutable object. It is used, when the value keeps changes like in the case of `useState` hook, but the difference is, it doesn't trigger a re-render when the value changes.

The most common use case of this is to grab HTML elements from the DOM.

```jsx
function App() {
    const myBtn = React.useRef(null);
    const handleBtn = () => myBtn.current.click();
    return (
        <button ref={myBtn} onChange={handleBtn} >
        </button>
    )
}
```

## 5. `useReducer` Hook:

If works very similar to `useState`. It's a different way to manage state using `Redux Pattern`. Instead of updating the state directly, we dispatch actions, that go to a reducer function, and this function figure out, how to compute the next state.

```jsx
function reducer(state, dispatch) {
    switch(action.type) {
        case 'increment':
            return state+1;
        case 'decrement':
            return state-1;
        default:
            throw new Error();
    }
}

function useReducer() {
    // state is the state we want to show in the UI.
    const [state, dispatch] = React.useReducer(reducer, 0);

    return (
        <>
        Count : {state}
        <button onClick={() => dispatch({type:'decrement'})}>-</button>
        <button onClick={() => dispatch({type:'increment'})}>+</button>
        </>
    )
}
```

## 6. `useMemo` Hook: 

`useMemo` is very similar to `useCallback`. It accepts a function and a list of dependencies, but the difference between `useMemo` and `useCallback` is that `useMemo` returns the memo-ized value returned by the passed function. It only recalculates the value when one of the dependencies changes. It’s very useful if you want to avoid expensive calculations on every render when the returned value isn’t changing.

```jsx
function useMemo() {

    const [count, setCount] = React.useState(60);

    const expensiveCount = useMemo(() => {
        return count**2;
    }, [count]) // recompute when count changes.
}
```

## 7. `useCallback` Hook:

Returns a memorized callback when passed a function and a list of dependencies that set the parameters. Useful when a component is passing a callback to its child component in order to prevent rendering. It only changes the callback when one of its dependencies is changed.

```jsx
function useCallbackDemo() {
    const [count, setCount] = useState(60);

    const showCount = React.useCallback(() => {
        alert(`Count ${count}`);
    }, [count])

    return <> <SomeChild handler = {showCount} /> </>
}
```