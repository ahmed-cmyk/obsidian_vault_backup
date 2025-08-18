# Modern Mode

For a very long time JS didn’t have any compatibility issues as even whenever new code was added to the language, the old functionality didn’t change.

The downside of this approach was that any mistake or imperfect decision got stuck in the language forever.

This was the case until 2009 when ECMAScript 5 (ES5) appeared. It added new features to the language and modified some of the existing ones. To keep the old code working, most such modifications are off by default. You need to explicitly enable them with a special directive: "use strict".

## Enabling the modern way

The directive looks like a string: `"use strict"` or `'use strict'`. When it is located at the top of a script, the whole script works the “modern” way.

```js
"use strict";

// this code works the modern way
...
```

> **IMPORTANT:** Please make sure that "use strict" is at the top of your scripts, otherwise strict mode may not be enabled.

> **NOTE:** You cannot cancel `”use strict"` once it’s been activated.

### Enabling in the browser console

You can try to press `Shift+Enter` to input multiple lines, and put use strict on top, like this:

```js
'use strict'; <Shift+Enter for a newline>
//  ...your code
<Enter to run>
```

> **NOTE:** It works in most browsers, namely Firefox and Chrome.

### Do you need it?

Using modern features like “classes” and “modules’ enables `”use strict"` automatically.

---

#javascript