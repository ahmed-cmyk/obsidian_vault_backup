# **React Native Learning Roadmap**

## **1. Foundation**

- **JavaScript Refresher**
    - ES6+ syntax (let/const, arrow functions, spread/rest)
    - Async/await & Promises
    - Array & Object methods (map, filter, reduce)
    - Modules & imports/exports
- **React Fundamentals**
    - Components (functional components, props, children)
    - State & useState
    - useEffect and lifecycle equivalents
    - Component composition & props drilling
    - Lists & keys
    - Forms & controlled components

---
## **2. React Native Core Concepts**

- RN project structure (App.js, package.json, node_modules)
- Core components:
    View, Text, Image, ScrollView, FlatList, SectionList
- Inputs: TextInput, Button, Pressable, TouchableOpacity
- Styling with StyleSheet.create
    - Flexbox layout (justifyContent, alignItems)
    - Percentages vs absolute units
    - Platform-specific styling (Platform.select)
- Dimensions & Responsive Design (Dimensions API)

---
## **3. Navigation**

- React Navigation
    - Stack Navigator
    - Bottom Tabs
    - Drawer Navigation
    - Passing params between screens
    - Navigation lifecycle hooks

---
## **4. State Management**

- `useContext` for simple global state
- Redux Toolkit or Zustand (state management libraries)
- Async storage for persistence
- React Query or SWR for server state

---
## **5. Networking**

- Fetching data with fetch or axios
- Handling errors, retries, and loading states
- Offline support & caching (React Query, MMKV)

---
## **6. Animations**

- React Native Reanimated (modern approach)
- `LayoutAnimation` (built-in, lightweight)
- Gesture Handler (for swipes, pans, long press)
- Lottie for animated illustrations

---
## **7. Forms**

- `react-hook-form` (highly recommended)
- Validation with yup or zod

---
## **8. Platform APIs**

- Camera (expo-camera or react-native-vision-camera)
- Location (`expo-location` or `react-native-geolocation`)
- Notifications (`expo-notifications` or `notifee`)
- Sensors (`expo-sensors`)

---
## **9. Performance**

- Optimizing `FlatList` (`keyExtractor`, `getItemLayout`)
- Memoization (`useMemo`, `useCallback`, `React.memo`)
- Avoiding re-renders
- Hermes engine & profiling

---
## **10. Testing**

- Unit Testing: Jest + React Native Testing Library
- E2E Testing: Detox

---
## **11. Deployment**

- Expo EAS Build (Android & iOS)
- Signing & Certificates
- OTA Updates (Expo Updates)
- App Store / Play Store submission

---
## **12. Advanced Topics**

- Native Modules (bridging native code)
- Push Notifications & Deep Linking
- Theming & Dark Mode
- Internationalization (i18n)
- Continuous Integration (GitHub Actions + EAS)

---
