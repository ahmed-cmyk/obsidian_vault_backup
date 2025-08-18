# Suspense SSR Architecture

The primary new API is `renderToPipeableStream`.
The primary existing API is `<Suspense>`

SSR lets you generate HTML from react components on the server, and sends that HTML to your users. SSR lets your users see the page’s content before your JavaScript bundle loads and runs.

**SSR in React always happens in several steps:**

- On the server, fetch data for the entire app.
- Then, on the server, render the entire app into HTML and send it in the response.
- Then, on the client, load the JavaScript content for the entire app.
- Then, on the client, connect the JavaScript logic to the server-generated HTML for the entire app (this is "hydration”).

---

## SSR

Using SSR means that you give your users something to look at (besides a blank screen) while the JavaScript is loaded and parsed. After going through the code, interactivity is added to the application.

> **Note**: Especially for content-heavy websites, SSR is extremely useful because it lets users with worse connections start reading or looking at the content while JavaScript is loading.

When both React and your application code loads, you want to make this HTML interactive. You tell React: “Here’s the App component that generated this HTML on the server. Attach event handlers to that HTML!” React will render your component tree in memory, but instead of generating DOM nodes for it, it will attach all the logic to the existing HTML.

> **Note**: This process of rendering your components and attaching event handlers is known as “hydration”.

---

## The problem with hydration

The main problem with hydration is that it has to wait for all the data to be loaded before it starts sending any of it to the client.

After the JavaScript code loads, React starts to "hydrate" the HTML and make it interactive. React “walks” the server-generated HTML when rendering components, and attach the event handlers to that HTML.

> **Note**: For this to work, the tree produced by your components in the browser must match the tree produced by the server otherwise React can’t match them up.
> 
> A very unfortunate consequence of this is that you have to load the code for *all components* on the client before you can start hydrating any of them.

> **Add. Note**: React hydrates the tree in a single pass. This means that once it starts hydrating, React won’t stop until it’s finished doing this for the entire tree. 
> 
> As a consequence, you have to wait for all components to be hydrated before you can interact with anything.

**The entire process looks like this:**

```jsx
fetch data (server) 
	→ render to HTML (server) 
		→ load code (client) 
			→ hydrate (client)
```

> **Note**: Neither of the stages can start until the previous one ends.

---

## `<Suspense>`

React 18 lets you use `<Suspense>` to **break down your app into smaller independent units** which will go through these steps independently from each other and won’t block the rest of the app. 

As a result, your app’s users will see the content sooner and be able to start interacting with it much faster. The slowest part of your app won’t drag down the parts that are fast. 

> **Note**: These improvements are automatic, and you don’t need to write any special coordination code for them to work.

There are two major SSR features unlocked by `<Suspense>`:

- **Streaming HTML on the server**: To opt into it, you’ll need to switch from `renderToString` to the new `renderToPipeableStream` method.
- **Selective Hydration on the client**: To opt into it, you’ll need to switch to `hydrateRoot` on the client and start wrapping parts of your app with `<Suspense>`.

### HTML Streaming

> **Note**: Streaming does not occur in a top-down order. Once the data is ready, React will stream that in and the component will be made available.

The following example wraps the `<Comments />` component with `<Suspense>` component, while also telling `<Suspense>` to use the `<Spinner />` component until it’s ready:

```jsx
<Layout>
  <NavBar />
  <Sidebar />
  <RightPane>
    <Post />
    <Suspense fallback={<Spinner />}>
      <Comments />
    </Suspense>
  </RightPane>
</Layout>
```

> **Note**: By wrapping `<Comments>` into `<Suspense>`, we tell React that **it doesn’t need to wait for comments to start streaming the HTML for the rest of the page.** Instead, React will send the placeholder (a spinner) instead of the comments.

> **Add. Note**: When the data for the comments is ready on the server, React will send additional HTML into the same stream, as well as a minimal inline `<script>` tag to put that HTML in the “right place”.

### Selective Hydration

Beyond just letting you stream HTML, `<Suspense>` also allows you to perform selective hydration, wherein, you can wrap a component for which you assume React will take time to load and parse its JavaScript.

> **Note**: This means that React does not have to wait to hydrate the rest of the app. Afterwards, once the JavaScript code has been parsed, the component is also parsed.
>
> **Supp. Note**: It is important to remember that React will try to hydrate everything in one go, so selective hydration prevents users from being denied interactivity until a slow component has had its data parsed.

> **Add. Note**: React starts hydrating everything as early as possible starting with the `<Suspense>` boundary that it finds earliest in the tree, and it prioritizes the most urgent part of the screen based on the user interaction.

**Source**: [Suspense SSR Architecture | GitHub](https://github.com/reactwg/react-18/discussions/37)

---
#reactjs