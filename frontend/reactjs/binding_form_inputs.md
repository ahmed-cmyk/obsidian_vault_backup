# Binding Form Inputs

When you bind a form to a piece of react state you should ensure that it isn’t `undefined`. Changing an undefined value will print the following error:

> **Warning**: A component is changing an uncontrolled input to be controlled.

It is better to turn it into a controlled value by assigning it a default value, like so:

```js
const [email, setEmail] = React.useState('');
```

Source: [Binding form inputs | Josh Comeau](https://www.joshwcomeau.com/react/common-beginner-mistakes/#flipping-from-uncontrolled-to-controlled-7)

---

#reactjs