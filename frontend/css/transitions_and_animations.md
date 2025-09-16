# CSS: Transitions and Animations

CSS transitions and animations allow you to create dynamic and engaging user interfaces without relying on JavaScript. They provide smooth changes to CSS properties over time, enhancing the user experience.

## 1. CSS Transitions

CSS transitions provide a way to control the speed of animation when changing CSS properties. Instead of an abrupt change, the change happens smoothly over a specified duration.

### `transition` Shorthand Property

```css
transition: property duration timing-function delay;
```

*   `property`: The CSS property to which the transition is applied (e.g., `width`, `height`, `background-color`, `all`).
*   `duration`: The time it takes for the transition to complete (e.g., `2s`, `500ms`). Required.
*   `timing-function`: The speed curve of the transition.
    *   `ease` (default): Slow start, then fast, then slow end.
    *   `linear`: Same speed from start to end.
    *   `ease-in`: Slow start.
    *   `ease-out`: Slow end.
    *   `ease-in-out`: Slow start and end.
    *   `cubic-bezier(n,n,n,n)`: Custom curve.
*   `delay`: Specifies a delay before the transition starts (e.g., `1s`).

```html
<div class="box transition-example"></div>

<style>
.box {
  width: 100px;
  height: 100px;
  background-color: blue;
  transition: width 2s, background-color 1s ease-in 0.5s; /* Multiple transitions */
}

.box:hover {
  width: 200px;
  background-color: red;
}
</style>
```

### Individual Transition Properties

*   `transition-property`
*   `transition-duration`
*   `transition-timing-function`
*   `transition-delay`

```css
.box {
  transition-property: width;
  transition-duration: 2s;
  transition-timing-function: ease-out;
  transition-delay: 0.5s;
}
```

## 2. CSS Animations

CSS animations allow you to create more complex, multi-step animations. They involve two key parts:

1.  **`@keyframes`**: Defines the animation sequence.
2.  **`animation` property**: Applies the `@keyframes` to an element.

### `@keyframes` Rule

Defines the animation. You specify the styles at various points (keyframes) during the animation sequence.

*   `from` (0%) and `to` (100%): Define the start and end states.
*   Percentages: Define intermediate states.

```css
@keyframes mymove {
  from { background-color: red; }
  to { background-color: blue; }
}

@keyframes slidein {
  0%   { transform: translateX(0%); }
  50%  { transform: translateX(100px); opacity: 0.5; }
  100% { transform: translateX(0%); opacity: 1; }
}
```

### `animation` Shorthand Property

```css
animation: name duration timing-function delay iteration-count direction fill-mode play-state;
```

*   `name`: The name of the `@keyframes` rule to bind to the selector.
*   `duration`: The time an animation takes to complete one cycle.
*   `timing-function`: The speed curve of the animation.
*   `delay`: A delay before the animation starts.
*   `iteration-count`: The number of times an animation should be played (`infinite` for endless).
- `direction`: Whether the animation should play forwards, backwards, or alternate.
    *   `normal` (default)
    *   `reverse`
    *   `alternate`
    *   `alternate-reverse`
*   `fill-mode`: Specifies a style for the target element when the animation is not playing (before it starts, after it ends, or both).
    *   `none` (default)
    *   `forwards`: Retains the style values from the last keyframe.
    *   `backwards`: Applies the style values from the first keyframe.
    *   `both`: Applies both forwards and backwards rules.
*   `play-state`: Specifies whether the animation is running or paused.
    *   `running` (default)
    *   `paused`

### Individual Animation Properties

*   `animation-name`
*   `animation-duration`
*   `animation-timing-function`
*   `animation-delay`
*   `animation-iteration-count`
*   `animation-direction`
*   `animation-fill-mode`
*   `animation-play-state`

---
## Performance Considerations

*   **Animate `transform` and `opacity`**: These properties are generally more performant to animate because they don't trigger layout recalculations or repaints of other elements. Avoid animating `width`, `height`, `margin`, `padding`, `top`, `left`, etc., if possible, as they can be costly.
*   **Hardware Acceleration**: Browsers can often use the GPU to render animations involving `transform` and `opacity`, leading to smoother results.

CSS transitions and animations are powerful tools for adding visual flair and interactivity to your web pages, improving the overall user experience.

---
