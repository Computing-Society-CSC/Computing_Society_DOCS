# Computing Society Documentation

Welcome to the official documentation repository for the Computing Society! This repository contains comprehensive session materials, tutorials, and resources covering a wide range of computing topics from web development fundamentals to advanced AI workflows.

## 📚 About This Repository

This repository serves as the central knowledge base for Computing Society sessions. Each session is documented in Markdown format with clear explanations, code examples, and practical exercises. Whether you're a beginner starting your programming journey or an experienced developer looking to expand your skills, you'll find valuable resources here.

## 🗓️ Session Catalog

### **Foundation Track**
| Session | Topic | Description | Prerequisites |
|---------|-------|-------------|---------------|
| [Session 1: Web Development](./Session_1_Web_Development.md) | HTML & CSS Foundations | Learn the basics of frontend web development with HTML and CSS. Build your first personal portfolio webpage. | None |
| [Session 2: Flask](./Session_2_Flask.md) | Introduction to Flask | Transition from static to dynamic web applications with Python's Flask framework. Learn back-end development fundamentals. | Basic Python |
| [Session 3: Flask Part 2](./Session_3_Flask_Part_2.md) | Advanced Flask Concepts | Dive deeper into Flask with templates, forms, and database integration. | Session 2 |
| [Session 3.5: Flask Practice + Python Basics](./Session%203.5%20-%20Flask%20Practice+Python%20Basics.md) | Flask Practice Session | Hands-on exercises to reinforce Flask concepts and Python fundamentals. | Session 2 |
| [Session 4: Review Session](./session4_review_session.md) | Comprehensive Review | Review and consolidate knowledge from previous sessions with practical challenges. | Sessions 1-3 |

### **Intermediate Track**
| Session | Topic | Description | Prerequisites |
|---------|-------|-------------|---------------|
| [Session 5: APIs](./Session_5_api.md) | Working with APIs | Learn how to consume and create RESTful APIs. Understand HTTP methods, authentication, and data formats. | Session 2 |
| [Session 6: Databases](./session_6_database.md) | Database Fundamentals | Introduction to databases, SQL, and integrating databases with web applications. | Session 3 |

### **Advanced Track**
| Session | Topic | Description | Prerequisites |
|---------|-------|-------------|---------------|
| [Session 7: LangGraph Intro](./Session_7_LangGraph_Intro.md) | Introduction to LangGraph | Build AI workflows with cycles and decisions. Learn state machines for AI applications. | Intermediate Python |
| [Session 8: LangGraph Real AI Agent](./Session_8_LangGraph_Real_AI_Agent.md) | Building Real AI Agents | Create sophisticated AI agents with multi-agent systems and human-in-the-loop patterns. | Session 7 |

## 🚀 Getting Started

### Prerequisites
- **Basic Programming Knowledge**: Familiarity with any programming language (Python recommended)
- **Development Environment**:
  - VS Code or any text editor
  - Python 3.11+ (for Flask and LangGraph sessions)
  - Modern web browser
- **Git**: Basic understanding of Git for cloning and version control

### Installation & Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/Computing-Society-CSC/Computing_Society_DOCS.git
   cd Computing_Society_DOCS
   ```

2. **Set up Python environment** (for Flask/LangGraph sessions):
   ```bash
   # Create a virtual environment
   python -m venv venv
   
   # Activate it (macOS/Linux)
   source venv/bin/activate
   
   # Activate it (Windows)
   venv\Scripts\activate
   
   # Install required packages
   pip install flask langgraph
   ```

3. **Explore sessions**:
   - Start with Session 1 if you're new to web development
   - Jump to specific sessions based on your interests
   - Follow the learning path for structured progression

## 📖 Learning Path

### **For Complete Beginners**
1. Start with **Session 1: Web Development** to learn HTML/CSS
2. Learn basic Python (external resources recommended)
3. Continue with **Session 2: Flask** for back-end development
4. Practice with **Session 3.5: Flask Practice**
5. Review with **Session 4: Review Session**

### **For Intermediate Developers**
1. Refresh with **Session 3: Flask Part 2**
2. Learn **Session 5: APIs** for web services
3. Study **Session 6: Databases** for data persistence
4. Explore **Session 7: LangGraph Intro** for AI workflows

### **For Advanced Developers**
1. Master **Session 8: LangGraph Real AI Agent**
2. Combine knowledge from all sessions to build full-stack applications

## 🛠️ How to Use These Materials

### **For Self-Study**
1. Read through the session documentation
2. Follow along with code examples
3. Complete the challenges and exercises
4. Experiment by modifying the code
5. Build your own projects using the concepts learned

### **For Workshop Leaders**
1. Use session materials as teaching guides
2. Adapt examples to your audience's skill level
3. Encourage hands-on coding during sessions
4. Use discussion prompts to engage participants
5. Assign follow-up projects for practice

### **For Project Development**
1. Reference sessions for specific techniques
2. Use code snippets as starting points
3. Combine concepts from multiple sessions
4. Contribute improvements back to the repository

## 💻 Code Structure

Each session includes:
- **Theory explanations** with analogies and diagrams
- **Step-by-step tutorials** with complete code examples
- **Challenges** with varying difficulty levels
- **Discussion prompts** for group learning
- **Reference tables** for quick lookup

Example session structure:
```
Session_X_Topic.md
├── Learning Goals
├── Theory & Concepts
├── Hands-on Tutorial
├── Challenges & Exercises
├── Discussion Prompts
└── Additional Resources
```

## 🤝 Contributing

We welcome contributions to improve these materials! Here's how you can help:

### **Types of Contributions**
1. **Content Improvements**: Fix typos, clarify explanations, update examples
2. **New Sessions**: Add documentation for additional topics
3. **Code Examples**: Provide better or alternative implementations
4. **Translations**: Translate sessions to other languages
5. **Exercises**: Create additional challenges and projects

### **Contribution Process**
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Make your changes
4. Test that the documentation renders correctly
5. Commit your changes (`git commit -m 'Add some improvement'`)
6. Push to the branch (`git push origin feature/improvement`)
7. Open a Pull Request

### **Guidelines**
- Follow the existing Markdown formatting style
- Include code examples with proper syntax highlighting
- Add comments to explain complex concepts
- Keep explanations beginner-friendly when appropriate
- Update the README session catalog if adding new sessions

## 📝 Session Format Standards

To maintain consistency across sessions, please follow these formatting guidelines:

### **Headers**
- Use `#` for main session title
- Use `##` for major sections
- Use `###` for subsections

### **Code Blocks**
```markdown
```python
# Python code with proper indentation
def example():
    return "Hello World"
```
```

### **Tables**
```markdown
| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Data 1   | Data 2   | Data 3   |
```

## 📊 Progress Tracking

Track your learning journey by checking off sessions as you complete them:

- [ ] Session 1: Web Development
- [ ] Session 2: Flask  
- [ ] Session 3: Flask Part 2
- [ ] Session 3.5: Flask Practice
- [ ] Session 4: Review Session
- [ ] Session 5: APIs
- [ ] Session 6: Databases
- [ ] Session 7: LangGraph Intro
- [ ] Session 8: LangGraph Real AI Agent

## 🎯 Learning Outcomes

By completing all sessions, you will be able to:
- Build full-stack web applications with Flask
- Create responsive web pages with HTML/CSS
- Work with databases and APIs
- Design and implement AI workflows with LangGraph
- Develop problem-solving skills through coding challenges
- Collaborate effectively on software projects

## 📄 License

This documentation is shared under an open license to promote learning and collaboration. Please attribute the Computing Society when sharing or adapting these materials.

---

*Last Updated: January 2025*  
*Maintained by the Computing Society*  
*For questions or suggestions, please open an issue in this repository.*