
templ files start with a package name, followed by any required imports, just like Go.

```go
package main

import "fmt"
import "time"
```

---
## Components

Components are markup and code that is compiled into functions that return a `templ.Component` interface by running the `templ generate` command.

Components can contain templ elements that render HTML, text, expressions that output text or include other templates, and branching statements such as `if` and `switch`, and `for` loops.

```go
package main

templ headerTemplate(name string) {
  <header data-testid="headerTemplate">
    <h1>{ name }</h1>
  </header>
}
```

### Tags must be closed

Unlike HTML, templ requires that all HTML elements are closed with either a closing tag (`</a>`), or by using a self-closing element (`<hr/>`).

> **NOTE:** templ is aware of which HTML elements are "void", and will not include the closing `/` in the output HTML.

---
## Elements

templ elements are used to render HTML within templ components.

```go
// button.templ
package main

templ button(text string) {
	<button class="button">{ text }</button>
}
```

```go
// main.go
package main

import (
	"context"
	"os"
)

func main() {
	button("Click me").Render(context.Background(), os.Stdout)
}
```

```html
<!-- output -->
<button class="button">
 Click me
</button>
```

## Attributes

### Constant Attributes

templ elements can have HTML attributes that use the double quote character `"`.

```go
templ component() {
  <p data-testid="paragraph">Text</p>
}
```

```html
<!-- output -->
<p data-testid="paragraph">Text</p>
```

### String expression attributes

Element attributes can be set to Go strings.

```go
templ component(testID string) {
  <p data-testid={ testID }>Text</p>
}

templ page() {
  @component("testid-123")
}
```

Rendering the `page` component results in:

```html
<!-- output -->
<p data-testid="testid-123">Text</p>
```

It's also possible to use function calls in string attribute expressions.

Here's a function that returns a string based on a boolean input.

```go
func testID(isTrue bool) string {
    if isTrue {
        return "testid-123"
    }
    return "testid-456"
}
```

```go
templ component() {
  <p data-testid={ testID(true) }>Text</p>
}
```

The result:

```html
<!-- output -->
<p data-testid="testid-123">Text</p>
```

Functions in string attribute expressions can also return errors.

```go
func testID(isTrue bool) (string, error) {
    if isTrue {
        return "testid-123", nil
    }
    return "", fmt.Errorf("isTrue is false")
}
```

If the function returns an error, the `Render` method will return the error along with its location.

### Boolean attributes

Boolean attributes where the presence of an attribute name without a value means `true`, and the attribute name not being present means `false` are supported.

```go
templ component() {
  <hr noshade/>
}
```

```html
<!-- output -->
<hr noshade>
```

To set boolean attributes using variables or template parameters, a question mark after the attribute name is used to denote that the attribute is boolean.

```go
templ component() {
  <hr noshade?={ false } />
}
```

```html
<!-- output -->
<hr>
```

### Conditional attributes

Use an `if` statement within a templ element to optionally add attributes to elements.

```go
templ component() {
  <hr style="padding: 10px"
    if true {
      class="itIsTrue"
    }
  />
}
```

```html
<!-- output -->
<hr style="padding: 10px" class="itIsTrue" />
```

### Attribute key expressions

Use a string expression to dynamically set the key of an attribute.

```go
templ paragraph(testID string) {
  <p { "data-" + testID }="paragraph">Text</p>
}

templ component() {
  @paragraph("testid")
}
```

```html
<!-- output -->
<p data-testid="paragraph">Text</p>
```

### Spread attributes

Use the `{ attrMap... }` syntax in the open tag of an element to append a dynamic map of attributes to the element's attributes.

It's possible to spread any variable of type `templ.Attributes`. `templ.Attributes` is a `map[string]any` type definition.

- If the value is a `string`, the attribute is added with the string value, e.g. `<div name="value">`.
- If the value is a `bool`, the attribute is added as a boolean attribute if the value is true, e.g. `<div name>`.
- If the value is a `templ.KeyValue[string, bool]`, the attribute is added if the boolean is true, e.g. `<div name="value">`.
- If the value is a `templ.KeyValue[bool, bool]`, the attribute is added if both boolean values are true, as `<div name>`.

```go
templ component(shouldBeUsed bool, attrs templ.Attributes) {
  <p { attrs... }>Text</p>
  <hr
    if shouldBeUsed {
      { attrs... }
    }
  />
}

templ usage() {
  @component(false, templ.Attributes{"data-testid": "paragraph"}) 
}
```

```html
<!-- output -->
<p data-testid="paragraph">Text</p>
<hr>
```

### URL attributes

Attributes that expect a URL, such as `<a href={ url }>`, `<form action={ url }>`, or `<img src={ url }>`, have special behavior if you use a dynamic value.

```go
templ component(p Person) {
  <a href={ p.URL }>{ strings.ToUpper(p.Name) }</a>
}
```

When you pass a `string` to these attributes, templ will automatically sanitize the input URL, ensuring that the protocol is safe (e.g., `http`, `https`, or `mailto`) and does not contain potentially harmful protocols like `javascript:`.

>**NOTE:** To bypass URL sanitization, you can use `templ.SafeURL(myURL)` to mark that your string is safe to use. This may introduce security vulnerabilities to your program.

If you use a constant value, e.g. `<a href="javascript:alert('hello')">`, templ will not modify it, and it will be rendered as is.

### JS attributes

`onClick` and other `on*` handlers have special behaviour, they expect a reference to a `script` template.

> **NOTE:** This ensures that any client-side JavaScript that is required for a component to function is only emitted once, that script name collisions are not possible, and that script input parameters are properly sanitized.

```go
script withParameters(a string, b string, c int) {
	console.log(a, b, c);
}

script withoutParameters() {
	alert("hello");
}

templ Button(text string) {
	<button onClick={ withParameters("test", text, 123) } onMouseover={ withoutParameters() } type="button">{ text }</button>
}
```

```html
<!-- output -->
<script>
 function __templ_withParameters_1056(a, b, c){console.log(a, b, c);}function __templ_withoutParameters_6bbf(){alert("hello");}
</script>
<button onclick="__templ_withParameters_1056("test","Say hello",123)" onmouseover="__templ_withoutParameters_6bbf()" type="button">
 Say hello
</button>
```

### JSON attributes

To set an attribute's value to a JSON string (e.g. for HTMX's [hx-vals](https://htmx.org/attributes/hx-vals) or Alpine's [x-data](https://alpinejs.dev/directives/data)), serialize the value to a string using a function.

```go
func countriesJSON() string {
	countries := []string{"Czech Republic", "Slovakia", "United Kingdom", "Germany", "Austria", "Slovenia"}
	bytes, _ := json.Marshal(countries)
	return string(bytes)
}
```

```go
templ SearchBox() {
	<search-webcomponent suggestions={ countriesJSON() } />
}
```

### Attributes and elements can contain expressions

templ elements can contain placeholder expressions for attributes and content.

```go
// button.templ
package main

templ button(name string, content string) {
	<button value={ name }>{ content }</button>
}
```

Rendering the component to stdout, we can see the results.

```go
// main.go
func main() {
	component := button("John", "Say Hello")
	component.Render(context.Background(), os.Stdout)
}
```

```html
<!-- output -->
<button value="John">Say Hello</button>
```

---
## Statements

### Templ: Control Statements (`if`, `for`, `switch`)

Templ does **not** require a special prefix for `if`, `for`, or `switch` since Go control statements are more common than text starting with these words.  

However, if a text run begins with `if`, `for`, or `switch` **without an opening `{`**, Templ will throw an error to avoid printing raw source code.

---
### Examples of Errors

**Missing `{`:**
```go
// broken-if.templ
package main

templ showIfTrue(b bool) {
	if b 
	  <p>Hello</p>
	}
}
````

**Text starting with if:**

```go
// paragraph.templ
package main

templ text(b bool) {
	<p>if a tree fell in the woods</p>
}
```

> This rule applies to if, for, and switch.

---
## **How to Fix**

1. Wrap in a Go string expression.    
2. Capitalize the keyword (If, For, Switch).

```go
// display.templ
package main

templ display(price float64, count int) {
	<p>Switch to Linux</p>
	<p>{ `switch to Linux` }</p>
	<p>{ "for a day" }</p>
	<p>{ fmt.Sprintf("%f", price) }{ "for" }{ fmt.Sprintf("%d", count) }</p>
	<p>{ fmt.Sprintf("%f for %d", price, count) }</p>
}
```

---
## Raw Go

### Variable declarations

Scoped variables can be created using this syntax, to reduce the need for multiple function calls.

```go
// component.templ
package main

templ nameList(items []Item) {
    {{ first := items[0] }}
    <p>
        { first.Name }
    </p>
}
```

```html
<!-- output -->
<p>A</p>
```

---
## Template composition

Templates can be composed using the import expression.

```go
templ showAll() {
	@left()
	@middle()
	@right()
}

templ left() {
	<div>Left</div>
}

templ middle() {
	<div>Middle</div>
}

templ right() {
	<div>Right</div>
}
```

```html
<!-- output -->
<div>
 Left
</div>
<div>
 Middle
</div>
<div>
 Right
</div>
```

### Children

Children can be passed to a component for it to wrap.

```go
templ showAll() {
	@wrapChildren() {
		<div>Inserted from the top</div>
	}
}

templ wrapChildren() {
	<div id="wrapper">
		{ children... }
	</div>
}
```

```html
<div id="wrapper">
 <div>
  Inserted from the top
 </div>
</div>
```

#### Using children in code components

Children are passed to a component using the Go context. To pass children to a component using Go code, use the `templ.WithChildren` function.

```go
package main

import (
  "context"
  "os"

  "github.com/a-h/templ"
)

templ wrapChildren() {
	<div id="wrapper">
		{ children... }
	</div>
}

func main() {
  contents := templ.ComponentFunc(func(ctx context.Context, w io.Writer) error {
    _, err := io.WriteString(w, "<div>Inserted from Go code</div>")
    return err
  })
  ctx := templ.WithChildren(context.Background(), contents)
  wrapChildren().Render(ctx, os.Stdout)
}
```

```html
<!-- output -->
<div id="wrapper">
 <div>
  Inserted from Go code
 </div>
</div>
```

To get children from the context, use the `templ.GetChildren` function.

```go
package main

import (
  "context"
  "os"

  "github.com/a-h/templ"
)

func main() {
  contents := templ.ComponentFunc(func(ctx context.Context, w io.Writer) error {
    _, err := io.WriteString(w, "<div>Inserted from Go code</div>")
    return err
  })
  wrapChildren := templ.ComponentFunc(func(ctx context.Context, w io.Writer) error {
    children := templ.GetChildren(ctx)
    ctx = templ.ClearChildren(ctx)
    _, err := io.WriteString(w, "<div id=\"wrapper\">")
    if err != nil {
      return err
    }
    err = children.Render(ctx, w)
    if err != nil {
      return err
    }
    _, err = io.WriteString(w, "</div>")
    return err
  })
```

### Components as parameters

Components can also be passed as parameters and rendered using the `@component` expression.

```go
package main

templ heading() {
    <h1>Heading</h1>
}

templ layout(contents templ.Component) {
	<div id="heading">
		@heading()
	</div>
	<div id="contents">
		@contents
	</div>
}

templ paragraph(contents string) {
	<p>{ contents }</p>
}
```

```go
// main.go
package main

import (
	"context"
	"os"
)

func main() {
	c := paragraph("Dynamic contents")
	layout(c).Render(context.Background(), os.Stdout)
}
```

```html
<!-- output -->
<div id="heading">
	<h1>Heading</h1>
</div>
<div id="contents">
	<p>Dynamic contents</p>
</div>
```

### Joining Components

Components can be aggregated into a single Component using `templ.Join`.

```go
package main

templ hello() {
	<span>hello</span>
}

templ world() {
	<span>world</span>
}

templ helloWorld() {
	@templ.Join(hello(), world())
}
```

```go
// main.go
package main

import (
	"context"
	"os"
)

func main() {
	helloWorld().Render(context.Background(), os.Stdout)
}
```

```html
<!-- output -->
<span>hello</span><span>world</span>
```

### Sharing and re-using components

Since templ components are compiled into Go functions by the `go generate` command, templ components follow the rules of Go, and are shared in exactly the same way as Go code.

templ files in the same directory can access each other's components. Components in different directories can be accessed by importing the package that contains the component, so long as the component is exported by capitalizing its name.

#### Exporting components

To make a templ component available to other packages, export it by capitalizing its name.

```go
package components

templ Hello() {
	<div>Hello</div>
}
```

#### Importing components

To use a component in another package, import the package and use the component as you would any other Go function or type.

```go
package main

import "github.com/a-h/templ/examples/counter/components"

templ Home() {
	@components.Hello()
}
```

---
## Forms and Validation

To pass data from the client to the server without using JavaScript, you can use HTML forms to POST data.

templ can be used to create forms that submit data to the server. Depending on the design of your app, you can collect data from the form using JavaScript and submit it to an API from the frontend, or use a HTTP form submission to send the data to the server.

### Hypermedia approach

templ isn't a framework, you're free to choose how you want to build your applications, but a common approach is to create a handler for each route, and then use templates to render the form and display validation errors.

In Go, the `net/http` package in the standard library provides a way to handle form submissions, and Gorilla `schema` can decode form data into Go structs. See [https://github.com/gorilla/schema](https://github.com/gorilla/schema)

#### Create a View Model

This view model should contain any data that is used by the form, including field values and any other state.

```go
type Model struct {
  Initial          bool
  SubmitButtonText string

  Name  string
  Email string
  Error string
}
```

The model can also include methods for validation, which will be used to check the data before saving it to the database.

#### Create a form template

The form should contain input fields for each piece of data in the model.

In the example code, the `name` and `email` input fields are populated with the values from the model.

Later, we will use the Gorilla `schema` package to populate Go struct fields automatically from the form data when the form is submitted.

If a field value is invalid, the `has-error` class is added to the form group using the `templ.KV` function.

To protect your forms from cross-site request forgery (CSRF) attacks, use the [`gorilla/csrf`](https://github.com/gorilla/csrf) middleware to generate and validate CSRF tokens.

```go
csrfKey := mustGenerateCSRFKey()
csrfMiddleware := csrf.Protect(csrfKey, csrf.TrustedOrigins([]string{"localhost:8080"}), csrf.FieldName("_csrf"))
```

In your form templates, include a hidden CSRF token field using a shared component:

```html
<input type="hidden" name="_csrf" value={ ctx.Value("gorilla.csrf.Token").(string) }/>
```

This ensures all POST requests include a valid CSRF token.

```go
templ View(m Model) {
  <h1>Add Contact</h1>
  <ul>
    <li><a href="/contacts" hx-boost="true">Back to Contacts</a></li>
  </ul>
  <form id="form" method="post" hx-boost="true">
    @csrf.CSRF()
    <div id="name-group" class={ "form-group", templ.KV("has-error", m.NameHasError()) }>
      <label for="name">Name</label>
      <input type="text" id="name" name="name" class="form-control" placeholder="Name" value={ m.Name }/>
    </div>
    <div id="email-group" class={ "form-group", templ.KV("has-error", m.EmailHasError()) }>
      <label for="email">Email</label>
      <input type="email" id="email" name="email" class="form-control" placeholder="Email" value={ m.Email }/>
    </div>
    <div id="validation">
      if m.Error != "" {
        <p class="error">{ m.Error }</p>
      }
      if msgs := m.Validate(); len(msgs) > 0 {
        @ValidationMessages(msgs)
      }
    </div>
    <a href="/contacts" class="btn btn-secondary">Cancel</a>
    <input type="submit" value="Save"/>
  </form>
}
```

#### Display the form

The next step is to display the form to the user.

On `GET` requests, the form is displayed with an empty model for adding a new contact, or with an existing contact's data for editing.

```go
func (h *Handler) Get(w http.ResponseWriter, r *http.Request) {
  model := NewModel()
  // If it's an edit request, populate the model with existing data.
  if id := r.PathValue("id"); id != "" {
    contact, ok, err := h.DB.Get(r.Context(), id)
    if err != nil {
      h.Log.Error("Failed to get contact", slog.String("id", id), slog.Any("error", err))
      http.Error(w, err.Error(), http.StatusInternalServerError)
      return
    }
    if !ok {
      http.Redirect(w, r, "/contacts/edit", http.StatusSeeOther)
      return
    }
    model = ModelFromContact(contact)
  }
  h.DisplayForm(w, r, model)
}
```

#### Handle form submission

When the form is submitted, the `POST` request is handled by parsing the form data and decoding it into the model using the Gorilla `schema` package.

If validation fails, the form is redisplayed with error messages.

```go
func (h *Handler) Post(w http.ResponseWriter, r *http.Request) {
  // Parse the form.
  err := r.ParseForm()
  if err != nil {
    http.Error(w, err.Error(), http.StatusBadRequest)
    return
  }

  var model Model

  // Decode the form.
  dec := schema.NewDecoder()
  dec.IgnoreUnknownKeys(true)
  err = dec.Decode(&model, r.PostForm)
  if err != nil {
    h.Log.Warn("Failed to decode form", slog.Any("error", err))
    http.Error(w, err.Error(), http.StatusBadRequest)
    return
  }

  // Validate the input.
  if len(model.Validate()) > 0 {
    h.DisplayForm(w, r, model)
    return
  }

  // Save the contact.
  id := r.PathValue("id")
  if id == "" {
    id = ksuid.New().String()
  }
  contact := db.NewContact(id, model.Name, model.Email)
  if err = h.DB.Save(r.Context(), contact); err != nil {
    h.Log.Error("Failed to save contact", slog.String("id", id), slog.Any("error", err))
    model.Error = "Failed to save the contact. Please try again."
    h.DisplayForm(w, r, model)
    return
  }

  // Redirect back to the contact list.
  http.Redirect(w, r, "/contacts", http.StatusSeeOther)
}
```



---


#go #templ 