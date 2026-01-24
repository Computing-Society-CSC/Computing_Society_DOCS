# 💻 Computing Society — Session 3: *Advanced Flask Concepts*

## 🎯 Goal of the Session
By the end of this session, everyone will:
- Understand **HTTP methods** (GET vs POST) and form handling
- Learn about **Flask request object** and data processing
- Build **interactive forms** with validation
- Explore **session management** and user data persistence

---

## HTTP Methods & Flask Request Object (25 min)

### 🧠 Understanding HTTP Methods

| Method | Purpose | Real-World Example |
|:--|:--|:--|
| **GET** | Retrieve data | Loading a webpage, search results |
| **POST** | Send/submit data | Login forms, file uploads, purchases |
| **PUT** | Update existing data | Editing profile information |
| **DELETE** | Remove data | Deleting a post or account |

**Key Difference:**
- **GET**: Data visible in URL, bookmarked, cached. Example: when you google "Cat Photos"
- **POST**: Data hidden, more secure, no length limits. Example: when you submit your Common Application

https://www.youtube.com/watch?v=tkfVQK6UxDI

### 🔧 Flask Request Object

**What is the request object?**
- A global object that contains all incoming request data
- Gives access to form data, URL parameters, cookies, files, etc.

**app.py:**
```python
from flask import Flask, render_template, request

app = Flask(__name__)

# Default method is GET
@app.route('/search')
def search():
    # Access URL parameters: /search?query=python
    query = request.args.get('query', '')  # Gets 'query' parameter or empty string
    return f"Search results for: {query}"

# Handling both GET and POST
@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        # Access form data submitted via POST
        username = request.form['username']
        password = request.form['password']
        
        # Process login logic here
        return f"Welcome {username}! Login successful."
    
    # If GET request, show the login form
    return '''
    <form method="POST">
        Username: <input type="text" name="username"><br>
        Password: <input type="password" name="password"><br>
        <input type="submit" value="Login">
    </form>
    '''
```

### 🔍 Deep Dive: Request Object Methods

| Method | Purpose | Example |
|:--|:--|:--|
| `request.args` | URL parameters (GET) | `?name=John` → `request.args.get('name')` |
| `request.form` | Form data (POST) | `<input name="email">` → `request.form['email']` |
| `request.files` | Uploaded files | `request.files['document']` |
| `request.cookies` | Browser cookies | `request.cookies.get('session')` |
| `request.method` | HTTP method | `request.method == 'POST'` |

---

## Building Interactive Forms (30 min)

### **What Are Forms? (The Website's Questionnaires)**

Think of a web form as a digital questionnaire or order form. Just like you fill out a form to join a club or order food, web forms are how users *send* information *to* a website.

*   **On the Front End (What you see):** A form is made up of familiar elements: text boxes for your name, checkboxes for preferences, dropdown menus for choices, and a submit button. This is all built with HTML.

*   **On the Back End (The brain):** When you hit "Submit," your answers are bundled up and sent to the Flask server. Flask then uses the **`request` object** to "open" this bundle and read the data you sent (`request.form['name']`, `request.form['email']`, etc.). It can then validate the information, save it, or use it to show you a new page (like a "Thank You" message).

### 🎨 Creating a Professional Contact Form

**templates/contact.html:**
```html
{% extends "base.html" %}

{% block content %}
<div class="contact-form">
    <h2>Contact Us</h2>
    
    {% with messages = get_flashed_messages() %}
        {% if messages %}
            <div class="alert">
                {% for message in messages %}
                    <p>{{ message }}</p>
                {% endfor %}
            </div>
        {% endif %}
    {% endwith %}
    
    <form method="POST" action="{{ url_for('contact') }}">
        <div class="form-group">
            <label for="name">Full Name:</label>
            <input type="text" id="name" name="name" required 
                   value="{{ request.form.get('name', '') }}">
        </div>
        
        <div class="form-group">
            <label for="email">Email:</label>
            <input type="email" id="email" name="email" required
                   value="{{ request.form.get('email', '') }}">
        </div>
        
        <div class="form-group">
            <label for="subject">Subject:</label>
            <select id="subject" name="subject">
                <option value="">Select a subject...</option>
                <option value="membership">Membership Inquiry</option>
                <option value="event">Event Question</option>
                <option value="technical">Technical Support</option>
                <option value="other">Other</option>
            </select>
        </div>
        
        <div class="form-group">
            <label for="message">Message:</label>
            <textarea id="message" name="message" rows="5" required>{{ request.form.get('message', '') }}</textarea>
        </div>
        
        <button type="submit">Send Message</button>
    </form>
</div>

<style>
.form-group { margin-bottom: 15px; }
label { display: block; margin-bottom: 5px; font-weight: bold; }
input, select, textarea { 
    width: 100%; 
    padding: 8px; 
    border: 1px solid #ddd; 
    border-radius: 4px; 
}
.alert { 
    background: #d4edda; 
    color: #155724; 
    padding: 10px; 
    border-radius: 4px; 
    margin-bottom: 15px; 
}
</style>
{% endblock %}
```

### 🔧 Enhanced app.py with Form Handling

```python
from flask import Flask, render_template, request, redirect, url_for, flash

app = Flask(__name__)
app.secret_key = 'your-secret-key-here'  # Needed for flash messages

@app.route('/contact', methods=['GET', 'POST'])
def contact():
    if request.method == 'POST':
        # Get form data
        name = request.form.get('name', '').strip()
        email = request.form.get('email', '').strip()
        subject = request.form.get('subject', '')
        message = request.form.get('message', '').strip()
        
        # Basic validation
        errors = []
        
        if not name:
            errors.append('Please enter your name.')
        if not email or '@' not in email:
            errors.append('Please enter a valid email address.')
        if not subject:
            errors.append('Please select a subject.')
        if not message or len(message) < 10:
            errors.append('Please enter a message with at least 10 characters.')
        
        if errors:
            # Show error messages
            for error in errors:
                flash(error)
            return render_template('contact.html')
        
        # If validation passes - process the form
        # In a real app, you might save to database or send email
        flash(f'Thank you {name}! Your message has been sent.')
        return redirect(url_for('contact_success', name=name))
    
    # GET request - show empty form
    return render_template('contact.html')

@app.route('/contact/success')
def contact_success():
    name = request.args.get('name', 'Visitor')
    return render_template('success.html', name=name)
```
---

## Session Management & User Data (20 min)

### 🧠 Understanding Sessions

**What are sessions?**
- A way to store user-specific data across multiple requests
- Like a "memory" that remembers user information between page loads
- Stored server-side with a session ID in client's cookie

### 🔧 Implementing User Sessions

**Enhanced app.py:**
```python
from flask import Flask, render_template, request, redirect, url_for, flash, session
import datetime

app = Flask(__name__)
app.secret_key = 'a-very-secret-key-12345'  # Change this in production!

@app.route('/')
def home():
    # Track visits using session
    if 'visit_count' in session:
        session['visit_count'] += 1
    else:
        session['visit_count'] = 1
        session['first_visit'] = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
    
    return render_template('home.html')

@app.route('/user/profile', methods=['GET', 'POST'])
def user_profile():
    if request.method == 'POST':
        # Save user preferences to session
        session['username'] = request.form.get('username', 'Guest')
        session['theme'] = request.form.get('theme', 'light')
        session['notifications'] = 'notifications' in request.form
        
        flash('Profile updated successfully!')
        return redirect(url_for('user_profile'))
    
    # Pre-fill form with session data
    profile_data = {
        'username': session.get('username', ''),
        'theme': session.get('theme', 'light'),
        'notifications': session.get('notifications', False)
    }
    
    return render_template('profile.html', profile=profile_data)

@app.route('/logout')
def logout():
    # Clear session data
    session.clear()
    flash('You have been logged out.')
    return redirect(url_for('home'))
```

**templates/home.html:**
```html
{% extends "base.html" %}

{% block content %}
<div class="dashboard">
    <h2>Welcome to Computing Society!</h2>
    
    <div class="session-info">
        <h3>Your Session Information:</h3>
        <p><strong>Visit Count:</strong> {{ session.get('visit_count', 0) }}</p>
        <p><strong>First Visit:</strong> {{ session.get('first_visit', 'Just now!') }}</p>
        <p><strong>Username:</strong> {{ session.get('username', 'Not set') }}</p>
        <p><strong>Theme:</strong> {{ session.get('theme', 'light') }}</p>
    </div>
    
    <div class="actions">
        <a href="{{ url_for('user_profile') }}" class="btn">Edit Profile</a>
        {% if session.get('username') %}
            <a href="{{ url_for('logout') }}" class="btn btn-secondary">Logout</a>
        {% endif %}
    </div>
</div>

<style>
.session-info {
    background: #f8f9fa;
    padding: 15px;
    border-radius: 5px;
    margin: 20px 0;
}
.btn { 
    display: inline-block; 
    padding: 10px 20px; 
    background: #007bff; 
    color: white; 
    text-decoration: none; 
    border-radius: 4px; 
    margin-right: 10px; 
}
.btn-secondary { background: #6c757d; }
</style>
{% endblock %}
```
---

## Summary & Next Steps (5 min)

### 🎓 What We Learned Today
- ✅ **HTTP Methods**: GET vs POST and when to use each
- ✅ **Request Object**: Accessing form data, URL parameters, and files
- ✅ **Form Handling**: Building interactive forms with validation
- ✅ **Session Management**: Remembering user data across requests

**Remember:** Practice makes perfect! Try building your own project ideas using these concepts. 🚀

---

*Happy Coding! 👨💻👩💻*
