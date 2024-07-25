# Setup and Tooling

'react' is the API part of the library.

```html
<script src="https://unpkg.com/react@18.2.0/umd/react.development.js"></script>
```

React DOM is the part that will render it to the DOM (the part that 'talks' to the DOM).

```html
<script src="https://unpkg.com/react-dom@18.2.0/umd/react-dom.development.js"></script>
```

### React Element

- A React Element is a plain object that describes what do we want to see on the screen.

- Immutable: Once we create a React Element, we can't change its children or attributes. This immutability makes the creation and rendering of React Elements fast.

- Creating a React Element using `React.createElement`:

```js
const element = React.createElement(
  "h1", // HTML element that should be rendered
  { className: "greeting" }, // HTML element attributes
  "Hello, world!", // Child element
);
```

- Creates an object like:

```js
{
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
}
```

- Creating a React Element with JSX:

```js
const element = <h1 className="greeting">Hello, world!</h1>;
```

### Rendering React Elements

React Elements are rendered into the DOM using the `ReactDOM.render` method:

```js
import React from "react";
import ReactDOM from "react-dom";

const element = <h1 className="greeting">Hello, world!</h1>;
ReactDom.render(element, document.getElementById("root"));
```

### React Elements and Components Together

```js
import React from "react";
import ReactDOM from "react-dom";

// React Component
const Greeting = (props) => {
  return <h1 className="greeting">Hello, {props.name}!</h1>;
};

// React Element
const element = <Greeting name="Alice" />;

ReactDOM.render(element, document.getElementById("root"));
```

### React Component

- React Component is a fundamental building block of React application.

- It is a JavaScript function or class that optionally accepts inputs (known as 'props') and returns a React Element that describes how a section of the UI should appear.

- Component names are always capitalized (!required).

- Basically, React Components are like JS classes, or templates (don't be confused with Class Components. I am making just an analogy of behavior. JS classes are like blueprints or templates, and so are React Components (there are Functional and Class components and they both behave as a template or a blueprint)). And React Elements are "instances" of that template or a blueprint. Like JS class instances, they are plain objects.

There are two types of React Components:

- Functional Components.
- Class Components.

### Functional Components

- These are JavaScript function that return React Elements.
- They can use hooks (like `useState`, `useEffect`) to manage state and side effects.

```js
const Greeting = ({ name }) => {
  return <h1>Hello, {name}!</h1>;
};
```

### Class Components

- ES6 classes that extend from `React.Component`.
- They have their own state and lifecycle methods.

```js
class Greeting extends React.component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

### Props

- Props: Short for properties.
- Props are read-only inputs passed to a component to configure it.

```js
<Greeting name="Alice" />
```

### State

- State is a way to store and manage data that changes over time within a component.
- In Functional Components we use `useState` hook to manage data.

```js
const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click Me</button>
    </div>
  );
};
```

### Lifecycle Methods

- Class Components have special methods that run at different points in the Component's life (like mounting, updating and unmounting).
- Functional Components achieve similar behavior using hooks like `useEffect`.

```js
class Timer extends React.Component {
  componentDidMount() {
    this.interval = setInterval(
      () => this.setState({ time: new Date() }),
      1000,
    );
  }
  componentWillUnmount() {
    clearInterval(this.interval);
  }
  render() {
    return <h1>{this.state.time.toLocaleTimeString()}</h1>;
  }
}
```

### Rendering React Component

To actually connect React Components with DOM:

- In HTML there is usually created an empty `<div>` tag with an id of 'root': `<div id="root"></div>`.

- That element is retrieved from the DOM.

- Create a root for the application: `ReactDOM.createRoot()` method is applied to that root element (the result is usually saved in 'root' constant: `const root = ReactDOM.createRoot(rootContainer)`).

- To render each component, we need to call the `render()` method on the 'root' and pass the component to it: `root.render(React.createElement(App))`.

```js
const container = document.getElementById("root");
const root = ReactDOM.createRoot(container);

root.render(React.createElement(App));
```

### One Way Data Flow

- React has a concept of One Way Data Flow.
- In React, data flows in a single direction from parent to child Components.
- Parent Component passes data to its children via props.
- The child components cannot directly modify the props they recieve. Instead, they can only use the data provided to them.

<b>State Management:</b>

- The state of a component is managed within that component and can be passed down to child components as props.
- When the state of the parent component changes, it triggers a re-render of the parent and all its children.
- When state changes, a new state object is created rather than modifying the existing state.
- To enable child Components to communicate back to their parent components, callback functions are passed as props.
- When an event occurs in the child component, it can call the callback function, passing any necessary data back up the parent component. Parent can then handle this data and update its state.

### Setup

- `npm init` - initialize new project. Initializes `package.json`.

#### Prettier Setup

- ` npm install -D prettier` - Add 'Prettier' - formats code in a nice way. Recommended to add.
  - `.prettierrc` create 'Prettier' configuration file.
  - 'Ctrl+,`.
  - 'Format On Save' in VS Code settins look up and check it.
  - Go back to settings and turn on: 'Prettier: Require Config`
  - 'Ctrl+Shift+P'.
  - Type Preferences: 'Open Settings (JSON)' and select 'Preferences: Open Settings (JSON)' (this will open the User settings).
  - In the opened settings.json file, add the following configuration:

```js
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "prettier.requireConfig": true
}

```

- Open the Command Palette by pressing Ctrl+Shift+P.
- Type Preferences: Open Settings (JSON) and select Preferences: Open Workspace Settings (JSON).
- Configure Prettier in Workspace Settings

```js
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "prettier.requireConfig": true
}
```

- In 'package.json' "scripts":`"format": "prettier --write \"src/**/*.{js,jsx}\""` - format files with this extension.
- `npm run format`
- 'Ctrl+.'
- `Preferences: Open Settings (UI)`
- `default formatter`
- `esbenp.prettier-vscode`
- Check `Format On Save`

We can check out Prettier formatting history from terminal:

- Go to the 'OUTPUT' section.
- Choose Prettier from the drop down menu.

#### ESLint Setup

To install ESLint and configure it to work with Prettier.

- `npm i -D eslint eslint-config-prettier`
- `.eslintrc.json` create file.

```js
{
  "extends": ["eslint:recommend", "prettier"],
  "plugins": [],
  "parserOptions": {
    "ecmaVersion": 2024,
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "env": {
    "es6": true,
    "browser": true,
    "node": true
  }
}
```

- 'package.json' "scripts" ` "lint": "eslint \"src/**/*.{js,jsx}\" --quiet",`

- To debug run: `npm run -- --debug` (first `--` means don't pass to npm, pass to ESLint).

#### Vite

- Install Vite `npm i -D vite @vitejs/plugin-react`
  Note: when working with Vite, if files contain JSX, the file extension must be .jsx.
- 'vite.config.js' create file.

```js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  pluging: [react()],
  root: "src",
});
```

- `root: "src",` is necessary if index.html is not in the root directory.
- 'package.json' "scripts" :

```js
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
```

` "preview": "vite preview",` is a production preview.

#### React

Install React

`npm i react react-dom`

### ESLint plugins

ESLint plugins to enhance our linting setup for a React project.

`npm install -D eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react`
