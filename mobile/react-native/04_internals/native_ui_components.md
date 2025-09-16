# Native UI Components

This note explains how React Native renders UI components by leveraging native platform UI elements, and how to create custom Native UI Components.

## How React Native Renders UI

- **Shadow Tree:** React Native uses a concept called the "Shadow Tree" (or Yoga layout engine) to calculate the layout of components using Flexbox on the JavaScript side.
- **Native View Hierarchy:** The calculated layout is then used to create and update the actual native UI views (e.g., `UIView` on iOS, `android.view.View` on Android).
- **Bridge Communication:** In the old architecture, the layout instructions and view properties are sent over the JavaScript Bridge to the native side.

## Why Create Custom Native UI Components?

- **Performance:** For complex UI elements or animations that require high performance and direct access to native APIs.
- **Platform-Specific Features:** To integrate UI components that are unique to a specific platform and don't have a direct React Native equivalent.
- **Reusing Existing Native Views:** To wrap existing native UI libraries or custom views into a React Native component.

## Creating a Native UI Component (Conceptual Steps)

### iOS (Objective-C/Swift)

1.  **Create a Native View:** Develop your custom `UIView` (or subclass) in Objective-C or Swift.
2.  **Create a View Manager:** Create a class that inherits from `RCTViewManager`.
3.  **`RCT_EXPORT_MODULE()`:** Export the view manager.
4.  **`RCT_EXPORT_VIEW_PROPERTY()`:** Expose properties from your native view to JavaScript.
5.  **JavaScript Wrapper:** Create a JavaScript component that uses `requireNativeComponent` to bridge to your native view manager.

### Android (Java/Kotlin)

1.  **Create a Native View:** Develop your custom `android.view.View` (or subclass) in Java or Kotlin.
2.  **Create a View Manager:** Create a class that extends `SimpleViewManager` (or `ViewGroupManager` for composite views).
3.  **`getName()` Method:** Override `getName()` to return the component's name.
4.  **`@ReactProp` Annotation:** Expose properties from your native view to JavaScript.
5.  **JavaScript Wrapper:** Create a JavaScript component that uses `requireNativeComponent` to bridge to your native view manager.

## Example (Conceptual)

```javascript
// JavaScript side
import { requireNativeComponent } from 'react-native';

const MyNativeView = requireNativeComponent('MyNativeView');

const App = () => {
  return <MyNativeView style={{ width: 100, height: 100, backgroundColor: 'red' }} />; 
};

export default App;
```

```objective-c
// iOS Native View Manager (Objective-C)
#import <React/RCTViewManager.h>

@interface MyNativeViewManager : RCTViewManager
@end

@implementation MyNativeViewManager

RCT_EXPORT_MODULE(MyNativeView)

- (UIView *)view
{
  UIView *view = [[UIView alloc] init];
  view.backgroundColor = [UIColor blueColor];
  return view;
}

@end
```

```java
// Android Native View Manager (Java)
package com.your-app-name;

import com.facebook.react.uimanager.SimpleViewManager;
import com.facebook.react.uimanager.ThemedReactContext;
import com.facebook.react.uimanager.annotations.ReactProp;
import android.graphics.Color;
import android.view.View;

public class MyNativeViewManager extends SimpleViewManager<View> {

    public static final String REACT_CLASS = "MyNativeView";

    @Override
    public String getName() {
        return REACT_CLASS;
    }

    @Override
    public View createViewInstance(ThemedReactContext context) {
        View view = new View(context);
        view.setBackgroundColor(Color.RED);
        return view;
    }

    @ReactProp(name = "color")
    public void setColor(View view, String color) {
        view.setBackgroundColor(Color.parseColor(color));
    }
}
```