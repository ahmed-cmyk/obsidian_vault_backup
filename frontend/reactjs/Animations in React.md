# Animations in React

## Why Animations?

Animations improve user experience by:

* Providing feedback (e.g., loading indicators)
* Drawing attention to important elements
* Making transitions between states smoother and more intuitive

In React, animations can be implemented declaratively or imperatively using CSS or JavaScript libraries.

---
## Techniques & Tools

### 1. CSS Transitions & Animations

* Ideal for **simple animations** (fade, slide, scale, etc.)
* You can use `className` toggling to trigger CSS rules.
* Example:

```jsx
.fade-enter {
  opacity: 0;
}
.fade-enter-active {
  opacity: 1;
  transition: opacity 300ms ease-in;
}
```

```jsx
<div className={isVisible ? 'fade-enter fade-enter-active' : 'fade-exit'} />
```

✅ Pros: Lightweight, hardware-accelerated
🚫 Cons: Limited control, no complex sequencing

---
### 2. React Transition Group

* A React library for managing the lifecycle of animations.
* Supports enter/exit animations, controlled via `CSSTransition` or `Transition`.

```jsx
import { CSSTransition } from 'react-transition-group';

<CSSTransition in={show} timeout={300} classNames="fade" unmountOnExit>
  <div>Content</div>
</CSSTransition>
```

✅ Pros: Well-integrated with React’s component lifecycle
🚫 Cons: Boilerplate for complex sequences

---
### 3. Framer Motion

* A powerful animation library built specifically for React.
* Supports animations, gestures, drag, spring physics, shared layouts.

```jsx
import { motion } from 'framer-motion';

<motion.div animate={{ opacity: 1 }} initial={{ opacity: 0 }} transition={{ duration: 0.5 }}>
  Content
</motion.div>
```

✅ Pros: Intuitive API, complex animations made easy, physics-based
🚫 Cons: Slightly larger bundle size compared to CSS

---
### 4. GSAP (GreenSock Animation Platform)

* A robust JavaScript animation library (not React-specific).
* Use when you need timeline-based animations, or highly complex interactions.

✅ Pros: Very powerful, fine control over animations
🚫 Cons: Imperative, heavier than CSS-based solutions

---
## Patterns & Best Practices

* Prefer declarative (e.g., Framer Motion) over imperative when possible.
* Keep animations short (usually 200ms–500ms) to avoid slowing down the UI.
* Always respect **reduced motion preferences** for accessibility:

```jsx
@media (prefers-reduced-motion: reduce) {
  * {
    animation: none !important;
    transition: none !important;
  }
}
```

* Animate transform and opacity for better performance (GPU-accelerated).

---
## Common Mistakes to Avoid

* Animating properties like `top`, `left`, `width`, or `height` instead of `transform` — which is more performant.
* Forgetting to clean up timers, intervals, or event listeners in imperative animations.
* Not handling exit animations properly (e.g., removing elements from the DOM before they finish animating).
* Ignoring accessibility for users who prefer reduced motion.

---

#reactjs