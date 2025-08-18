# Executing Scripts

You can execute your scripts using `NodeJS` or you could insert them into HTML documents using the `<script>` tag.

```html
<!DOCTYPE HTML>
<html>
<body>
  <p>Before the script...</p>
  <script>
    alert( 'Hello, world!' );
  </script>
  <p>...After the script.</p>
</body>
</html>
```

## The `<script>` tag

### The `type` attribute

In HTML4, the script was required to have a `type` attribute which would usually be `type="text/javascript"`. It’s no longer required.

### The `language` attribute

This attribute was meant to show the language of the script but now it’s no longer needed as JS is the default language.

## External Scripts

You can attach external scripts to HTML using the `src` attribute.

```html
<script src="/path/to/script.js"></script>
```

Attaching multiple scripts:

```html
<script src="/js/script1.js"></script>
<script src="/js/script2.js"></script>
```

> **IMPORTANT:**  The benefit of a separate file is that the browser will download it and store it in its [cache](https://en.wikipedia.org/wiki/Web_cache).

Other pages that reference the same script will take it from the cache instead of downloading it, so the file is actually downloaded only once. That reduces traffic and makes pages faster.

> **IMPORTANT:** A single <script> tag can’t have both the src attribute and code inside.

---

#javascript