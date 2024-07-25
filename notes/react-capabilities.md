### React Router

React Router is a library for handling routing in React applications.

<h4>React Router Setup:</h4>

1. Install React Router:  
   `npm install react-router-dom`

2. Basic Setup:

- `BrowserRouter`, `Routes`, `Route`, `Link` - Import components.
  `import { BrowserRouter, Routes, Route, Link } from "react-router-dom";`

- `<BrowserRouter />`
  Whole application is wrapped in `<BrowserRouter />` to provide routing capabilities. It also creates a Context.

- `Routes` and `Route` for defining routes.
  Inside the `<BrowserRouter />` we define routes using `Routes` and `Route`.  
  `Routes` wraps multiple `Route`.
  `Route` defines a path. A component will be rendered when the path matches.

- `Link` for navigation.
  Creates navigation links.  
  They work like anchor tags (`a`) but prevent default browser behavior of reloading the page.

3. Example:

```js
import { BrowserRouter, Routes, Route, Link } from "react-router-dom";

const App = () => {
  return (
    <BrowserRouter>
      <header>
        <Link to="/">Adopt Me!</Link>
      </header>
      <Routes>
        <Route path="/details/:id" element={<Details />} />
        <Route path="/" element={<SearchParams />} />
      </Routes>
    </BrowserRouter>
  );
};
```

### useParams

`useParams` hook is part of `react-router-dom` and allows us to access the paraneters of the current route.

- BrowserRouter creates a global context that holds the current location, navigation methods, and other routing information.
- Components like `Route`, `Link`, and hooks like `useParams`, `useNavigate`, and `useLocation` are designed to access this global context. This allows these components and hooks to interact with the routing state without needing to pass 'props' manually through each level of your component tree.

```js
// Create routing context

import { BrowserRouter, Routes, Router, Link } from "react-router-dom";
import Details from "./Details";

const App = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/details/:id" element={<Details />} />
      </Routes>
    </BrowserRouter>
  );
};
```

```js
// Use Routing context
import { useParams } from "react-router-dom";

const Details = () => {
  const { id } = useParams();

  return (
    <div>
      <h2>Details for Pet ID: {id}</h2>
    </div>
  );
};
```

### React Query

Minimize effects.  
Create small, testable effects.  
Use libraries.

Install React Query:  
`npm install @tanstack/react-query`

Import:  
`import { QueryClient, QueryClientProvider } from "@tanstack/react-query";`

Note: React Query cache is stored in-memory.

Manage and cache queries:

```js
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: Infinity,
      cacheTime: Infinity,
    },
  },
});
```

Wrap the app:

```js
<BrowserRouter>
  <QueryClientProvider client={queryClient}>
    <header>
      <Link to="/">Adopt Me!</Link>
    </header>
    <Routes>
      <Route path="/details/:id" element={<Details />} />
      <Route path="/" element={<SearchParams />} />
    </Routes>
  </QueryClientProvider>
</BrowserRouter>
```

Usage:

`useQuery(['details', id], fetchPet);` - `useQuery` is passed as the queryKey to fetchPet. The second argument is the default, if the value doesn't exist in a storage.

`isLoading` is for the first load. `isFetching` is for refetching.  
...........................

React Query is a data-fetching and state-management library.

<b>React Query Setup:</b>

1. `npm install @tanstack/react-query`
   `npm install @tanstack/react-query-devtools`

2. `import { QueryClient, QueryClientProvider } from "@tanstack/react-query"`
   `import { ReactQueryDevtolls } from "@tanstack/react-query-devtools"`

- It simplifies fetching, caching, synchronizing and updating server state in the React Components.
- React Query provides hooks to handle asynchronous data, manage cache, and keep UI in sync with server.

3. `new QueryClient()` - Create `QueryClient` instance. This instance is used to configure global settings for React Query, such as caching and query behavior.

4. Wrap the whole application in `QueryClientProvider` to make React Query available throught the application.  
   This allows React Query to manage queries and mutations globally.

```js
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

const queryClient = new QueryClient();

const App = () => {
  return (
    <QueryClientProvider client={queryClient}>
      <h1>Hi!</h1>
    </QueryClientProvider>
  );
};
```

We can set default parameters for the Query Client:

```js
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      // Duration (in milliseconds) that the data remains fresh.
      // During this time, React Query will not trigger refetching unless manually invalidated.
      staleTime: Infinity,
      // Duration (in milliseconds) that the cached data remains in memory after the query is no longer in use.
      // After this period, the cached data will be garbage collected.
      cacheTime: Infinity,
    },
  },
});
```

- Queries are functions that fetch data and return a promise. Queries are cached.
- Mutations are used for creating, updating, or deleting data on server. Mutations are not cached

- `useQuery` Fetches and manages data. Handles loading, error, and success states.
- `useMutation` Manages mutations like creating or updating data. Handles loading, error, and success states for mutations.
- `useQueryClient` Provides access to the query client for querying and invalidating queries.

<b>useQuery</b>

```js
const fetchUser = async () => {
  const response = await fetch("https://api.example.com/user");
  if (!response.ok) throw new Error("Network response was not ok");
  return response.json();
};

const { data, error, isLoading } = useQuery(["user"], fetchUser);
```

<b>useMutation</b>

Manages mutations such as creating, updating, or deleting data.

```js
const mutation = useMutation(addPost, {
  onSuccess: () => {
    // Optionally refetch or invalidate queries
  },
});
```

<b>useQueryClient</b>

Provides access to the `QueryClient` instance.

```js
const queryClient = useQueryClient();

const invalidateQueries = () => {
  queryClient.invalidateQueries(["posts"]);
};
```

### Class Components

- Class Components must extend Component.
- Every Class Component has a `render` method.
- `this.props` - Class components can recieve props (immutable).
- `this.state` - Class components can hold state.
- `state = {}` - used for similar goal as `useState` hook in Functional Components.
- `static defaultProps()` - default props, if it was not passed when the component was created.
- `componentDidMount` - Runs when component is first rendered (mounted).
- `componentDidUpdate` - Runs on every change within a component.
- `componentWillUnmount` - Runs every time component unmounts.
- `componentDidCatch` - For error handling.
