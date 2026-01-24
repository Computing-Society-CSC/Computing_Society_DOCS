# 💻 Computing Society — Session 3: *Introduction to Flask*  
### From Static HTML to Dynamic Web Apps  

---

## 🎯 Goal of the Session
By the end of this session, everyone will:
- Understand **what Flask is** and **where it fits** in web development.  
- See how the **front end** (HTML + CSS) connects with the **back end** (Python + Flask).  
- Build a simple **interactive webpage** that responds to user input.

---

## 🕐 Duration · ~1.5 hours  
**Tools Needed:** VS Code · Python 3 · Browser · Flask (we’ll install together)

---

## 1️⃣ Warm-Up — From Front End to Back End (10 min)

**Purpose:** Bridge what we learned last time (HTML + CSS) to something new (Python on the server).

> 💬 **Discussion:**  
> – What happens when you click a button or submit a form?  
> – Static pages can’t “think.” They only show what’s already written.  
> – Flask will be our “brain” that makes pages change.

---

## 2️⃣ What Is Flask? (20 min) — *Beginner-Friendly Explanation*

### 🧩 Where Flask Fits in Web Development

| Layer | What It Does | Example Tools / Languages |
|:--|:--|:--|
| **Front end (client side)** | Everything users see and interact with in the browser. | HTML · CSS · JavaScript |
| **Back end (server side)** | Handles logic & data; talks to the database and sends results back to the browser. | **Flask (Python)** · Django · Node.js |
| **Database** | Stores information (users, messages, etc.) | SQLite · MySQL · MongoDB |

➡️ Flask belongs to the **back-end layer** and is written in **Python**.

---

### 🧠 So What Exactly *Is* Flask?

Flask is a **web framework** — a ready-made structure that helps you build a web server with Python.  

-  A Python package that provides functions to create and run web apps.  

> Flask saves you from writing the low-level plumbing of a web server yourself.

---

### 🍽️ Flask Website Analogy

* **Website = Restaurant**
  The whole web application is like a restaurant that serves users (customers).

* **Users = Customers**
  They visit the restaurant (website) to get what they want — in this case, dishes (webpages).

* **Data = Ingredients**
  The raw materials used to make dishes — like data stored in a database (e.g., user info, product details).

* **Flask = Chefs and Waiters**
  Flask handles the *logic* (cooking) and *serving* (sending responses to users):

  * **Chefs** prepare dishes → Flask processes data and renders HTML pages.
  * **Waiters** deliver dishes → Flask routes the right content to users based on their requests.

* **Dishes = Webpages**
  The final product displayed to customers — nicely prepared and served dynamic web pages.

Flask gives you:  
- A mini web server to handle requests  
- “Routes” to decide what page to show  
- Templates to mix Python data with HTML  
- Debug mode for easy testing  

---

### ⚙️ Inside Flask
Flask is built on two Python packages:  
- **Werkzeug** – handles requests and responses  
- **Jinja2** – lets you insert Python variables into HTML (`{{ name }}`)

---

## 3️⃣ Building Your First Flask App (25 min)

### Step 0 - Create a new environment

create a project folder with your preferred name(`hello_flask`)

open Anaconda (if Windows) or Terminal (if Mac/Linux)

```bash
conda create --name flask_env python=3.11 flask
```
Why do we use python 3.11 version instead of the latest version 3.14? Because version 3.14 is not compatible with Flask yet.

Next, activate your environment:
```bash
conda activate flask_env
```

### Step 1 — Install Flask

Now we are going to install the packages in the Flask module into the enivornment:
```bash
pip install flask
```

**Verification:**
```bash
python3 -m flask --version
```
*Expected output: Shows Flask version and dependencies*

### Step 2 — Project Structure
```
flask_intro/
├── app.py
└── templates/
```

### Step 3 — Core Application Code
**app.py:**
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Hello, Flask!"

if __name__ == '__main__':
    app.run(debug=True)
```

**Running the application:**
```bash
python app.py
```
*Visit http://127.0.0.1:5000/ to see your webpage*

### 🔍 Deep Dive: Flask Application Object
- `Flask(__name__)` creates the application instance
- `__name__` helps Flask locate templates and static files
- The app object handles routing and request processing

### 🧠 Route Explanation
```python
@app.route('/')
def home():
    return "Hello, Flask!"
```
- `@app.route()` is a **decorator** that maps URLs to functions
- `/` represents the root URL (homepage)
- The function returns the HTTP response content

### 🧩 Mini Challenge: Adding Routes
```python
@app.route('/about')
def about():
    return "This is our Computing Society website!"

@app.route('/contact')
def contact():
    return "Contact us at: ****@uwcchina.org"
```

## 4️⃣ Flask + HTML Templates (25 min)

### Template Folder Structure
```
flask_intro/
├── app.py
└── templates/
    ├── index.html
    └── base.html
```

### Step 1: Creating Templates
**templates/index.html:**
```html
<!DOCTYPE html>
<html>
<head>
  <title>{{ title }}</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 40px; }
    .header { color: #2c3e50; }
  </style>
</head>
<body>
  <h1 class="header">Welcome to {{ club_name }}!</h1>
  <p>We meet every Wednesday at 6 PM.</p>
</body>
</html>
```

### Step 2: Enhanced app.py
```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def home():
    return render_template(
        'index.html',
        title='Home Page',
        club_name='Computing Society'
    )

@app.route('/events')
def events():
    return render_template(
        'index.html', 
        title='Upcoming Events',
        club_name='CompSoc Events'
    )
```

### 🔍 Deep Dive: Jinja2 Template Engine
**Variable Syntax:**
- `{{ variable }}` - Outputs variable content
- `{% statement %}` - Control structures
- `{# comment #}` - Template comments

**Template Inheritance Example:**
**templates/base.html:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}{% endblock %} - CompSoc</title>
</head>
<body>
    <nav>Navigation Bar</nav>
    <main>{% block content %}{% endblock %}</main>
    <footer>© 2024 Computing Society</footer>
</body>
</html>
```

**templates/index.html:**
```html
{% extends "base.html" %}

{% block title %}Home{% endblock %}

{% block content %}
    <h1>Welcome to {{ club_name }}!</h1>
    <p>Dynamic content here</p>
{% endblock %}
```

### 🧩 Mini Challenge: Template Enhancement
```python
@app.route('/members')
def members():
    members_list = ['Alice', 'Bob', 'Charlie', 'Diana']
    return render_template(
        'members.html',
        title='Our Members',
        members=members_list,
        count=len(members_list)
    )
```

**templates/members.html:**
```html
{% extends "base.html" %}

{% block content %}
<h1>Our Members ({{ count }})</h1>
<ul>
    {% for member in members %}
    <li>{{ member }}</li>
    {% endfor %}
</ul>
{% endblock %}
```

## 5️⃣ Adding User Interaction (15 min)

### Enhanced Form Handling
**templates/index.html:**
```html
<form action="/greet" method="post">
    <label for="username">Your Name:</label>
    <input type="text" id="username" name="username" 
           placeholder="Enter your name" required>
    <button type="submit">Say Hello!</button>
</form>

<!-- Alternative GET method form -->
<form action="/search" method="get">
    <input type="text" name="query" placeholder="Search...">
    <button type="submit">Search</button>
</form>
```

### Enhanced app.py with Multiple Methods
```python
from flask import Flask, render_template, request, redirect, url_for

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html', title='Home')

@app.route('/greet', methods=['POST'])
def greet():
    name = request.form.get('username', 'Anonymous')
    return f"""
    <h1>Hello, {name}!</h1>
    <p>Welcome to the Computing Society!</p>
    <a href="/">Back to Home</a>
    """

@app.route('/search', methods=['GET'])
def search():
    query = request.args.get('query', '')
    return f"<h2>Search Results for: {query}</h2>"

# Dynamic URL routes
@app.route('/user/<username>')
def user_profile(username):
    return f"<h1>User Profile: {username}</h1>"

@app.route('/post/<int:post_id>')
def show_post(post_id):
    return f"<h2>Displaying Post #{post_id}</h2>"
```

This is only a small part of flask. You can learn more about flask in the official document!
