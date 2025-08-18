## To `stdout`

```go
package main

import (
	"context"
	"os"
)

func main() {
	component := hello("John")
	component.Render(context.Background(), os.Stdout)
}
```

> **NOTE:** Instead of passing `os.Stdout` to the component's render function, you can pass any type that implements the `io.Writer` interface. This includes files, `bytes.Buffer`, and HTTP responses.

## To HTTP

```go
package main

import (
	"fmt"
	"net/http"

	"github.com/a-h/templ"
)

func main() {
	component := hello("John")
	
	http.Handle("/", templ.Handler(component))

	fmt.Println("Listening on :3000")
	http.ListenAndServe(":3000", nil)
}
```

---

#templ #go