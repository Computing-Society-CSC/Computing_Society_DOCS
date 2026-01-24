# 💻 Computing Society — Session 5: *APIs in Action*  
### From Web Pages to Data Exchange  

---

## 🎯 Goal of the Session
By the end of this session, everyone will:
- **Set up a new Flask project** with proper environment structure
- **Understand what an API is** through simple, real-world examples
- **Get data from other websites' APIs** using Python
- **Create our own API** that serves data using Flask
- **Connect a web page to our API** using JavaScript

---

## 🕐 Duration · ~1.5 hours  

---

## 0️⃣ Project Setup — *Creating Our API Environment* (10 min)

### Setting Up Your Development Environment

Before we start coding, let's set up a clean project structure:

```bash
# Create a new folder for our API project
mkdir api-session-project
cd api-session-project

# Create a Conda environment with Python
conda create -n api-session python=3.11

# Activate the environment
conda activate api-session

# Install the packages we need
pip install flask requests
```

### 📁 Project Structure
Create these files in your project folder:

```
api-session-project/
│
├── app.py                 # Our main Flask application
├── templates/             # HTML templates folder
│   └── events.html       # Our events page template
└── README.md             # Project notes (optional)
```

### 🧪 Quick Test - Basic Flask App
Let's make sure everything works:

```python
# app.py
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "🚀 API Session is working!"

if __name__ == '__main__':
    app.run(debug=True)
```

**Run your app:**
```bash
python app.py
```

Visit `http://127.0.0.1:5000` - you should see "🚀 API Session is working!"

---

## 1️⃣ Warm-Up — *What Is an API?* (10 min)

### Let's Start with What We Know
Remember when we built Flask websites with forms? The user filled out a form, clicked submit, and got a new page. That's **user-to-server** communication.

**Today we're learning: server-to-server communication!**

### 🧩 API (Application Programing Interface) = Restaurant Menu Analogy

Think of an API like a **restaurant menu**:

| What's Happening | Restaurant Version | API Version |
|:--|:--|:--|
| You want something | Look at the menu | Check API documentation |
| You place your order | "I'd like pizza" | Request `/api/pizza` |
| You get what you asked for | Server brings pizza | API returns pizza data |
| The format | Plate with food | JSON data format |

**Key Insight:** With regular web pages, you get HTML (the whole webpage with styling). With APIs, you get just the **raw data** (like ingredients without the plate).

---

## 2️⃣ Getting Data from Other APIs (25 min)

### Let's Try It Out!

We'll use Python to ask another website for data. First, let's update our imports:

```python
# Add to the top of app.py
from flask import Flask, jsonify, render_template
import requests

app = Flask(__name__)
```

Now let's try the Cat Facts API:

```python
# This is like calling a restaurant and ordering
response = requests.get("https://catfact.ninja/fact")

# The restaurant prepares our order
data = response.json()

# We get our food (data) and eat it (use it)
print("🐱 Cat Fact:", data["fact"])
```

### 🛠️ Let's Break This Down:

1. **`requests.get()`** = "Hey API, can I have this data please?"
2. **`response.json()`** = "Let me read this data in a way Python understands"
3. **`data["fact"]`** = "Now let me get the specific piece I want"

### 📋 Understanding API Responses

When you ask an API for data, it tells you how things went:

| Status Code | What It Means | Real Life Example |
|:--|:--|:--|
| **200 OK** | Everything worked! | "Your pizza is ready!" |
| **404 Not Found** | The API endpoint doesn't exist | "We don't have that on our menu" |
| **500 Server Error** | The API has problems | "Our kitchen is on fire!" |

### 🧠 Your Turn - Mini Challenge

**Try different APIs!** Here are some fun ones:

```python
# Cat facts API
@app.route('/cat-fact')
def cat_fact():
    response = requests.get("https://catfact.ninja/fact")
    data = response.json()
    return f"<p>🐱 Fact: {data['fact']}</p>"

# Joke API  
@app.route('/joke')
def joke():
    response = requests.get("https://official-joke-api.appspot.com/random_joke")
    data = response.json()
    return f"<p>{data['setup']} ... {data['punchline']}</p>"
```

**Your Task:** Pick one API from [publicapis.dev](https://publicapis.dev/) and make it work in Flask!

---

## 3️⃣ Building Our Own API with Flask (30 min)

### Wait... We Can Make APIs Too?

Yes! Instead of always returning web pages, Flask can return **pure data**.

### From Web Pages to Data Endpoints

**Before (HTML page):**
```python
@app.route('/members')
def show_members():
    members = ["Alice", "Bob", "Charlie"]
    return render_template('members.html', members=members)
```

**Now (API endpoint):**
```python
@app.route('/api/members')
def get_members():
    members = ["Alice", "Bob", "Charlie"]
    return jsonify({
        "count": len(members),
        "members": members
    })
```

### 🔍 What's Different?

| `render_template()` | `jsonify()` |
|:--|:--|
| Returns HTML webpage | Returns raw data |
| Browser shows pretty page | Browser shows structured data |
| For humans to read | For computers to read |

**Try it!** Visit `http://127.0.0.1:5000/api/members` and see the difference.

### 📝 API Actions Explained

Think of CRUD operations:

| Action | HTTP Method | Real Life Example |
|:--|:--|:--|
| **GET** | Read data | Looking at a menu |
| **POST** | Create new data | Placing a new order |
| **PUT** | Update data | Changing your order |
| **DELETE** | Remove data | Cancelling your order |

---

### 🧩 Your Turn - Greeting API

**Build this step by step:**

```python
# Step 1: Basic greeting
@app.route('/api/greet/<name>')
def greet(name):
    return jsonify({"message": f"Hello, {name}!"})

# Step 2: Make it fancier!
@app.route('/api/greet/<name>')
def greet(name):
    return jsonify({
        "greeting": f"Hello, {name}!",
        "timestamp": "2024-01-01 10:00:00",
        "language": "English"
    })
```

**Test it:** Go to `http://127.0.0.1:5000/api/greet/YourName`

---

## 4️⃣ Connecting Web Pages to Our API (20 min)

### The Magic Part: Websites Talking to Themselves!

Now we'll make a web page that can get data from our API **without refreshing**.

### HTML + JavaScript Side: create index.html in templates

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Random Quote Demo</title>
</head>
<body>
    <h2>Get Random Quote</h2>
    <button id="get-quote-btn">Get Quote</button>
    <div id="quote-result"></div>

    <script>
    document.getElementById("get-quote-btn").onclick = async function() {
        const response = await fetch("/api/random-quote");
        const data = await response.json();
        document.getElementById("quote-result").innerHTML =
            `<p>"${data.quote}" - ${data.author}</p>`;
    };
    </script>
</body>
</html>
```

### Flask Side:

```python
@app.route('/api/random-quote')
def random_quote():
    response = requests.get("https://catfact.ninja/fact")
    data = response.json()

    return jsonify({
        "quote": data["fact"],
        "author": "Cat Facts API"
    })

@app.route('/quote-page')
def fact_page():
    return render_template('index.html')
```



### 🎯 How This Works:

1. **User clicks button** in browser
2. **JavaScript** calls our Flask API
3. **Flask** gets data from external API
4. **Flask** returns data to JavaScript
5. **JavaScript** updates the page

**No page refresh needed!** This is called **AJAX**.

---

## 5️⃣ Mini Project (15 min)

### 🎯 Build a Club Events API

**Step 1 - Create the API endpoint:**
```python
@app.route('/api/events')
def get_events():
    events = [
        {
            "title": "Session 5 — Databases & CRUD",
            "date": "Nov 17, 2024",
            "description": "Storing and retrieving data"
        },
        {
            "title": "Session 6 — Final Projects",
            "date": "Nov 24, 2024", 
            "description": "Building your own web applications"
        }
    ]
    return jsonify({"events": events})
```

**Step 2 - Create the web page:**
```html
<!-- events.html -->
<h2>Upcoming Sessions</h2>
<div id="events-container">
    <p>Loading events...</p>
</div>

<script>
// When page loads, get events automatically
window.onload = async function() {
    const response = await fetch("/api/events");
    const data = await response.json();
    
    let html = "";
    data.events.forEach(event => {
        html += `
            <div class="event-card">
                <h3>${event.title}</h3>
                <p><strong>Date:</strong> ${event.date}</p>
                <p>${event.description}</p>
            </div>
        `;
    });
    
    document.getElementById("events-container").innerHTML = html;
};
</script>
```

**Step 3 - Connect them in Flask:**
```python
@app.route('/events')
def show_events():
    return render_template('events.html')
```

---

## 6️⃣ Wrap-Up (5 min)

### ✅ What We Learned Today:

| Concept | What It Means | Why It Matters |
|:--|:--|:--|
| **Project Setup** | Creating environments & structure | Clean, organized code |
| **API** | How programs talk to each other | Apps can share data |
| **JSON** | Universal data language | Everyone understands it |
| **requests** | Python's way to get API data | Access endless information |
| **jsonify()** | Flask's way to serve data | Our apps can share too |
| **fetch()** | JavaScript's way to get data | Dynamic websites |

### 🌟 Key Insight:
**Regular websites** are for **people** to read.
**APIs** are for **computers** to talk to each other.

### 🎓 Try This at Home:
Build a **"Club Dashboard"** that shows:
- Random inspirational quote
- Next meeting date from your API  
- Number of club members
- Weather for meeting day

### 🔧 Remember Your Environment:
```bash
# Next time you work on this project:
conda activate api-session
python app.py
```

---
