# 🌐 Computing Society – Session 1: HTML Foundations  
### *Frontend Basics: Creating Your First Web Page*

Welcome to your first Computing Society workshop!  
In this session, we’ll focus on **HTML**, the backbone of all websites.  
By the end, you’ll understand how to structure a webpage and apply what you’ve learned to a practical task.

---

## 🎯 Learning Goals
1. Understand what **HTML** is and its function in web development.  
2. Learn the structure and syntax of basic HTML tags.  
3. Build a simple **personal webpage** step-by-step.  
4. Apply your knowledge through a **timed challenge**.

---

## 🧠 Understanding HTML

### 💬 Starter Question
> “When you open a website, what do you think tells your browser what to display?”

### 💡 Explanation
**HTML (HyperText Markup Language)** provides the **structure** for every webpage.  
It defines what content is shown and how it is organized.

Think of it like constructing a house:

| House Element | Website Equivalent | Purpose |
|----------------|--------------------|----------|
| Walls & Rooms | **HTML** | Defines *what appears* on the page |
| Paint & Decoration | **CSS** | Defines *how it looks* |

---

## 🏗️ Step 1: Setting Up Your HTML File

1. Open **VS Code** (or any text editor).  
2. Create a new file named **`index.html`**.  
3. Enter the following code:

```html
<!DOCTYPE html>
<html>
<head>
    <title>My First Page</title>
</head>
<body>
    <h1>Hello, World!</h1>
    <p>This is my first webpage.</p>
</body>
</html>
```

💡 **Notes:**
- The `<head>` section contains information *about* the page (not visible to users).  
- The `<body>` section contains everything *that users see*.  

✅ **Try it out:**  
Save → Right-click → *Open in Browser*  
You should see the heading and paragraph displayed.

---

## ✨ Step 2: Personalize Your Page

Replace the content within `<body>` with your own details:

```html
<h1>Your Name</h1>
<img src="https://placekitten.com/200/200" alt="Profile Photo">
<h2>About Me</h2>
<p>I’m a Computing Society member and I love building things!</p>

<h2>My Hobbies</h2>
<ul>
    <li>Coding</li>
    <li>Music</li>
    <li>Reading</li>
</ul>

<a href="https://www.example.com">My Favorite Website</a>
```

### 💬 Discussion Prompts
- “Why use `<h1>` for the main title and `<h2>` for subheadings?”  
- “What is the purpose of the `alt` attribute in images?”

---

### 🧩 Core HTML Tag Reference

| Tag             | Function                                    | Example                                    |
| --------------- | ------------------------------------------- | ------------------------------------------ |
| `<div>`         | Defines a block-level section or container  | `<div><p>Content inside a div</p></div>`   |
| `<span>`        | Defines an inline section or text container | `<span style="color:red;">Red text</span>` |
| `<h1>`–`<h6>`   | Headings                                    | `<h1>Main Title</h1>`                      |
| `<p>`           | Paragraph                                   | `<p>Sample text</p>`                       |
| `<img>`         | Image                                       | `<img src="cat.jpg" alt="Cat">`            |
| `<ul>` / `<li>` | List                                        | `<ul><li>Item</li></ul>`                   |
| `<a>`           | Link                                        | `<a href="https://example.com">Visit</a>`  |


---

## 🧠 Step 3: Applied Task – “Mini Portfolio” Challenge

### 🎯 Objective
Apply everything learned to create a **personal portfolio webpage** using **only HTML**.

---

### 🧩 Scenario
You are preparing a short personal introduction for the Computing Society’s website.  
Your task: build a **one-page portfolio** to showcase who you are.

---

### ⏱ Time Allocation: 15 Minutes
- 2 min → Read the instructions  
- 10 min → Build your page  
- 3 min → Self-check and peer review  

---

### 📋 Task Requirements
Your webpage must include **all** of the following:

1. A **title** displayed in the browser tab.  
2. A **main heading (`<h1>`)** with your full name.  
3. A **profile image (`<img>`)** with proper `alt` text.  
4. An **About Me** paragraph (`<p>`).  
5. A **list (`<ul>` or `<ol>`)** with **three of the classes you attend**.  
6. A **hyperlink (`<a>`)** to an external website.  
7. A **footer (`<p>`)** saying “© 2025 Computing Society”.  
8. At least one **`<div>`** to group related content.  
9. At least one **`<span>`** to highlight or style a piece of inline text.

Make sure your HTML follows correct structure:
```html
<!DOCTYPE html>
<html>
<head> ... </head>
<body> ... </body>
</html>
```

---

### 🧾 Evaluation Criteria

| Category | Description | 
|-----------|--------------|
| ✅ Structure | Includes all required elements | 
| 🧠 Syntax | Correct tag usage, nesting, and attributes | 
| 💬 Content | Clear, accurate, and complete | 
| 🌐 Functionality | Image and link display correctly | 
| 🕒 Neatness | Completed on time and formatted clearly |

---

### 📤 Review Process
1. Open your page in the browser.  
2. Exchange with a partner for a **1-minute peer check**.  

---





## 🎨 Part 3: Making It Pretty (CSS)

### What is CSS?

CSS (Cascading Styles Sheets) is a language used to control the **presentation** and **layout** of web pages. It allows you to change things like:

*   **Colors:** Text color, background color, border color.
*   **Fonts:** The style of the text (e.g., Arial, Times New Roman), its size, and weight (boldness).
*   **Layout:** Where elements are positioned on the page (e.g., side-by-side, stacked, centered).
*   **Spacing:** The margin (space outside an element) and padding (space inside an element).
*   **Effects:** Rounded corners, shadows, and animations.

### How Does CSS Work?

The core concept of CSS is very simple. You write **rules** that tell the browser how to style specific HTML elements.

A CSS rule is made up of two main parts:

1.  **Selector:** This identifies *which* HTML element(s) you want to style (e.g., `h1`, `p`, `img`).
2.  **Declaration Block:** This contains one or more *declarations* inside curly braces `{ }` that define *what* the style should be.

Each **declaration** is a `property: value;` pair.

```css
selector {
    property: value;
    another-property: another-value;
}
```

**Let's break down a real example from our code:**

```css
h1 {
    color: darkblue;
}
```

*   **Selector:** `h1` (This targets all `<h1>` heading elements on the page.)
*   **Declaration:** `color: darkblue;`
    *   **Property:** `color` (This changes the text color.)
    *   **Value:** `darkblue` (This sets the text color to dark blue.)

You can add multiple declarations to style an element in many ways at once!

```css
h1 {
    color: darkblue;        /* Changes text color */
    text-align: center;     /* Aligns text to the center */
    font-family: Arial;     /* Changes the font */
}
```

---

### 🪄 Step 1: Add CSS to Your Page

Now that we know the basics, let's put it into practice! We’ll write our CSS *inside* the same HTML file for now.

Add this **inside the `<head>`** tag, above `</head>`:

```html
<style>
    body {
        background-color: lightblue;
        text-align: center;
        font-family: Arial, sans-serif;
    }

    h1 {
        color: darkblue;
    }

    img {
        border-radius: 50%;
        width: 150px;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    a {
        text-decoration: none;
        color: green;
        font-weight: bold;
    }

    a:hover {
        color: darkgreen;
    }
</style>
```

### 🔁 Step 2: Refresh Your Page

Save and refresh your browser again — you should see:

* A light blue background
* All content centered
* A rounded profile picture
* A list without bullets
* Styled links that change color when you hover!

---

### 🧠 Challenge #2

Try editing the CSS:

* Change the `background-color` to your favorite color.
* Make your heading (`h1`) a different `color`.
* Change the `font-family` to something else, like `"Courier New"`.
* Add more hobbies or experiment with `font-size`.

💡 Don’t be afraid to experiment! That’s the best way to learn CSS. If something breaks, you can always undo it.
---

## 🏁 Wrap-Up

Today, you learned how to:

1. Write and open your first HTML page.
2. Use basic tags (`<h1>`, `<p>`, `<ul>`, `<a>`, `<img>`).
3. Style your page using CSS inside a `<style>` tag.
4. Experiment and make it your own.

Your final product is a **personal profile card** — a great first step toward building full websites!

---

### 🧠 Quick Reference Cheat Sheet

| HTML Tag        | Purpose      | Example                                      |
| --------------- | ------------ | -------------------------------------------- |
| `<h1>`          | Main heading | `<h1>Hello!</h1>`                            |
| `<p>`           | Paragraph    | `<p>This is text.</p>`                       |
| `<img>`         | Image        | `<img src="cat.jpg" alt="Cat">`              |
| `<a>`           | Link         | `<a href="https://example.com">Click Me</a>` |
| `<ul>` / `<li>` | List         | `<ul><li>Item</li></ul>`                     |

| CSS Property       | Meaning         | Example                        |
| ------------------ | --------------- | ------------------------------ |
| `color`            | Text color      | `color: red;`                  |
| `background-color` | Page background | `background-color: lightblue;` |
| `font-family`      | Text font       | `font-family: Arial;`          |
| `text-align`       | Text alignment  | `text-align: center;`          |
| `border-radius`    | Rounded corners | `border-radius: 10px;`         |

---

🎉 **Congratulations!**
You just built your very first webpage and learned the foundations of frontend web development.
