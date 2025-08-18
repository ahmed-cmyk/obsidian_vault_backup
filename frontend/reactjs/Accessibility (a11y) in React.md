# Accessibility (a11y) in React

### Why Accessibility Matters

Accessibility ensures that your React application is usable by everyone, including people with disabilities.
It improves user experience, expands your audience, and is required by law in many regions (e.g., WCAG guidelines, ADA compliance).

---
## Key Concepts

* **Semantic HTML:** Use proper HTML elements (`<button>`, `<form>`, `<label>`, etc.) instead of generic `<div>` or `<span>`.

* **ARIA (Accessible Rich Internet Applications):** Attributes that provide additional information to assistive technologies like screen readers (e.g., `aria-label`, `aria-hidden`).

* **Keyboard Navigation:** All interactive elements should be reachable and operable via keyboard (using `Tab`, `Enter`, `Space`, etc.).

* **Focus Management:** Ensure the correct element receives focus when views change or modals open.

* **Color Contrast & Visual Design:** Text and UI elements must meet minimum contrast ratios to be readable for users with visual impairments.

---
## Tools for Testing Accessibility

* **axe DevTools:** Browser extension and Node library to detect accessibility issues.
* **Lighthouse:** Chrome DevTools audit tool that reports accessibility score and problems.
* **NVDA / VoiceOver / JAWS:** Screen readers for testing.
* **eslint-plugin-jsx-a11y:** Linter plugin to enforce accessibility best practices in JSX.

---
## React-Specific Tips

* Use libraries like `react-aria` or `downshift` for accessible components.

* Avoid adding `onClick` to `<div>`s — use `<button>`.

* Add `role` attributes where appropriate (`role="dialog"`, `role="navigation"`, etc.).

* Manage focus explicitly when opening modals or dynamically rendering components using `ref` and `.focus()`.

---
## Example: Accessible Button

```jsx
function AccessibleButton({ onClick }) {
  return (
    <button onClick={onClick} aria-label="Close dialog">
      ✖
    </button>
  );
}
```

---
## Best Practices

* Test your app with a keyboard and screen reader.
* Provide text alternatives for images (`alt` attributes).
* Announce dynamic content changes with ARIA live regions.
* Don’t rely solely on color to convey information.

---
## Common Mistakes to Avoid

* Ignoring semantic HTML in favor of custom components.
* Missing `alt` text on images.
* Poor color contrast ratios.
* Forgetting to trap focus in modals/dialogs.
* Not testing with assistive technology.

---

#reactjs