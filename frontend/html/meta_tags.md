# HTML: Meta Tags

Meta tags are HTML tags that provide metadata about an HTML document. Metadata is data about data. Meta tags are placed in the `<head>` section of an HTML page and are not displayed on the web page itself. They are used by browsers (how to display content or reload page), search engines (keywords), and other web services.

## Basic Meta Tags

### 1. Character Set (`charset`)

Specifies the character encoding for the document. `UTF-8` is the recommended and most common encoding, supporting almost all characters and symbols in the world.

```html
<meta charset="UTF-8">
```

### 2. Viewport (`viewport`)

Crucial for responsive web design. It controls the viewport's width and scaling on different devices.

*   `width=device-width`: Sets the width of the viewport to the width of the device.
*   `initial-scale=1.0`: Sets the initial zoom level when the page is first loaded by the browser.

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### 3. Description (`description`)

Provides a brief summary of the web page's content. This is often displayed by search engines in search results.

```html
<meta name="description" content="A comprehensive guide to HTML meta tags.">
```

### 4. Keywords (`keywords`)

Historically used by search engines to understand page content. Its importance has significantly diminished, and it's largely ignored by major search engines like Google.

```html
<meta name="keywords" content="HTML, meta tags, web development, SEO">
```

### 5. Author (`author`)

Specifies the author of the document.

```html
<meta name="author" content="John Doe">
```

## Other Useful Meta Tags

### Refresh (`http-equiv="refresh"`)

Instructs the browser to refresh the page after a specified number of seconds, optionally redirecting to another URL.

```html
<!-- Refresh page every 30 seconds -->
<meta http-equiv="refresh" content="30">

<!-- Redirect to example.com after 5 seconds -->
<meta http-equiv="refresh" content="5; url=https://www.example.com">
```

### Compatibility (`http-equiv="X-UA-Compatible"`)

Used to specify the document compatibility mode for Internet Explorer. `IE=edge` tells IE to use the highest available rendering mode.

```html
<meta http-equiv="X-UA-Compatible" content="IE=edge">
```

## Open Graph Protocol (for Social Media)

Open Graph (OG) meta tags are used by social media platforms (like Facebook, Twitter, LinkedIn) to control how URLs are displayed when shared. They turn your web pages into rich objects in a social graph.

*   `og:title`: The title of your object as it should appear within the graph.
*   `og:type`: The type of object (e.g., `website`, `article`, `video.movie`).
*   `og:image`: An image URL which should represent your object within the graph.
*   `og:url`: The canonical URL of your object that will be used as its permanent ID in the graph.
*   `og:description`: A one to two sentence description of your object.

```html
<meta property="og:title" content="My Awesome Article">
<meta property="og:type" content="article">
<meta property="og:image" content="https://www.example.com/images/article-thumbnail.jpg">
<meta property="og:url" content="https://www.example.com/my-awesome-article">
<meta property="og:description" content="This article explains everything about meta tags.">
```

## Twitter Cards

Similar to Open Graph, Twitter Cards allow you to add rich media experiences to Tweets that link to your content.

*   `twitter:card`: The type of Twitter Card (e.g., `summary`, `summary_large_image`, `app`, `player`).
*   `twitter:site`: The Twitter @username of the website.
*   `twitter:title`: Title of the content.
*   `twitter:description`: Description of the content.
*   `twitter:image`: URL of image to use in the card.

```html
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:site" content="@yourtwitterhandle">
<meta name="twitter:title" content="My Awesome Article">
<meta name="twitter:description" content="This article explains everything about meta tags.">
<meta name="twitter:image" content="https://www.example.com/images/article-thumbnail.jpg">
```

Meta tags are crucial for controlling how your web pages behave and appear across different platforms, from search engines to social media. Properly configuring them can significantly impact your site's visibility and user engagement.