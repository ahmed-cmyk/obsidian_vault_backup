
## Definition

- In Material Design, a **scaffold** is the fundamental **layout structure** for building consistent UIs.
- It organizes and holds together UI components like app bars, floating action buttons (FAB), and content.
- Provides a **standardized platform** so apps have a coherent look and feel.

---
## Key Properties of Scaffold

- `topBar` → App bar across the **top** of the screen (e.g., TopAppBar).
- `bottomBar` → App bar across the **bottom** of the screen (e.g., BottomAppBar).
- `floatingActionButton` → Floating button in the **bottom-right corner**, for key actions.
- `content` → Main screen content.
    - Receives PaddingValues from Scaffold → should be applied to root content to avoid overlapping bars.

---
## Usage Notes

- Provides **Material Design compliance** by default.
- Great for layouts with:
    - App bar(s)
    - FAB
    - Snackbars
    - Drawer layouts
- Helps manage **system insets and padding** automatically.

---
## Example

```kotlin
@Composable
fun ScaffoldExample() {
    var presses by remember { mutableIntStateOf(0) }

    Scaffold(
        topBar = {
            TopAppBar(
                colors = topAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer,
                    titleContentColor = MaterialTheme.colorScheme.primary,
                ),
                title = { Text("Top app bar") }
            )
        },
        bottomBar = {
            BottomAppBar(
                containerColor = MaterialTheme.colorScheme.primaryContainer,
                contentColor = MaterialTheme.colorScheme.primary,
            ) {
                Text(
                    modifier = Modifier.fillMaxWidth(),
                    textAlign = TextAlign.Center,
                    text = "Bottom app bar",
                )
            }
        },
        floatingActionButton = {
            FloatingActionButton(onClick = { presses++ }) {
                Icon(Icons.Default.Add, contentDescription = "Add")
            }
        }
    ) { innerPadding ->
        Column(
            modifier = Modifier.padding(innerPadding),
            verticalArrangement = Arrangement.spacedBy(16.dp),
        ) {
            Text(
                modifier = Modifier.padding(8.dp),
                text =
                    """
                    This is an example of a scaffold. It uses Scaffold's parameters to create:
                    - Top app bar
                    - Bottom app bar
                    - Floating action button
                    
                    You have pressed the FAB $presses times.
                    """.trimIndent(),
            )
        }
    }
}
```

---
## Summary

- **Scaffold = foundation for Material layouts.**
- Helps combine app bars, FAB, snackbars, drawers, and main content.
- Ensures **consistent padding** and layout behavior across devices.

---