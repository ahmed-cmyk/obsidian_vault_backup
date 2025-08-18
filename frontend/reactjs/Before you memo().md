# Before you `memo()`

 **In general, if some state update is slow, you need to:**

1. Verify you’re running a production build. (Development builds are intentionally slower, in extreme cases even by an order of magnitude.)
2. Verify that you didn’t put the state higher in the tree than necessary. (For example, putting input state in a centralized store might not be the best idea.)
3. Run React DevTools Profiler to see what gets re-rendered, and wrap the most expensive subtrees with memo(). (And add useMemo() where needed.)

---

## Move `state` down

When dealing with `state` you should try to ensure that it is placed as far down the component tree as it possibly could. Doing so, prevents unnecessary rendering by isolating `state` only to the components that need it.

---

## Lift Content Up

Another way to avoid having to `useMemo` is to lift content up and then move it into its own isolated component. This component can act as the parent to other components that use the same `state`. 

**Source**: [Before you `memo()` | Overreacted](https://overreacted.io/before-you-memo/)

---
#reactjs