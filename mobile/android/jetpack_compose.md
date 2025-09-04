
**Jetpack Compose** is a modern toolkit for building Android UIs. Compose simplifies and accelerates UI development on Android with less code, powerful tools, and intuitive Kotlin capabilities.

> **NOTE:** With Compose, you can build your UI by defining a set of functions, called composable functions, that take in data and describe UI elements.

**There are a few differences with composable functions compared to normal functions:**

1. You add the `@Composable` annotation before the function.
2. `@Composable` function names are capitalized.
3. `@Composable` functions can't return anything.

A `Surface` is a container that represents a section of UI where you can alter the appearance, such as the color or border.

```kotlin
@Composable  
fun Greeting(name: String, modifier: Modifier = Modifier) {  
    Surface(color = Color.Blue) {  
        Text(  
            text = "Hi, my name is $name!",  
            modifier = modifier  
        )  
    }  
}
```

---
## Annotations

**Annotations** are means of attaching extra information to code. This information helps tools like the Jetpack Compose compiler, and other developers understand the app's code.

> **NOTE:** Annotations can take parameters. Parameters provide extra information to the tools processing them.

---
## Composable function names

The compose function that returns nothing and bears the `@Composable` annotation MUST be named using Pascal case. 

The Compose function:

- _MUST_ be a noun: `DoneButton()`
- _NOT_ a verb or verb phrase: `DrawTextField()`
- _NOT_ a nouned preposition: `TextFieldWithLink()`
- _NOT_ an adjective: `Bright()`
- _NOT_ an adverb: `Outside()`

---
## UI Hierarchy in Compose

UI is structured as a **tree of composables**. Higher-level containers (layouts) wrap and position child composables.

---
## Layout Composables

### Column

- Arranges children **vertically** (top → bottom).
- Think of it as a vertical stack.
- Commonly used for screens with elements stacked down.

```kotlin
Column(
    modifier = Modifier.fillMaxSize(),
    verticalArrangement = Arrangement.Center,
    horizontalAlignment = Alignment.CenterHorizontally
) {
    Text("First")
    Text("Second")
}
```

### Row

- Arranges children **horizontally** (left → right).    
- Useful for toolbars, lists of buttons, etc.

```kotlin
Row(
    modifier = Modifier.fillMaxWidth(),
    horizontalArrangement = Arrangement.SpaceEvenly,
    verticalAlignment = Alignment.CenterVertically
) {
    Text("One")
    Text("Two")
    Text("Three")
}
```

### Box

- Places children **on top of each other** (like a stack).    
- First child is at the back, later ones overlay on top.
- Useful for overlays, backgrounds, or floating buttons.

```kotlin
Box(
    modifier = Modifier.fillMaxSize(),
    contentAlignment = Alignment.BottomEnd
) {
    Image(painter = painterResource(R.drawable.bg), contentDescription = null)
    Button(onClick = { }) {
        Text("Click me")
    }
}
```

### Quick Summary

- **Column** → vertical stack.    
- **Row** → horizontal stack.
- **Box** → layered stack (overlapping).

---
