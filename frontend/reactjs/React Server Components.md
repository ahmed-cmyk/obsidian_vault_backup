# React Server Components

## Primer on SSR and CSR

In client-side rendering (CSR), an empty HTML file is used, the JS is downloaded and parsed, then react creates a bunch of DOM nodes and starts populating the UI. Until this process finishes, the user is left staring at a blank screen.

In server-side rendering (SSR), the server renders our application to generate the actual HTML. The user receives a fully formed HTML document. After the JS has been downloaded, client-side react updates the DOM, and adds interactivity (event handlers, effects, etc.) to the app.

### SSR

![](React%20Server%20Components/image.png)

Each of these flags represents a commonly-used web performance metric. Here's the breakdown:

1. **First Paint** — The user is no longer staring at a blank white screen. The general layout has been rendered, but the content is still missing. This is sometimes called FCP (First Contentful Paint).
2. **Page Interactive** — React has been downloaded, and our application has been rendered/hydrated. Interactive elements are now fully responsive. This is sometimes called TTI (Time To Interactive).
3. **Content Paint** — The page now includes the stuff the user cares about. We've pulled the data from the database and rendered it in the UI. This is sometimes called LCP (Largest Contentful Paint).

---

## React Server Components

React server components are special components that run exclusively on the server-side. A key thing regarding server components is that they never re-render. Once, a server component has been rendered, it is send to the client and locked in place.

> **Note**: This means that react server components are immutable and henceforth, they are incompatible with many react features such as states as those require a certain degree of mutability.

React server components cannot use:

- `states`, because they can change.
- `effects`, because they run only after being rendered on the client, and server components never make it to the client.

**Example of a React Server Component:**

```jsx
import db from 'imaginary-db';

async function Homepage() {
  const link = db.connect('localhost', 'root', 'passw0rd');
  const data = await db.query(link, 'SELECT * FROM products');

  return (
    <>
      <h1>Trending Products</h1>
      {data.map((item) => (
        <article key={item.id}>
          <h2>{item.title}</h2>
          <p>{item.description}</p>
        </article>
      ))}
    </>
  );
}

export default Homepage;
```

> **Note**: React server components need to be tightly integrated with a bunch of stuff outside of React, things like the bundler, the server, and the router.

> **Add. Note**: Currently there's only one way to start using React Server Components, and that's with Next.js 13.4+, using their brand-new re-architected “App Router”.

---

## Changes to the development scene

Following the advent of the new react server components, all components are considered as server components unless we “opt in” for client components.

**The following snippet showcases an example of the opt in process:**

```jsx
/*
This expression signals that the component is a client component
*/
'use client'; 

import React from 'react';

function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Current value: {count}
    </button>
  );
}

export default Counter;
```

> **Note**: Client components can only import other client components.

The react team made that decision in an attempt to prevent any scenarios where one of your client components needs to re-render but it cannot because some of its children are server components.

> **Note**: You don’t need to add the `use client` directive to all components. All children of a client component are implicitly converted to client components.

**Source**: [Server Components | Josh Comeau](https://www.joshwcomeau.com/react/server-components/)

---
#reactjs