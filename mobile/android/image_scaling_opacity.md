
## Image Scaling & Opacity in Jetpack Compose
### Scaling an Image

- Use the `contentScale` parameter of the Image composable to control how the image fits inside its container.
- Common option:

```kotlin
Image(
    painter = image,
    contentDescription = null,
    contentScale = ContentScale.Crop
)
```

- `ContentScale.Crop` → scales the image **uniformly** while maintaining aspect ratio so that both width & height are equal to or larger than the container.
- Don’t forget to import:

```kotlin
import androidx.compose.ui.layout.ContentScale
```

### Changing Image Opacity

- Use the alpha parameter to adjust transparency.
- Example (50% opacity):

```kotlin
Image(
    painter = image,
    contentDescription = null,
    contentScale = ContentScale.Crop,
    alpha = 0.5F
)
```

---
## Layout Modifiers
### What are Modifiers?

- Used to **decorate** or **add behavior** to composables.
- Examples: backgrounds, padding, alignment, sizing.
- A composable must accept a modifier parameter for you to apply one.

### Examples

```kotlin
Text(
    text = "Hello, World!",
    modifier = Modifier.background(color = Color.Green)
)
```

---
### Arrangement & Alignment

- **Arrangement** → how children are spaced inside Row/Column when container is larger than children.
    - spaceBetween, spaceAround, spaceEvenly, Top, Center, Bottom, etc.
- **Alignment** → aligns children to start, center, or end within the layout.

Example with Column:

```kotlin
Column(
    verticalArrangement = Arrangement.SpaceEvenly,
    horizontalAlignment = Alignment.CenterHorizontally
) {
    Text("Item 1")
    Text("Item 2")
    Text("Item 3")
}
```

---
