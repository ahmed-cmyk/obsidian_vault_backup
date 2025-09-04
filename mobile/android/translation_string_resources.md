
## Why use string resources?

- Keeps **UI text separate** from code (easier to maintain).
- Supports **localization (l10n)** → adapting app to different languages & regions.
- Supports **internationalization (i18n)** → designing app so it can be localized easily.
- Makes it easier for translators to work without touching code.

---
## Defining string resources

- Strings are stored in XML inside the **res/values/** directory.
- File: `res/values/strings.xml`

**Example:**

```xml
<resources>
    <string name="app_name">MyApp</string>
    <string name="welcome_message">Welcome, %1$s!</string>
</resources>
```

- name → unique ID (used in code).
- Value → actual text.

---

## Accessing string resources

- **In Kotlin code:**

```kotlin
val appName = getString(R.string.app_name)
```

- **In XML layouts (View-based):**

```xml
android:text="@string/welcome_message"
```

- **In Jetpack Compose:**

```kotlin
Text(text = stringResource(R.string.welcome_message))
```  

---
## String formatting

- Use placeholders for dynamic values.
- `%s` → string, `%d` → integer, `%f` → float, etc.

```xml
<string name="greeting">
Hello %1$s, you have %2$d new messages.
</string>
```

```kotlin
val greeting = getString(R.string.greeting, "Ahmed", 3)
```

---
## Supporting multiple languages

- Create **language-specific values folders**:    

```
res/
  values/strings.xml          # Default (e.g., English)
  values-es/strings.xml       # Spanish
  values-fr/strings.xml       # French
  values-ur/strings.xml       # Urdu
```

- Example `values-es/strings.xml`:

```xml
<resources>
    <string name="app_name">MiAplicación</string>
    <string name="welcome_message">¡Bienvenido, %1$s!</string>
</resources>
```

- Android automatically picks the correct version based on the device **locale**.    

---
## Best practices

- Always keep **default strings.xml** in English (or your base language).
- Avoid hardcoding strings in layouts or code.
- Use **string arrays** when you have a list of localized items.
- Test different locales using:
    - **Android Studio → Tools → Localization Inspector**
    - Or change device language in settings.
- If a translation is missing, Android falls back to the default `values/strings.xml`.

---
## Extras

- **Plural support (quantities):**

```xml
<plurals name="numberOfSongsAvailable">
    <item quantity="one">%d song found.</item>
    <item quantity="other">%d songs found.</item>
</plurals>
```

```kotlin
val songs = resources.getQuantityString(R.plurals.numberOfSongsAvailable, 3, 3)
```

- **Translatable attribute:**

```xml
<string name="app_id" translatable="false">com.example.app</string>
```

- → prevents non-UI strings (like IDs) from being included in translation.

---