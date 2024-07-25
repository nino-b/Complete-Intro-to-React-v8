### Hooks

- Cant create hooks conditionally.
- Hooks must be created on every single render and in the same order!.

```js
// Short version
const [location, setLocation] = useState("Seattle, WA");

// Long version
const locationHook = useState("");
const location = locationHook[0]; // Initial State
const setLocation = locationHook[1]; // Function that updates the state
```

- If we want to do async / await of an effect, create async function inside of the effect.
- Recommendation: When creating a custom hook, track status. Makes easier to test.

### StrictMode

- `<StrictMode>` is a component in React that helps developers identify potential problems in an application.
- It doesn't render any visible UI, but it activates additional checks and warnings for its descendants.
- This makes it easier to find and fix common issues such as legacy API usage, unexpected side effects, and unsafe lifecycle methods.
