# Native Modules

This note explains what Native Modules are in React Native, how they allow JavaScript to interact with native platform capabilities, and how to create them for both iOS and Android.

## What are Native Modules?

- **Purpose:** Native Modules are platform-specific code (Objective-C/Swift for iOS, Java/Kotlin for Android) that can be invoked from JavaScript in your React Native application.
- **Use Cases:** Accessing device features not available in React Native directly (e.g., Bluetooth, advanced camera features, custom hardware), integrating existing native libraries, or optimizing performance-critical operations.

## How they Work (Old Architecture)

- **JavaScript Bridge:** Native Modules communicate with JavaScript via the JavaScript Bridge.
- **Exporting Modules:** Native code exposes methods and constants to JavaScript.
- **Callbacks and Promises:** Asynchronous operations often use callbacks or Promises to return results to JavaScript.

## Creating a Native Module (Conceptual Steps)

### iOS (Objective-C/Swift)

1.  **Create a Class:** Create a new Objective-C or Swift class that inherits from `RCTEventEmitter` (for events) or `NSObject`.
2.  **`RCT_EXPORT_MODULE()`:** Use this macro to export the module to JavaScript.
3.  **`RCT_EXPORT_METHOD()`:** Use this macro to export methods that can be called from JavaScript.
4.  **`RCT_EXPORT_BLOCKING_SYNCHRONOUS_METHOD()`:** For synchronous methods (use with caution).

### Android (Java/Kotlin)

1.  **Create a Class:** Create a new Java or Kotlin class that extends `ReactContextBaseJavaModule`.
2.  **`@ReactModule` Annotation:** Annotate the class with `@ReactModule`.
3.  **`getName()` Method:** Override `getName()` to return the module's name as it will be accessed in JavaScript.
4.  **`@ReactMethod` Annotation:** Annotate methods that you want to expose to JavaScript.
5.  **Register the Module:** Add the module to your `Package` class, which is then registered with the `ReactNativeHost`.

## Example (Conceptual)

```javascript
// JavaScript side
import { NativeModules } from 'react-native';
const { CalendarModule } = NativeModules;

CalendarModule.createCalendarEvent('Party', 'My House');
```

```objective-c
// iOS Native Module (Objective-C)
#import <React/RCTBridgeModule.h>

@interface CalendarModule : NSObject <RCTBridgeModule>
@end

@implementation CalendarModule

RCT_EXPORT_MODULE();

RCT_EXPORT_METHOD(createCalendarEvent:(NSString *)name location:(NSString *)location)
{
  NSLog(@"Create event %@ at %@", name, location);
}

@end
```

```java
// Android Native Module (Java)
package com.your-app-name;

import com.facebook.react.bridge.NativeModule;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.bridge.ReactContext;
import com.facebook.react.bridge.ReactContextBaseJavaModule;
import com.facebook.react.bridge.ReactMethod;

import java.util.Map;
import java.util.HashMap;

public class CalendarModule extends ReactContextBaseJavaModule {
   CalendarModule(ReactApplicationContext context) {
       super(context);
   }

   @Override
   public String getName() {
       return "CalendarModule";
   }

   @ReactMethod
   public void createCalendarEvent(String name, String location) {
       System.out.println("Create event " + name + " at " + location);
   }
}
```