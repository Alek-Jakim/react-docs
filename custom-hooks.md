# Custom React Hooks

### 1. `usePageBottom` Hook

Lets you know when the user has scrolled to the bottom of a page.

```jsx
export function usePageBottom() {

    const [bottom, setBottom] = useState(false);

    useEffect(() => {

        function handleScroll() {
            const isBottom = window.innerHeight + document.documentElement.scrollTop === document.documentElement.offsetHeight;
            setBottom(isBottom);
        }
        window.addEventListener("scroll", handleScroll);

        return () => {
            window.removeEventListener("scroll", handleScroll);
        }

    }, []);

    return bottom
}
```

---

### 2. `useWindowSize` Hook

Gives you the current size (width) of the page (useful for media queries).

```jsx
export function useWindowSize() {

    // Check if it's sever-side, in which case window object doesn't exist
    const isSSR = typeof window === "undefined";

    const [windowSize, setWindowSize] = useState({
        width: isSSR ? 1200 : window.innerWidth,
        height: isSSR ? 800 : window.innerHeight
    });

    const changeWindowSize = () => {
        setWindowSize({ width: window.innerWidth, height: window.innerHeight });
    }

    useEffect(() => {
        window.addEventListener("resize", changeWindowSize);

        // Remove resize listener when component unmounts
        return () => {
            window.removeEventListener("resize", changeWindowSize)
        }
    }, []);

    return windowSize;
}
```

---

### 3. `useDeviceDetect` Hook

Detects whether the user uses a mobile device or not.

```jsx
export function useDeviceDetect() {

    const [isMobile, setIsMobile] = useState(false);

    useEffect(() => {
        const userAgent = typeof navigator === "undefined" ? "" : navigator.userAgent;

        const mobile = Boolean(userAgent.match(/Android|BlackBerry|iPhone|iPad|iPod|Opera Mini|IEMobile|WPDesktop/i));

        setIsMobile(mobile);
    }, []);

    return { isMobile }
}
```