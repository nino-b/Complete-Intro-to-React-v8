# Vitest

1. Install 'testing-library': `npm i -D vitest @testing-library/react`

2. Install 'happy-dom' (to test everything in Node, not in the browser): `npm install -D happy-dom`

3. Configure 'package.json' - `"scripts"`: `"test": "vitest`

4. We can run tests by: `npm run test` / `npm run t` / `npm t`

5. Configure 'vite.config.js':

```js
test: {
  environment: "happy-dom";
}
```

- Note: Try to test functionality, not implementation.
- Try to test from user's perspective (what does the user expect when she clicks on the button).
- If there is a bad test, we should delete it or fix it.

#### Test files

- Test folder name must be: `__tests__`
- Test files must have either of the extensions: `.test.js`, `.test.jsx`, `.spec.js`, `.spec.jsx`.

### Testing Process

1. Set up testing environment.
   `import { expect, test } from "vitest"`
   `import { render } from "@testing-library/react"`

   `expect` - A function for making assertions in out rests.
   `test` - A function used to define a test case.

2. Set up a test.
   `test("Description of what should the result be")`

3. Cleanup - `unmount()`

- React Testing Library has built-in automatic cleanup. But if we want to explicitly clear the environment, we can call `unmount()` at the end of the test.
- `unmount()` - To keep the test environment isolated and don't interfere with other tests, we need to clean up after each test.

```js
import { expect, test } from "vitest";
import { render } from "@testing-library/react";
import Pet from "../Pet";

test("Displays a default thumbnail", async () => {
  const pet = render(<Pet />);

  const petThumbnail = await pet.findByTestId("thumbnail");
  expect(petThumbnail.src).toContain("none.jpg");

  pet.unmount();
});
```

### Cases

#### StaticRouter

If we are testing `<Link></Link>` that is not wrapped in a `<Router></Router>` in that specific Component:

- We need to wrap the Component in the test environment in `<StaticRouter></StaticRouter>`
- Suitable for server-side render testing.
- Simulates the routing context in an environment where there is no browser.

- Note: when querying an element, it is recommended to temporarily add selectors to DOM elements and use those.
- This helps out to avoid depending on the DOM structure, that might change over time.
- Also we can add same selectors to the different parts and dynamically test them.

```js
// Component to be tested
const Pet = ({ images, id }) => {
  let hero = "http://pets-images.dev-apis.com/pets/none.jpg";
  if (images && images.length) {
    hero = images[0];
  }

  return (
    <Link to={`/details/${id}`} className="pet">
      <img data-testid="thumbnail" src={hero} alt="Pet" />
    </Link>
  );
};
```

```js
// Test Case

import { expect, test } from "vitest";
import { render } from "@testing-library/react";
import { StaticRouter } from "react-router-dom/server";
import Pet from "../Pet";

test("displays a default thumbnail", async () => {
  /**
   * `<StaticRouter>` - Wraps the component in a Routing Context to simulate router behavior.
   * Used for the testing cases of routing-related features (like '<Link>') that are not themselves wrapped in a router.
   *
   * `render()` - Renders the component
   */
  render(
    <StaticRouter>
      <Pet id="123" />
    </StaticRouter>,
  );

  /**
   * Finds an element with a specific 'data-testId' attribute.
   *
   * 'data-' attribute is Temporarily added in the component.
   * This makes testing not to depend on actual DOM attributes, that might be changed over time.
   *
   *
   * 'screen' - An object that contains query methods for finding elements in the rendered output.
   *
   * 'findByTestId' - Method that finds an element by 'data-testid' attribute. It returns a promise that resolves when an element is found.
   */
  const petThumbnail = await screen.findByTestId("thumbnail");
  /**
   * 'expect' - Creates an assertion.
   *
   * 'petThumbnail.src' - Accesses 'src' attribute of the 'petThumbnail' element. Returns a string.
   *
   * 'toContain' - Checks if the value passed to it is  withing the string it is called on.
   *
   * '"none.jpg"' - Expected substring that should be present in 'src' attribute..
   */
  expect(petThumbnail.src).toContain("none.jpg");
});

test("displays a non-default thumbnail", async () => {
  render(
    <StaticRouter>
      <Pet id="123" images={("1.jpg", "2.jpg", "3.jpg")} />
    </StaticRouter>,
  );

  const petThumbnail = await screen.findByTestId("thumbnail");
  expect(petThumbnail.src).toContain("1.jpg");
});
```
