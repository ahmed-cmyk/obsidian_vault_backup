# Returning Multiple Elements

The best practice for returning multiple elements is to use `<React.Fragment>`. This helps as it  prevents us from having to use `<div>` to wrap around the returned elements, which would in turn pollute your markup.

The shorthand for `<React.Fragment>` is to use `<>`. This means that the following two examples are presented as the same:

```jsx
<React.Fragment>
	<element1 />
	<element2 />
<React.Fragment>
```

```jsx
<>
	<element1 />
	<element2 />
<>
```

Source: [Return Multiple Elements | Josh Comeau](https://www.joshwcomeau.com/react/common-beginner-mistakes/#returning-multiple-elements-6)

---

#reactjs