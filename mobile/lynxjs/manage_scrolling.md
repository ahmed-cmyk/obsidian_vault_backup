# Manage Scrolling

You can control the scrolling direction of content through the `scroll-orientation` property.

> **IF** you have a non-scrollable element, then you can simply use the `overflow` property. Example: `overflow: hidden;`.

The `<view>` element does not support scrolling, and in place of that you should use specialized scrolling elements such as:

- `<scroll-view>`
- `<list>`

---
## `<scroll-view>`

`<scroll-view>` is a basic scrolling element in LynxJS. It allows users to scroll vertically or horizontally within a fixed viewport area.

To achieve the desired scrolling affect you only need to set the layout direction of the `scroll-orientation` property.

```tsx
<scroll-view scroll-orientation="vertical">
<scroll-view scroll-orientation="horizontal">
```

---
## `<list>`

The `<list>` element is useful for scenarios where a large amount of data needs to be presented, or in scenarios with infinite scrolling or loading for more content. It can demand an on-demand loading way, rendering only the content in the visible area.

```tsx
<list
	scroll-orientation="vertical"
	list-type="single"
	span-count={1}
>
```

You can also use `<list>` to handle more complex layouts such as `flow` and `waterfall`.

```tsx
<list
  className="list-container"
  list-type="waterfall"
  span-count={2}
  scroll-orientation="vertical"
>
```

### `<list>` options

| Value       | Description                                                                                                                                                                                                                                                                                 |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `single`    | Single-column/row layout                                                                                                                                                                                                                                                                    |
| `flow`      | Multi-column/row grid layout. The grid layout fully reflects regularity, with the `top` of adjacent child nodes in columns being consistent. Typically used for child nodes of uniform size. The width of child nodes is determined by the width of `<list>` and `span-count`.              |
| `waterfall` | Multi-column/row waterfall layout. Content is continuously filled from top to bottom into the shortest column, achieving visual continuity and dynamism. Typically used for child nodes of varying sizes. The width of child nodes is determined by the width of `<list>` and `span-count`. |

---