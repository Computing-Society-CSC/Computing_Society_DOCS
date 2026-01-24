## 🧩 **Session: Applying Flask Basics — GET/POST & Templates**

### **Goal**

By the end of the session, everyone can:

* Build a small web page that takes user input (via a form) and displays it.
* Understand Flask’s `GET` and `POST` request methods.
* Use Jinja2 template inheritance (`extends`, `block`) and variables (`{{ variable }}`).
* Beginners get a crash course in Python syntax used in Flask apps.

---

### **0. Warm-up (5 min)**

**Activity:**

* Ask: “What’s the difference between a URL parameter and form input?”
* Show an example URL like `/hello?name=Tom` → introduce GET query string.

---

### **1. Quick Python Refresher (10 min)**

**For beginners; quick examples shown live**

* Variables, lists, and dictionaries
* Functions (`def`), indentation
* `if/else`, loops (`for item in list:`)
* How Flask routes use decorators like `@app.route()`

**Tip for advanced members:** ask them to assist others or tweak the examples (e.g., add input validation).

---

### **2. Mini Flask Review (5 min)**

* Recap how Flask runs: routes → functions → templates
* Difference between:

  ```python
  return render_template('index.html', name='Tom')
  ```

  and

  ```python
  return 'Hello Tom'
  ```

---

### **3. Main Task (30 min)**

**Project:** *A Mini Greeting App*

**Requirements:**

1. `/` → shows a form asking for name and favorite color (`method="POST"`).
2. When submitted, redirect to `/greet` using `POST`, and display:

   ```
   Hello {{ name }}! Your favorite color is {{ color }}.
   ```
3. Use template inheritance:

   * `base.html` (with navigation bar or title)
   * `index.html` extends `base.html`
   * `greet.html` extends `base.html`

**Optional challenge (for advanced students):**

* Save each name-color pair in a global list and display all submissions below.

---

### **4. Sharing & Reflection (10 min)**

* Volunteers show their version.
* Discuss: When to use GET vs POST?
* What benefits does Jinja2’s template system bring over hardcoding HTML?

---

### **🧠 Key Takeaways**

* `GET` = retrieve data, visible in URL
* `POST` = send data, hidden in body
* Jinja2 allows logic-free HTML templates
* Flask route functions return rendered templates
