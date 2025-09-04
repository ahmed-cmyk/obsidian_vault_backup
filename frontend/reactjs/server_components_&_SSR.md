# Server Components & SSR in React

## What are Server Components?

Server Components are a new experimental React feature (introduced in React 18) that allows certain components to render **entirely on the server**, sending only the necessary HTML and data to the client — without shipping unnecessary JavaScript to the browser.

They let you build faster, smaller, and more secure apps by keeping logic and heavy computation on the server.

---
## Key Concepts

### Server Components vs Client Components

* **Server Components**
  * Render on the server.
  * Do not include client-side JavaScript.
  * Cannot use browser-only APIs (like `window`, `document`, or `useState`).
  * Ideal for fetching data, rendering markup, or doing secure computations.

* **Client Components**

  * Render in the browser.
  * Can use interactivity, state, and browser APIs.

You can combine both in a single app. Server Components can render Client Components inside them.

---
### Example

```jsx
// Server Component
export default function ProductList() {
  const products = fetchProductsFromDB();
  return (
    <ul>
      {products.map((product) => (
        <Product key={product.id} product={product} />
      ))}
    </ul>
  );
}

// Client Component
"use client";

function Product({ product }) {
  const [count, setCount] = useState(0);
  return (
    <li>
      {product.name} <button onClick={() => setCount(count + 1)}>Add</button>
    </li>
  );
}
```

Here, `ProductList` runs only on the server, but it renders `Product`, which is interactive and runs in the browser.

---
## Server-Side Rendering (SSR)

### What is SSR?

In SSR, React renders the entire page (or part of it) on the server into HTML, which is sent to the client. This enables faster first paint (as the browser receives ready HTML) and better SEO.

SSR has been around for much longer than Server Components and is supported by frameworks like Next.js.

---
## SSR Example with Next.js

```jsx
export async function getServerSideProps() {
  const data = await fetchData();
  return { props: { data } };
}

export default function Page({ data }) {
  return <div>{data}</div>;
}
```

---
## Benefits of Server Components & SSR

* Smaller bundle sizes (less JavaScript sent to the client).
* Faster initial render & improved Core Web Vitals.
* Better SEO and crawler-friendly.
* Logic and data stay secure on the server.

---
## Common Mistakes to Avoid

✅ Trying to use browser APIs in Server Components — they run on the server.
✅ Over-fetching data on both client and server — prefer server-fetch when possible.
✅ Treating Server Components as a replacement for SSR — they solve different problems and can work together.
✅ Forgetting to mark Client Components properly (in Next.js, you must use `"use client"`).

---
#reactjs