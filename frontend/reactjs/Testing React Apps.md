# Testing React Apps

## Why Test React Apps?

Testing React applications helps ensure that your components and business logic work as expected, are maintainable, and prevent regressions when making changes.

React apps are commonly tested at three levels:

* **Unit tests:** testing individual components or functions.
* **Integration tests:** testing multiple components working together.
* **End-to-end (E2E) tests:** testing the app as a user would interact with it in a browser.

---
## Key Tools & Libraries

### Jest

* Test runner and assertion library.
* Used for unit and integration tests.
* Supports mocking, code coverage, and snapshots.

### React Testing Library (RTL)

* A lightweight library built on top of DOM testing utilities.
* Encourages testing React components the way users interact with them (not implementation details).
* Works well with Jest.

### Cypress / Playwright

* Tools for E2E testing.
* Simulate real browsers and user interactions.
* Best for testing full workflows from a user’s perspective.

---
## Example: Unit Test with Jest + RTL

```jsx
// Counter.js
export default function Counter() {
  const [count, setCount] = React.useState(0);
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

```jsx
// Counter.test.js
import { render, screen, fireEvent } from "@testing-library/react";
import Counter from "./Counter";

test("increments count when button is clicked", () => {
  render(<Counter />);
  fireEvent.click(screen.getByText("Increment"));
  expect(screen.getByText("Count: 1")).toBeInTheDocument();
});
```

---
## Example: E2E Test with Cypress

```jsx
// counter.cy.js
describe("Counter", () => {
  it("increments the count", () => {
    cy.visit("/");
    cy.contains("Increment").click();
    cy.contains("Count: 1");
  });
});
```

---
## Best Practices

* Write tests that mimic how users interact with your app.
* Avoid testing implementation details.
* Keep tests small, fast, and reliable.
* Use descriptive test names that explain what behavior is being tested.
* Run tests automatically as part of CI/CD pipelines.

---
## Common Mistakes to Avoid

* Writing tests that break when refactoring without changing behavior.
* Over-mocking everything — prefer realistic scenarios.
* Not cleaning up after tests, which can cause flaky tests.
* Ignoring performance and letting tests become too slow.

---

#reactjs