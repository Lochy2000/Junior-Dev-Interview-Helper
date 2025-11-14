# HTML Fundamentals - Complete Guide for Junior Developers

> Essential HTML concepts, semantic markup, accessibility, and interview questions

## 📋 Table of Contents

- [What is HTML?](#what-is-html)
- [Basic HTML Structure](#basic-html-structure)
- [Semantic HTML Elements](#semantic-html-elements)
- [Forms and Input](#forms-and-input)
- [Accessibility (a11y)](#accessibility-a11y)
- [SEO Best Practices](#seo-best-practices)
- [Common Interview Questions](#common-interview-questions)
- [Code Examples](#code-examples)

---

## What is HTML?

**HTML (HyperText Markup Language)** is the standard markup language for creating web pages. It describes the structure and content of a webpage using elements (tags).

### Key Concepts
- HTML is **not** a programming language—it's a **markup language**
- Elements are defined by tags: `<tagname>content</tagname>`
- Attributes provide additional information: `<tag attribute="value">`
- HTML5 is the latest version with semantic elements and APIs

---

## Basic HTML Structure

Every HTML document follows this basic structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page Title - Appears in Browser Tab</title>
    <meta name="description" content="SEO description for search engines">
</head>
<body>
    <!-- Your content goes here -->
    <h1>Main Heading</h1>
    <p>Paragraph text</p>
</body>
</html>
```

### Explanation
- `<!DOCTYPE html>` - Declares this is an HTML5 document
- `<html lang="en">` - Root element, `lang` helps screen readers and search engines
- `<head>` - Contains metadata (not visible on page)
- `<body>` - Contains visible content

---

## Semantic HTML Elements

**Semantic elements** clearly describe their meaning to both the browser and developer.

### Why Use Semantic HTML?
✅ Better accessibility for screen readers
✅ Improved SEO (search engines understand structure)
✅ Easier to read and maintain
✅ Better mobile experience

### Common Semantic Elements

| Element | Purpose | Example Use |
|---------|---------|-------------|
| `<header>` | Page or section header | Site logo, navigation |
| `<nav>` | Navigation links | Main menu, breadcrumbs |
| `<main>` | Main content (unique per page) | Primary page content |
| `<article>` | Self-contained content | Blog post, news article |
| `<section>` | Thematic grouping | Chapter, tab panel |
| `<aside>` | Tangentially related content | Sidebar, pullquote |
| `<footer>` | Footer for page or section | Copyright, links |

### Example Structure

```html
<body>
    <header>
        <h1>My Website</h1>
        <nav>
            <ul>
                <li><a href="#home">Home</a></li>
                <li><a href="#about">About</a></li>
            </ul>
        </nav>
    </header>

    <main>
        <article>
            <h2>Article Title</h2>
            <p>Article content...</p>
        </article>
    </main>

    <aside>
        <h3>Related Links</h3>
    </aside>

    <footer>
        <p>&copy; 2024 My Website</p>
    </footer>
</body>
```

---

## Forms and Input

Forms are essential for user interaction and data collection.

### Basic Form Structure

```html
<form action="/submit" method="POST">
    <label for="username">Username:</label>
    <input type="text" id="username" name="username" required>

    <label for="email">Email:</label>
    <input type="email" id="email" name="email" required>

    <label for="password">Password:</label>
    <input type="password" id="password" name="password" minlength="8" required>

    <button type="submit">Submit</button>
</form>
```

### Input Types

- `text` - Single-line text
- `email` - Email with validation
- `password` - Hidden text
- `number` - Numeric input
- `tel` - Telephone number
- `url` - Web address
- `date` - Date picker
- `checkbox` - Multiple selection
- `radio` - Single selection from group
- `file` - File upload
- `submit` - Submit button

### Form Attributes

- `required` - Field must be filled
- `placeholder` - Hint text
- `minlength`/`maxlength` - Character limits
- `pattern` - Regex validation
- `disabled` - Cannot interact
- `readonly` - Can see but not edit

---

## Accessibility (a11y)

Making your HTML accessible ensures everyone can use your website.

### Best Practices

#### 1. Use Semantic HTML
```html
<!-- ❌ Bad -->
<div class="button" onclick="submit()">Click Me</div>

<!-- ✅ Good -->
<button onclick="submit()">Click Me</button>
```

#### 2. Add Alt Text to Images
```html
<!-- ❌ Bad -->
<img src="logo.png">

<!-- ✅ Good -->
<img src="logo.png" alt="Company Logo">
```

#### 3. Use ARIA Labels When Needed
```html
<button aria-label="Close menu">
    <span aria-hidden="true">&times;</span>
</button>
```

#### 4. Ensure Keyboard Navigation
- All interactive elements should be reachable via Tab key
- Use `tabindex` appropriately (0 for normal order, -1 to remove from order)

#### 5. Color Contrast
- Text should have sufficient contrast ratio (WCAG AA: 4.5:1 for normal text)

---

## SEO Best Practices

### 1. Use Proper Heading Hierarchy

```html
<!-- ✅ Good -->
<h1>Main Page Title</h1>
  <h2>Section Title</h2>
    <h3>Subsection</h3>

<!-- ❌ Bad - Skipping levels -->
<h1>Main Title</h1>
  <h3>Subsection</h3> <!-- Skipped h2 -->
```

### 2. Meta Tags

```html
<head>
    <title>Page Title - Brand Name (Under 60 chars)</title>
    <meta name="description" content="Brief description under 160 characters for search results">
    <meta name="keywords" content="html, tutorial, web development">

    <!-- Open Graph for social sharing -->
    <meta property="og:title" content="Page Title">
    <meta property="og:description" content="Description for social media">
    <meta property="og:image" content="image-url.jpg">
</head>
```

### 3. Structured Data

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Article Title",
  "author": "Author Name",
  "datePublished": "2024-01-01"
}
</script>
```

---

## Common Interview Questions

### 1. What is the difference between `<div>` and `<span>`?
**Answer**: `<div>` is a block-level element (takes full width, starts on new line), while `<span>` is inline (only takes needed width, doesn't break flow).

### 2. What does `<!DOCTYPE html>` do?
**Answer**: It tells the browser this is an HTML5 document and to render it in standards mode (not quirks mode).

### 3. What is semantic HTML and why is it important?
**Answer**: Semantic HTML uses elements that clearly describe their purpose (like `<article>`, `<nav>`). Important for accessibility, SEO, and code maintainability.

### 4. Explain the difference between `id` and `class`.
**Answer**:
- `id` - Unique identifier, only one per page, higher CSS specificity
- `class` - Can be reused multiple times, lower specificity

### 5. What are data attributes?
**Answer**: Custom attributes that start with `data-`, used to store extra information:
```html
<div data-user-id="123" data-role="admin">User Info</div>
```

### 6. What is the purpose of the `alt` attribute in images?
**Answer**: Provides alternative text when image can't load, crucial for screen readers (accessibility) and SEO.

### 7. What's the difference between `<script>`, `<script async>`, and `<script defer>`?
**Answer**:
- Normal: Blocks HTML parsing, executes immediately
- `async`: Downloads in parallel, executes when ready (may block parsing)
- `defer`: Downloads in parallel, executes after HTML parsing completes

### 8. What are void/self-closing elements?
**Answer**: Elements without closing tags: `<img>`, `<br>`, `<hr>`, `<input>`, `<meta>`, `<link>`

### 9. What is the `viewport` meta tag?
**Answer**: Controls page dimensions and scaling on mobile devices:
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### 10. Explain `<strong>` vs `<b>`, and `<em>` vs `<i>`.
**Answer**:
- `<strong>` - Semantic importance (SEO/accessibility), usually bold
- `<b>` - Visual styling only, bold
- `<em>` - Semantic emphasis, usually italic
- `<i>` - Visual styling only, italic

---

## Code Examples

### Complete Landing Page Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Learn HTML basics with this comprehensive tutorial">
    <title>HTML Tutorial - Complete Beginner's Guide</title>
</head>
<body>
    <header>
        <nav aria-label="Main navigation">
            <ul>
                <li><a href="#home">Home</a></li>
                <li><a href="#about">About</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
        </nav>
    </header>

    <main>
        <section id="hero">
            <h1>Welcome to HTML Basics</h1>
            <p>Learn to build web pages from scratch</p>
            <a href="#learn" class="cta-button">Start Learning</a>
        </section>

        <section id="features">
            <h2>What You'll Learn</h2>
            <article>
                <h3>Semantic HTML</h3>
                <p>Master modern HTML5 elements</p>
            </article>
            <article>
                <h3>Accessibility</h3>
                <p>Build inclusive web experiences</p>
            </article>
        </section>

        <section id="contact">
            <h2>Get In Touch</h2>
            <form action="/submit" method="POST">
                <div>
                    <label for="name">Name:</label>
                    <input type="text" id="name" name="name" required>
                </div>
                <div>
                    <label for="email">Email:</label>
                    <input type="email" id="email" name="email" required>
                </div>
                <div>
                    <label for="message">Message:</label>
                    <textarea id="message" name="message" rows="5" required></textarea>
                </div>
                <button type="submit">Send Message</button>
            </form>
        </section>
    </main>

    <footer>
        <p>&copy; 2024 HTML Tutorial. All rights reserved.</p>
        <nav aria-label="Footer navigation">
            <a href="/privacy">Privacy</a>
            <a href="/terms">Terms</a>
        </nav>
    </footer>
</body>
</html>
```

---

## Resources

- [MDN Web Docs - HTML](https://developer.mozilla.org/en-US/docs/Web/HTML)
- [W3C HTML Specification](https://html.spec.whatwg.org/)
- [Can I Use](https://caniuse.com/) - Browser compatibility
- [HTML Validator](https://validator.w3.org/)
- [WebAIM](https://webaim.org/) - Accessibility resources

---

**Keywords**: HTML tutorial, HTML5, semantic HTML, web development basics, HTML interview questions, HTML forms, accessibility, SEO, HTML elements, beginner HTML guide
