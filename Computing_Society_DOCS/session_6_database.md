# ⭐ SECTION 1 — What *is* a Database?

## 1.1 The Core Idea
A **database** is a system designed to **store, organize, and manage data** so that it is:

- **Persistent** (data does not disappear when the program closes)
- **Structured** (data follows a clear and predictable format)
- **Searchable** (can find exactly what you want quickly)
- **Reliable** (data stays correct and consistent)
- **Scalable** (works even with thousands or millions of records)

**Simple explanation:**  
> A database is like a super-organized digital filing cabinet.  
> Your program asks it questions, and the database replies fast and accurately.

---

## 1.2 Why Do We Need One?
Questions to think about:

- If you store data in **Python lists**, what happens when the program closes?
- How do you efficiently find “all users older than 16” from a huge list?
- What if two parts of a program try to edit data at the same time?
- How do you keep your data safe, consistent, and permanent?

A database solves all these issues.

---

## 1.3 Types of Databases (Simple Overview)

### **Relational Databases (RDBMS)**
Think of relational databases as **Excel sheets with rules and superpowers**:

- Data is stored in **tables**
- Tables have **rows** (records) and **columns** (fields)
- Most common type used in real applications
- Uses **SQL**

Examples: **SQLite, MySQL, PostgreSQL**

### **NoSQL Databases (awareness only)**
- Less structured  
- More flexible, used for huge or rapidly changing data  
Examples: MongoDB, Firebase

*For learning and most CSC projects → SQL databases are ideal.*

---

## 1.4 Key Vocabulary

### **Table**
A structured collection of data. Like one sheet in a spreadsheet.

### **Row**
One record or entry (e.g., one student).

### **Column**
A field/property (e.g., name, age, city).

### **Primary Key**
A unique identifier for each row.

### **Foreign Key** (not needed today)
A reference linking two tables together.

---

# ⭐ SECTION 2 — What is SQL?

https://youtu.be/zsjvFFKOm3c?si=Ww3thUawBNKZRePA

## 2.1 SQL = Structured Query Language
SQL is a **language for communicating with the database**.

A database does NOT understand Python, Java, or JavaScript —  
it understands **SQL commands**.

**Analogy:**  
> If the database is the librarian, SQL is the language you use to ask for books.

---

## 2.2 What SQL Is NOT
- SQL is **not** a full programming language  
- SQL cannot build apps by itself  
- SQL is **not** tied to Python — any language can send SQL commands  
- SQL does not handle loops or advanced logic (except inside queries)

---

## 2.3 What SQL *Can* Do (CRUD)

### **CREATE**  
Add new data → `INSERT`

### **READ**  
Look up existing data → `SELECT`

### **UPDATE**  
Modify data → `UPDATE`

### **DELETE**  
Remove data → `DELETE`

**Important idea:**  
> SQL describes *what you want*, not *how to do it*.  
> It’s a declarative language.

---

## 2.4 Why SQL Is Powerful
- Fast searching through huge datasets  
- Ensures data consistency  
- Allows safe multi-user access  
- SQL is standardized across systems  
- SQL skills transfer to any company or tech stack

Professional fact:  
> Almost every major company uses SQL every day — banks, hospitals, games, governments, apps.

---

# ⭐ SECTION 3 — How SQL Organizes Data

### Example Table: `students`

| id | name  | age | city     |
|----|--------|-----|----------|
| 1  | Alice | 17  | Shanghai |
| 2  | Bob   | 16  | Suzhou   |
| 3  | Carol | 18  | Beijing  |

**Important notes:**
- SQL does not store Python lists  
- SQL does not store objects  
- SQL stores **pure structured data** in rows and columns

# ⭐ SECTION 4 - SQLAlchemy & Flask Database 

## **Part 1: Why SQLAlchemy?**

### **The Problem with Raw SQL**
```python
# Raw SQL (what you learned before)
cursor.execute('''
    INSERT INTO users (name, email, age) 
    VALUES (?, ?, ?)
''', ('Alice', 'alice@email.com', 25))

# Problems:
# 1. Hard to read and maintain
# 2. No error checking in Python
# 3. Can't use Python objects directly
# 4. SQL injection risk if not careful
```

### **The Solution: SQLAlchemy ORM**
**ORM = Object-Relational Mapper**  
Think of it as a **translator** between Python and SQL:

```
Python Objects ↔ ORM ↔ SQL Database
    User           ↓        users table
  (Class)        Translates  (Table)
```

**Benefits:**
- Write Python code instead of SQL
- Automatic type checking
- Built-in security
- Works with Flask easily

---

## **Part 2: Quick Setup (5 Minutes)**

Download datagrip
https://www.jetbrains.com/zh-cn/datagrip/download/?section=windows

### **Installation**
```bash
pip install flask flask-sqlalchemy
```

### **Basic Flask + SQLAlchemy Setup**
```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)

# Configure database (SQLite for simplicity)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///students.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

# Create database object
db = SQLAlchemy(app)

print("✅ Flask + SQLAlchemy ready!")
```

---

## **Part 3: Your First SQLAlchemy Model**

### **What's a Model?**
A Python class that represents a database table.

```python
# Define a Student model (like a blueprint)
class Student(db.Model):
    # Table name (auto-generated: 'student')
    __tablename__ = 'students'
    
    # Columns = Class attributes
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50), nullable=False)
    age = db.Column(db.Integer)
    city = db.Column(db.String(50))
    
    # Magic method for printing
    def __repr__(self):
        return f'<Student {self.name}>'
```

**Each line explained:**
- `db.Model` → Makes this a database table
- `db.Column` → Creates a column
- `db.Integer`, `String` → Data types
- `primary_key=True` → Unique identifier
- `nullable=False` → Cannot be empty

---

## **Part 4: CRUD with SQLAlchemy (Much Easier!)**

### **Setup & Create Tables**
```python
# Create the database file and tables
with app.app_context():
    db.create_all()  # Creates all tables based on models
    print("✅ Database tables created!")
```

### **C - CREATE (Add Data)**
```python
# Method 1: Create object and add
student1 = Student(name="Alice", age=17, city="Shanghai")
db.session.add(student1)  # Stage for saving

# Method 2: Create and add in one line
student2 = Student(name="Bob", age=16, city="Suzhou")
db.session.add(student2)

# Save to database
db.session.commit()
print("✅ Students added!")
```

### **R - READ (Query Data)**
```python
# 1. Get ALL students
all_students = Student.query.all()
print("All students:", all_students)

# 2. Get by ID (primary key)
alice = Student.query.get(1)  # Gets student with id=1
print("Student with ID 1:", alice)

# 3. Filter with filter_by (simple conditions)
shanghai_students = Student.query.filter_by(city="Shanghai").all()
print("\nStudents in Shanghai:", shanghai_students)

# 4. Filter with filter (more complex)
older_students = Student.query.filter(Student.age > 16).all()
print("\nStudents older than 16:", older_students)

# 5. Get first result
first_student = Student.query.first()
print("\nFirst student:", first_student)

# 6. Count students
student_count = Student.query.count()
print(f"\nTotal students: {student_count}")
```

### **U - UPDATE (Modify Data)**
```python
# Find student
student = Student.query.get(1)

# Update fields (just change Python attributes!)
student.age = 18
student.city = "Shanghai Downtown"

# Save changes
db.session.commit()
print("✅ Student updated!")
```

### **D - DELETE (Remove Data)**
```python
# Find and delete
student = Student.query.get(2)  # Get student with id=2
if student:
    db.session.delete(student)
    db.session.commit()
    print("✅ Student deleted!")
```

---
