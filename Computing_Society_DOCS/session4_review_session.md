# 💻 Computing Society — Session 4: Review & Application

---

## 🎯 Session Goals

By the end of this session, you will:

✅ Review key concepts from **HTML, CSS, and Flask**  
✅ Understand the **structure and logic** of a Flask project  
✅ Create a new mini Flask project independently  

🕒 Duration: ~1.5 hours  
🧰 Tools: VS Code · Python 3.11 · Flask · Browser  

---

## 🧱 Session Structure

0️⃣ Python & Project Basics 
1️⃣ HTML + CSS Quick Review  
2️⃣ Flask Basics Refresher  
3️⃣ Flask Project Walkthrough (Interactive)  
4️⃣ Build Your Own Flask Project  

---

# 0️⃣ Python & Project Basics (15 min)

## 🧠 What Is Python?

Python is a programming language that can be used for many things — data science, AI, automation, and web development.  
Flask is one of the many tools (built with Python) that helps us make websites.

---

## 📦 What Are Packages and Libraries?

| Term | Meaning | Example |
|:--|:--|:--|
| **Package** | A collection of Python files (group of functions or classes) used for a specific purpose | `flask`, `math`, `random` |
| **Library** | A set of packages that solve larger problems | NumPy (library for math + arrays) |
| **Importing** | Bringing these packages into your code so you can use them | `from flask import Flask` |

💬 **Example**  
```python
import math
print(math.sqrt(16))  # outputs 4
```
We’re using the `math` package to call its function `sqrt()`.

---

## 🧩 What Is a Project Structure?

Every Python project has a main file that the computer starts from.  
In Flask, that file is usually called `app.py`.

```
flask_intro/
│
├── app.py              # main program file (runs first)
├── templates/          # HTML files (frontend)
└── static/             # CSS or images
```

🧠 **Explain:**  
When you type `python app.py`, Python runs that file.  
Inside, Flask starts a mini server on your computer (localhost 5000).  
Each `@app.route()` inside `app.py` decides what to do when someone visits a URL.

---

## ⚙️ Quick Overview — How Does Everything Run?

1️⃣ You run `python app.py`  
2️⃣ Flask creates a local server (`127.0.0.1:5000`)  
3️⃣ The user visits a URL like `/` or `/contact`  
4️⃣ Flask finds the matching function (defined with `@app.route`)  
5️⃣ That function returns an HTML page from `templates/`  
6️⃣ The browser renders it for the user

---

# 1️⃣ HTML & CSS Review (15 min)

## 🧠 Quick Recap

- **HTML** → structure of the webpage  
- **CSS** → style and design  
- Together, they make websites readable and beautiful  

---

## 💬 Discussion

> “When you open a website, what tells your browser what to display?”  
> “Why separate HTML and CSS?”

✅ HTML gives structure  
✅ CSS gives appearance  

---

# 2️⃣ Flask Basics Refresher (20 min)

## 🧩 Flask in One Sentence

> Flask is a **Python web framework** that connects your **code** to your **website**.

---

## ⚙️ Flask Core Concepts

| Concept | Function | Example |
|:--|:--|:--|
| `Flask(__name__)` | Create the app object | `app = Flask(__name__)` |
| `@app.route()` | Connect URL → function | `@app.route('/')` |
| `render_template()` | Load HTML file | `return render_template('index.html')` |
| `request` | Access user input | `request.form['name']` |
| `session` | Store user data | `session['user'] = 'Alice'` |

---

## 💡 Key Idea

> “When someone visits a URL, Flask runs the function under that route — and returns what it produces.”

---

# 3️⃣ Flask Project Walkthrough (≈30 min)

🎯 Understand your **Session 3 & 3.5 project** in detail.  
We’ll go through each part of your code, **explain**, and **ask questions** to confirm understanding.

---

## 💬 Warm-Up Question

> “Who remembers what the *basic structure* of a Flask project looks like?”  
> “What are the three most important parts?”

---

## 📁 Flask Project Structure

```
flask_intro/
│
├── app.py
└── templates/
    ├── base.html
    ├── index.html
    ├── members.html
    └── contact.html
```

🧠 **Explain:**  
- `app.py` — main logic controller  
- `templates/` — HTML files  
- `static/` — optional folder for CSS or images  

✅ **Checkpoint:** Everyone can describe the 3 parts.

---

## 🧩 Step 1 · Creating the App Object

```python
from flask import Flask, render_template, request, redirect, url_for, flash, session
import datetime

app = Flask(__name__)
app.secret_key = 'a-very-secret-key-12345'
```

💬 **Ask:**  
> “Why do we need `Flask(__name__)`?”  

🧠 **Explain:**  
- Creates your web app  
- `secret_key` enables sessions & flash messages  

✅ **Checkpoint:** Understand what initializes Flask.

---

## 🧩 Step 2 · Defining Routes

```python
@app.route('/')
def home():
    return render_template('index.html', title='Home Page')
```

💬 **Ask:**  
> “When you visit `127.0.0.1:5000/`, which function runs?”  

🧠 **Explain:**  
`@app.route()` links URLs to functions;  
`render_template()` loads an HTML file to display.

✅ **Checkpoint:** Understand what a route does.

---

## 🧩 Step 3 · Templates (Jinja2)

`index.html`
```html
{% extends "base.html" %}
{% block content %}
<h1>Welcome to {{ club_name }}!</h1>
{% endblock %}
```

💬 **Ask:**  
> “What does `{{ club_name }}` mean here?”  

🧠 **Explain:**  
It displays Python data passed into `render_template()`.

✅ **Checkpoint:** Recognize `{{ variable }}` and `{% logic %}`.

---

## 🧩 Step 4 · Handling Forms (POST)

```python
@app.route('/greet', methods=['POST'])
def greet():
    name = request.form.get('username', 'Anonymous')
    return f"<h1>Hello, {name}!</h1>"
```

💬 **Ask:**  
> “When does this function run — on page load or after Submit?”  

🧠 **Explain:**  
Triggered only after the form submits via POST.  
`request.form.get()` grabs the user’s input.

✅ **Advanced Checkpoint:** Understand POST and `request.form`.

---

## 🧩 Step 5 · Handling GET

```python
@app.route('/search', methods=['GET'])
def search():
    query = request.args.get('query', '')
    return f"<h2>Search Results for: {query}</h2>"
```

💬 **Ask:**  
> “How do we test this route?”  

🧠 **Explain:**  
Visit `/search?query=python` — GET data appears in the URL.  
`request.args.get()` reads it.

✅ **Advanced Checkpoint:** Understand GET and URL parameters.

---

## 🧩 Step 6 · Session Management

```python
@app.route('/user/profile', methods=['GET', 'POST'])
def user_profile():
    if request.method == 'POST':
        session['username'] = request.form.get('username', 'Guest')
        flash('Profile updated!')
        return redirect(url_for('user_profile'))
    profile_data = {'username': session.get('username', '')}
    return render_template('profile.html', profile=profile_data)
```

💬 **Ask:**  
> “What happens if you refresh the page after updating?”  

🧠 **Explain:**  
Data stays! `session` stores user data between visits.  
`flash()` shows temporary messages.

✅ **Advanced Checkpoint:** Understand `session` and `flash`.

---

## 🧩 Step 7 · Logout + Redirect

```python
@app.route('/logout')
def logout():
    session.clear()
    flash('You have been logged out.')
    return redirect(url_for('home'))
```

💬 **Ask:**  
> “What happens to session data now?”  

🧠 **Explain:**  
`session.clear()` erases all stored data;  
`redirect()` sends user to another route.

✅ **Advanced Checkpoint:** Know how to clear session & redirect.

---

## 🧩 Step 8 · Front-End + Back-End Connection

`contact.html`
```html
<form method="POST" action="{{ url_for('contact') }}">
  <label>Name:</label>
  <input type="text" name="name" required>
  <button type="submit">Send</button>
</form>
```

`app.py`
```python
@app.route('/contact', methods=['GET', 'POST'])
def contact():
    if request.method == 'POST':
        name = request.form.get('name', '')
        flash(f'Thank you {name}! Your message has been sent.')
        return redirect(url_for('contact'))
    return render_template('contact.html')
```

💬 **Ask:**  
> “How does the form communicate with Flask?”  

🧠 **Explain:**  
Form → POST data → Flask reads via `request.form` → Response flashed to user.

✅ **Advanced Checkpoint:** Understand full request–response cycle.

---

## 🧾 Summary Table

| Level | You Should Be Able To… |
|:--|:--|
| **Basic** | Describe Flask structure, explain routes, templates, Jinja2 |
| **Advanced** | Differentiate GET/POST, use `request`, handle forms, manage sessions |

---

# 4️⃣ Create Your Own Flask Project (35 min)

## 🧠 Objective

Apply everything learned by **building your own mini web app**.

---

## 🪄 Instructions

1. Create a new folder  
2. Initialize a new Flask app (`app.py`)  
3. Add at least **two routes** (`/` and `/about`)  
4. Use `render_template()` and one HTML file  
5. Add CSS styling  

✅ Everyone completes this before leaving.  
* feel free to use AI
---

## 🚀 Optional Advanced Challenge

- Add a form using POST  
- Use `session` to remember data  
- Add a dynamic route (`/user/<name>`)  
- Include `flash()` or `redirect()`  

💡 Example:  
A “Welcome App” that greets each user and counts visits.

