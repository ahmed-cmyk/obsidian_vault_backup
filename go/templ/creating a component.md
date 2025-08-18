
```go
package main

templ hello(name string) {
	<div>Hello, {name}</div>
}
```

> **NOTE:** You can generate go code from the templ file using `templ generate`.

The generated file looks something like:

```go
func hello(name string) templ.Component {
  // ...
}
```

---

#templ #go 