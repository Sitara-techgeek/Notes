# HTML

> **📅 Written:** 13 Jul 2026

### Definition

**HTML (HyperText Markup Language) is the standard markup language used to structure content on the web — it defines what each piece of content *is* (a heading, a paragraph, a link, an image) using tags.**

**In one sentence:**
> HTML is the skeleton of a webpage — it doesn't make things look pretty (that's CSS) or behave interactively (that's JavaScript), it just says "this is a heading, this is a list, this is a link."

**Example:**

```html
<h1>Welcome</h1>
<p>This is a paragraph of text.</p>
<a href="https://example.com">Click here</a>
```

---

## Key Features of HTML

- Made up of **elements**, written as tags: `<tagname>content</tagname>`
- Most tags come in **pairs** — an opening tag and a closing tag
- Some tags are **self-closing** (void elements) — e.g. `<img>`, `<br>`, `<input>`
- Tags can carry **attributes** for extra info — `<img src="cat.jpg" alt="A cat">`
- Browsers read HTML top to bottom and render it into the **DOM** (Document Object Model)

---

# Categorized Tags

> **📅 Written:** 13 Jul 2026

## 1. Document Structure Tags

### Definition

The skeleton every HTML file starts with — sets up the document before any visible content appears.

| Tag | Explanation |
| --- | --- |
| `<!DOCTYPE html>` | Tells the browser this is an HTML5 document. |
| `<html>` | The root element that wraps the entire page. |
| `<head>` | Holds metadata — title, links to CSS, meta tags — nothing visible here. |
| `<body>` | Holds everything the user actually sees on the page. |

**Example:**

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My Page</title>
  </head>
  <body>
    <p>Visible content goes here.</p>
  </body>
</html>
```

---

## 2. Text Tags

### Definition

Used to structure and emphasize written content — headings, paragraphs, and inline text formatting.

| Tag | Explanation |
| --- | --- |
| `<h1>` to `<h6>` | Headings, `h1` being the largest/most important and `h6` the smallest — used in order for structure, not just size. |
| `<p>` | A paragraph of text. |
| `<span>` | An inline container with no meaning of its own — used to style or target a small piece of text. |
| `<b>` / `<strong>` | Makes text bold; `<strong>` also signals real importance to screen readers. |
| `<i>` / `<em>` | Makes text italic; `<em>` also signals emphasis to screen readers. |
| `<br>` | A single line break (self-closing). |
| `<hr>` | A horizontal rule — a thematic break line (self-closing). |

**Example:**

```html
<h1>Main Title</h1>
<h2>Subheading</h2>
<p>This is a <strong>bold and important</strong> paragraph.</p>
<span>Just a small inline piece of text.</span>
```

---

## 3. Link Tags

### Definition

Used to create clickable references to other pages, sections, or resources.

| Tag | Explanation |
| --- | --- |
| `<a>` | The anchor tag — creates a hyperlink using the `href` attribute. |

**Example:**

```html
<a href="https://example.com">Visit Example</a>
<a href="#section2">Jump to Section 2</a>
<a href="mailto:hi@example.com">Email Me</a>
```

---

## 4. Image & Media Tags

### Definition

Used to embed visual or media content directly into the page.

| Tag | Explanation |
| --- | --- |
| `<img>` | Embeds an image using the `src` attribute (self-closing). |
| `<video>` | Embeds a video, with playback controls optional. |
| `<audio>` | Embeds an audio clip. |

**Example:**

```html
<img src="cat.jpg" alt="A sleeping cat">
<video src="clip.mp4" controls></video>
```

---

## 5. List Tags

### Definition

Used to group related items together, either ordered or unordered.

| Tag | Explanation |
| --- | --- |
| `<ul>` | An unordered (bulleted) list. |
| `<ol>` | An ordered (numbered) list. |
| `<li>` | A single list item — used inside `<ul>` or `<ol>`. |

**Example:**

```html
<ul>
  <li>Milk</li>
  <li>Eggs</li>
</ul>

<ol>
  <li>Wake up</li>
  <li>Brush teeth</li>
</ol>
```

---

## 6. Table Tags

### Definition

Used to display data in a row-and-column grid.

| Tag | Explanation |
| --- | --- |
| `<table>` | Wraps the entire table. |
| `<tr>` | A table row. |
| `<th>` | A table header cell (bold, centered by default). |
| `<td>` | A regular table data cell. |

**Example:**

```html
<table>
  <tr>
    <th>Name</th>
    <th>Age</th>
  </tr>
  <tr>
    <td>Sitara</td>
    <td>21</td>
  </tr>
</table>
```

---

## 7. Form & Input Tags

### Definition

Used to collect input from the user — text, choices, clicks, submissions.

| Tag | Explanation |
| --- | --- |
| `<form>` | Wraps a group of inputs meant to be submitted together. |
| `<input>` | A single input field — its `type` attribute decides text/checkbox/radio/etc (self-closing). |
| `<textarea>` | A multi-line text input box. |
| `<button>` | A clickable button. |
| `<select>` / `<option>` | A dropdown menu and its selectable options. |
| `<label>` | A caption tied to a specific input, usually via the `for` attribute. |

**Example:**

```html
<form>
  <label for="name">Name:</label>
  <input type="text" id="name" placeholder="Enter your name">
  <button type="submit">Submit</button>
</form>
```

---

## 8. Semantic / Sectioning Tags

### Definition

Tags that describe the **role** of a section of content, not just its appearance — this is what makes HTML more meaningful to browsers, search engines, and screen readers.

| Tag | Explanation |
| --- | --- |
| `<header>` | Introductory content for a page or section — often a logo/title/nav. |
| `<nav>` | A block of navigation links. |
| `<main>` | The primary, unique content of the page (one per page). |
| `<section>` | A generic, thematically grouped chunk of content. |
| `<article>` | Self-contained content that could stand alone (a blog post, a news story). |
| `<footer>` | Closing content for a page or section — often copyright, contact links. |

**Example:**

```html
<header><h1>My Blog</h1></header>
<nav><a href="#">Home</a> | <a href="#">About</a></nav>
<main>
  <article>
    <h2>Post Title</h2>
    <p>Post content...</p>
  </article>
</main>
<footer><p>© 2026 My Blog</p></footer>
```

---

# Common Attributes

> **📅 Written:** 13 Jul 2026

### Definition

**Attributes are extra pieces of information added inside the opening tag to configure or describe an element — they always live in `name="value"` form.**

| Attribute | Used On | Explanation |
| --- | --- | --- |
| `src` | `<img>`, `<video>`, `<script>` | Points to the file/resource to load. |
| `href` | `<a>`, `<link>` | The destination URL of a link. |
| `alt` | `<img>` | Alternative text shown/read if the image fails to load — critical for accessibility. |
| `id` | any element | A unique identifier for that one element, used to target it in CSS/JS. |
| `class` | any element | A reusable label to group/style multiple elements together. |
| `style` | any element | Inline CSS applied directly to that element. |
| `title` | any element | Tooltip text shown on hover. |
| `placeholder` | `<input>`, `<textarea>` | Faded hint text shown before the user types anything. |
| `type` | `<input>`, `<button>` | Specifies the kind of input/button (text, checkbox, submit, etc). |
| `disabled` | `<input>`, `<button>` | Greys out and disables the element from user interaction. |

**Example:**

```html
<img src="dog.jpg" alt="A brown dog" title="My dog" class="pet-photo">
<a href="https://example.com" id="main-link">Visit Site</a>
<input type="email" placeholder="you@example.com">
```

---

## Quick Revision

- **HTML** → Markup language that structures web content using tags.
- **Structure tags** → `<html>`, `<head>`, `<body>` — the document's skeleton.
- **Text tags** → `<h1>-<h6>`, `<p>`, `<span>`, `<strong>`, `<em>`.
- **Link tags** → `<a>` with `href`.
- **Media tags** → `<img>`, `<video>`, `<audio>` with `src`.
- **List tags** → `<ul>`/`<ol>` + `<li>`.
- **Table tags** → `<table>`, `<tr>`, `<th>`, `<td>`.
- **Form tags** → `<form>`, `<input>`, `<button>`, `<select>`, `<label>`.
- **Semantic tags** → `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<footer>` — meaning, not just layout.
- **Attributes** → Extra config inside the opening tag: `src`, `href`, `alt`, `id`, `class`, `style`, `type`.

---

## Interview Definition ⭐

> **HTML is the markup language that structures web content into meaningful elements — headings, paragraphs, links, images, lists, forms — using tags and attributes, which browsers then parse into the DOM for rendering.**