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


