# Describing UI

A Lynx page may contain various visual elements such as text and images, presented in different layouts to create diverse page styles.

---
## **Element Tags**

Lynx defines content and structure using a markup language, with the most basic unit being an *element tag*.

The element tags in the source code will be parsed by the *Lynx Engine* at runtime and translated into elements for rendering. Lynx elements are designed to be platform-agnostic. 

> **NOTE:** The elements are rendered natively by the *Lynx Engine* into the UI primitives for each platforms, such as IOS and Android views, or HTML elements on the Web.

```tsx
<text>Hello Lynx</text>
```

**Attributes:**

```tsx
<text style="background:red;">Hello Lynx</text>
```

**Empty Element Tags:**

```tsx
<image src="assets/logo.png" />
```

Nested element tags will form a tree composed of elements, which we refer to as the *element tree*.

**Nested Element Tags:**

```tsx
<view>
	<text>Hello</text>
	<text>Lynx</text>
</view>
```

---