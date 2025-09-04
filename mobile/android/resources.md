
## What are Resources?

- **Resources** = static content used by your app (images, strings, colors, layouts, animations, etc.).    
- Kept **separate from code** → makes them easier to manage and localize.
- At runtime, Android picks the **appropriate resource** based on the device configuration (e.g., language, screen size, night mode).

---
## Organizing Resources

- Stored in the `res/` directory inside subfolders:

```
MyProject/
    src/
        MyActivity.kt
    res/
        drawable/   -> graphics (PNG, XML shapes, vectors)
        mipmap/     -> launcher icons
        values/     -> strings.xml, colors.xml, themes.xml
```

- **drawable/** → images, shapes, vector assets
- **mipmap/** → launcher icons (kept separate since Android may use different resolutions on different screens/launchers)
- **values/** → XML-based resources (strings, colors, themes, dimensions)

> ✅ Use correct folder + qualifiers (drawable-hdpi, values-night, etc.) for device-specific resources.

---
## Accessing Resources in Compose

- Android generates an **R class** that contains IDs for all resources.
    - Example: R.drawable.graphic → refers to graphic.png in `drawable/`.

### Loading Drawables in Compose

```kotlin
val image = painterResource(R.drawable.androidparty)

Image(
    painter = image,
    contentDescription = null // skips TalkBack since decorative
)
```

📌 Notes:

- Import needed:

```kotlin
import androidx.compose.foundation.Image
import androidx.compose.ui.res.painterResource
```

- `painterResource()` → loads an image resource as a Painter for Compose.

---
## Accessibility & Content Descriptions

- **Always provide contentDescription** for meaningful images.
- Use null for decorative images → so TalkBack ignores them.
- Example:

```kotlin
Image(
    painter = image,
    contentDescription = "Profile picture"
)
```

---
## Previews

Compose lets you preview your composables inside Android Studio.

```kotlin
@Preview(showBackground = true)
@Composable
fun BirthdayCardPreview() {
    MyTheme {
        GreetingImage(
            message = "Happy Birthday Sam!",
            from = "From Emma"
        )
    }
}
```

---
## Extra Info (Best Practices)

- Prefer **vector drawables** (.xml) → scale across all screen densities.
- Use **string resources (strings.xml)** instead of hardcoding → allows localization.
- Use **colors.xml** and **themes.xml** for consistent theming.
- For large image assets, consider placing in drawable-nodpi/ if you don’t want Android to auto-scale.
- Use **contentDescription** carefully → improves accessibility and usability.

---