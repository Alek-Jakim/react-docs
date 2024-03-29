# Practical React Stuff

### **Fetch data from an API**

```jsx
import React, { useEffect, useState } from "react"

const Foo = () => {
    // State to hold the data we fetch
    const [users, setUsers] = useState([]);

    const fetchData = async () => {
        const data = await fetch("https://random-data-api.com/users/random_user?size=10");

        const response = await data.json();

        /* Get only the fields you need

        let alteredUserData = await response.map((user) => {
            return {
                name: user.name,
                email: user.email,
                password: user.password
            }
        });
        */
        setUsers(response);
    }

    useEffect(() => {
        //Fetch data when component first initialized
        fetchData();
    }, []);

    return (
        <div>
            {
                !users ? <div>Loading users...</div> : users.map((user, idx) => (
                    <div key={idx}>
                        {/*display user data*/}
                    </div>
                ))
            }
        </div>
    )
}

export default Foo
```

---

### `useMemo` in Practice

```jsx
import React, { useMemo, useState } from 'react'

const Calculate = () => {
    const [number, setNumber] = useState(0);
    const [dark, setDark] = useState(false);
    const themeStyles = {
        backgroundColor: dark ? "black" : "white",
        color: dark ? "white" : "black"
    }

    // By using useMemo, we only get a slow operation when changing number, other operations such as changeTheme are not affected
    const doubleNumber = useMemo(() => {
        return slowFunction(number);
    }, [number]);

    return (
        <div>
            <input type="number" value={number} onChange={(e) => setNumber(parseInt(e.target.value))} />
            <button onClick={() => setDark(prevDark => !prevDark)}>Change Theme</button>
            <div style={themeStyles}>{doubleNumber}</div>
        </div>
    )
}

// this is an expensive function that slows down the whole app
const slowFunction = (num) => {
    console.log("Calling slow function");
    for (let i = 0; i < 1000000000; i++) {}
    return num * 2
}

export default Calculate
```
---

### Dynamically Add Child Components

```jsx
function Child() {
  return <div>This is children content</div>;
}

function Parent({children}) {
  return (
    <div>
      <h3>Parent Component</h3>
      {children}
    </div>
  );
}

function App() {
  return (
    <Parent>
      <Child />
    </Parent>
  );
}
```

---

### Splash Raw JSON on Screen

```jsx
    const url = "https://random-data-api.com/api/users/random_user?size=10"

    const [coffeesJSON, setCoffeesJSON] = useState("");

    const fetchCoffeeJSON = () => {
        return fetch(url)
            .then((data) => {
                console.log(data);
                return data.json();
            })
            .catch((err) => {
                console.error(err);
            })
    }

    useEffect(() => {
        fetchCoffeeJSON().then((randomData) => setCoffeesJSON(JSON.stringify(randomData, null, 2)))
    }, [])

    return(
        <div>
            <pre>{coffeesJSON}</pre>
        </div>
    )
```

---

### Load Next Page API

```jsx
    const [userData, setUserData] = useState([]);
    const [counter, setCounter] = useState(1);

    const fetchUserData = async () => {
        setCounter(counter => counter + 1);
        let url = `https://random-data-api.com/api/users/random_user?size=3?page=${counter}`
        console.log(url)
        const response = await fetch(url, {
            method: "GET",
            headers: {
                "Content-Type": "application/json"
            }
        });

        if (response.ok) {
            const users = await response.json();
            console.log(users);
            if (userData.length === 0) {
                setUserData(users);
            } else {
                let updatedArr = [...userData, ...users]
                setUserData(updatedArr)
            }
        }
    }

    useEffect(() => {
        fetchUserData();
        // Note: here you might get an eslint warning that you need to include fetchUserData in the dependencies, which would cause infinite re-rendering

        //To solve this use useRef as below
    }, []);
```

---

### `useRef` for including fetch function into `useEffect` dependencies

```jsx
        const [userData, setUserData] = useState([]);
    const [counter, setCounter] = useState(1);

    const fetchUserData = useRef(() => { });

    fetchUserData.current = async () => {
        setCounter(counter => counter + 1);
        let url = `https://random-data-api.com/api/users/random_user?size=3?page=${counter}`
        console.log(url)
        const response = await fetch(url, {
            method: "GET",
            headers: {
                "Content-Type": "application/json"
            }
        });

        if (response.ok) {
            const users = await response.json();
            console.log(users);
            if (userData.length === 0) {
                setUserData(users);
            } else {
                let updatedArr = [...userData, ...users]
                setUserData(updatedArr)
            }
        }
    }

    useEffect(() => {
        fetchUserData.current();
    }, [fetchUserData]);
```

---

### React Countdown Timer

```jsx
import * as React from "react";
import { render } from "react-dom";

import "./styles.css";

function App() {
  const [counter, setCounter] = React.useState(60);

  // First Attempts
  // setInterval(() => setCounter(counter - 1), 1000);

  // Second Attempts
  // React.useEffect(() => {
  //   counter > 0 && setInterval(() => setCounter(counter - 1), 1000);
  // }, []);

  // Second Attempts - Inspection
  // React.useEffect(() => {
  //   counter > 0 &&
  //     setInterval(() => {
  //       console.log(counter);
  //       setCounter(counter - 1);
  //     }, 1000);
  // }, []);

  // Third Attempts
  // React.useEffect(() => {
  //   const timer =
  //     counter > 0 && setInterval(() => setCounter(counter - 1), 1000);
  //   return () => clearInterval(timer);
  // }, [counter]);

  // Suggested by Laurent
  React.useEffect(() => {
    counter > 0 && setTimeout(() => setCounter(counter - 1), 1000);
  }, [counter]);

  return (
    <div className="App">
      <div>Countdown: {counter}</div>
    </div>
  );
}

const rootElement = document.getElementById("root");
render(<App />, rootElement);
```