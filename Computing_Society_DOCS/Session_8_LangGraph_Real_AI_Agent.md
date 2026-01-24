# 🚀 **Session 2: Building a Real AI Assistant with Tools**

## 🎯 **Session Goals**
By the end of this session, you'll:
- Connect LangGraph to **real APIs and tools**
- Build an AI assistant that can **search, calculate, and research**
- Understand **tool selection and routing logic**
- Have a **production-ready template** for your own projects

---

## Part 1: 🔄 **From "Fake It" to "Make It"**

### **Recap: Our Session 1 Assistant**
```python
# Remember this? It was all mock data!
research_data = {
    "What is LangGraph?": "LangGraph is a library for...",
    "What is Python?": "Python is a popular language...",
}

# Problem: Static, limited, not real
```

### **The Evolution of AI Assistants**

| **Phase** | **What It Does** | **Limitation** |
|-----------|------------------|----------------|
| **Phase 1** | Answers from pre-written responses | Can't handle new questions |
| **Phase 2** | Uses LLM's internal knowledge | May be outdated/wrong |
| **Phase 3** | **Connects to real tools** | **Gets live, accurate info** |

### **Why Tools Matter**
Think of tools as **superpowers** for your AI:
- **Search**: Access to all current information
- **Calculator**: Perfect math every time  
- **APIs**: Connect to weather, news, databases
- **Your code**: Custom functionality

**Analogy**: LLMs are **smart but know-it-all friends**, tools give them **access to the real world**.

---

## Part 2: 🛠️ **Hands-on: Adding Your First Real Tool**

### **The Simplest Tool: A Calculator**

#### **Before (Mock):**
```python
def mock_calculator(question):
    # Fake responses
    if "2+2" in question:
        return "4"
    # What about 3987 × 234.5?
```

#### **After (Real):**
```python
def real_calculator(question):
    # Use Python's actual math!
    import re
    # Extract math expression safely
    # Calculate accurately
    return actual_result  # Perfect math every time
```

### **Let's Build It Step-by-Step**

#### **Step 1: Understanding the Architecture**
```
User Question → [Router] → [Tool Nodes] → [Answer Formatter] → Response
                    ↓
           Picks the right tool
```

#### **Step 2: Creating Our Calculator Node**

**Key Concept**: A **node** is just a function that:
1. Takes the **state** as input
2. Does some work (with tools!)
3. Updates the **state**
4. Returns the updated state

```python
# calculator_node.py - Detailed Explanation

def calculator_node(state):
    """
    A REAL calculator that handles actual math expressions.
    
    Input: state dict with "question" key
    Output: Updated state with "answer" and "tool_used"
    """
    
    print("🧮 Calculator Node: Processing math question...")
    
    # 1. Extract the question from state
    question = state.get("question", "")
    
    # 2. Extract math expression from question
    # Remove common phrases like "calculate", "what is", "compute"
    import re
    
    # Clean the question - remove common prefixes
    clean_question = question.lower()
    for prefix in ["calculate", "compute", "what is", "what's", "solve", "evaluate"]:
        clean_question = clean_question.replace(prefix, "")
    
    # Extract the math expression
    # Try to find the most complete math expression
    # Look for patterns like: (10+5)*2, 3*15+2, etc.
    
    # Pattern 1: Expressions with parentheses and operators after
    math_pattern1 = r'\([^)]+\)\s*[\+\-\*\/]\s*[\d\.]+'
    
    # Pattern 2: Expressions with parentheses only
    math_pattern2 = r'\([^)]+\)'
    
    # Pattern 3: Standard math expressions without parentheses
    math_pattern3 = r'[-+]?[0-9]*\.?[0-9]+(?:[ \t]*[-+*/%][ \t]*[-+]?[0-9]*\.?[0-9]+)+'
    
    # Try patterns in order of complexity
    match = re.search(math_pattern1, clean_question)
    if not match:
        match = re.search(math_pattern2, clean_question)
    if not match:
        match = re.search(math_pattern3, clean_question)
    
    # If still no match, try a simple catch-all
    if not match:
        # Look for any sequence that looks like a math expression
        math_pattern_simple = r'[\d\(\)\+\-\*\/\.]+'
        match = re.search(math_pattern_simple, clean_question)
    
    if match:
        expression = match.group(0).strip()
        
        # 3. SAFETY FIRST: Evaluate safely
        # WARNING: Never use eval() on untrusted input!
        # We're using a safe subset for demo purposes
        # In production, use a proper math parser like `numexpr`
        
        try:
            # Clean the expression (remove extra spaces)
            clean_expr = expression.replace(' ', '')
            
            # Simple safe evaluation for basic operations
            # This is a minimal safe calculator - expand for production!
            result = safe_calculate(clean_expr)
            
            # 4. Update the state
            state["answer"] = f"Calculator result: {expression} = {result}"
            state["tool_used"] = "calculator"
            state["raw_result"] = result
            
            print(f"   Calculated: {expression} = {result}")
            
        except Exception as e:
            # Handle errors gracefully
            state["answer"] = f"Sorry, I couldn't calculate '{expression}'. Error: {e}"
            state["tool_used"] = "calculator_error"
            
    else:
        # No math expression found
        state["answer"] = "I couldn't find a math expression to calculate."
        state["tool_used"] = "no_math_found"
    
    # 5. Return updated state (crucial step!)
    return state

def safe_calculate(expression):
    """
    Safely evaluate basic math expressions.
    Only allows: numbers, +, -, *, /, (, )
    """
    # Remove any non-approved characters
    import re
    safe_expr = re.sub(r'[^0-9\+\-\*\/\(\)\.]', '', expression)
    
    # Basic safety: check it's not empty
    if not safe_expr:
        raise ValueError("No valid expression")
    
    # Use Python's eval but only for our sanitized expression
    # In production, use a proper math library!
    return eval(safe_expr)
```

**What makes this a GOOD node?**
- ✅ **Safety first**: Sanitizes input before calculation
- ✅ **Clear purpose**: Does one thing well (calculates)
- ✅ **Error handling**: Gracefully handles problems
- ✅ **State management**: Updates state properly
- ✅ **Logging**: Prints what it's doing (for debugging)

---

## Part 3: 🔀 **Smart Routing: Choosing the Right Tool**

### **The Problem: How Does AI Know Which Tool to Use?**

**Bad Approach**: Always use all tools (wasteful, confusing)
**Good Approach**: **Router node** that analyzes the question first

### **Building Our Router Node**

```python
# router_node.py - The Brain of Our Assistant

def router_node(state):
    """
    Analyzes the question and decides which tool to use.
    
    Think of this as a receptionist at a hospital:
    - Math problems? Send to Calculator Doctor
    - General questions? Send to Search Doctor  
    - Fact questions? Send to Wikipedia Doctor
    """
    
    print("🤔 Router: Analyzing question to choose the right tool...")
    
    question = state.get("question", "").lower()
    
    # 1. Check for math keywords
    math_keywords = ["calculate", "math", "+", "-", "*", "/", "times", "plus", "minus", "divide"]
    if any(keyword in question for keyword in math_keywords):
        # Also check for actual math patterns
        import re
        if re.search(r'\d\s*[\+\-\*/]\s*\d', question):
            state["next_node"] = "calculator"
            print("   Routing to: Calculator 🧮")
            return state
    
    # 2. Check for fact-based questions
    fact_keywords = ["what is", "who is", "define", "wiki", "encyclopedia"]
    if any(keyword in question for keyword in fact_keywords):
        state["next_node"] = "wikipedia"
        print("   Routing to: Wikipedia 📚")
        return state
    
    # 3. Default to web search
    state["next_node"] = "search"
    print("   Routing to: Web Search 🌐")
    return state
```

### **How Routing Works in LangGraph**

```python
# The magic: conditional edges
# Connect router to tools with conditional edges
builder.add_conditional_edges(
    "router",
    route_based_on_decision,
    {
        "calculator": "calculator",
        "wikipedia": "wikipedia",
        "search": "search"
    }
)
```

**Visualizing the Flow**:
```
          [User Question]
                ↓
           [Router Node]
         /       |       \
        ↓        ↓        ↓
 [Calculator] [Wikipedia] [Search]
        ↓        ↓        ↓
    ┌───┴────────┴────────┘
    ↓
[Answer Formatter]
    ↓
   [END]
```

---

## Part 4: 🧩 **Building a Complete Multi-Tool Assistant**

Now that we have our core components - a calculator tool and a smart router - let's expand our assistant into a complete multi-tool system. This section will guide you through adding more tools, integrating everything into a cohesive workflow, and running your assistant in production-ready scenarios.

### **Step 1: Adding More Tools - Expanding Your Assistant's Capabilities**

**The Power of Specialization**: Just like a well-equipped workshop has different tools for different jobs, your AI assistant needs specialized tools for different types of questions. Each tool should excel at one specific task.

**Wikipedia Tool - The Fact Checker**:
- **Purpose**: Provides accurate, well-sourced factual information from Wikipedia's vast knowledge base
- **How It Works**: Uses the `WikipediaAPIWrapper` from `langchain-community` to search and retrieve summaries
- **Key Implementation Details**:
  - Extracts the main search term by removing question words ("what is", "who is", etc.)
  - Handles import errors gracefully with helpful installation instructions
  - Limits response length to maintain readability while providing comprehensive information
  - Includes source attribution for transparency

**Web Search Tool - The Current Events Expert**:
- **Purpose**: Fetches real-time information from the web using DuckDuckGo search
- **How It Works**: Leverages `DuckDuckGoSearchResults` tool with no API key required
- **Key Implementation Details**:
  - Configures result count (typically 3-5 results for balance of completeness and conciseness)
  - Formats results clearly with source attribution
  - Includes fallback mechanisms when search services are unavailable
  - Handles network errors and rate limits gracefully

**Tool Design Principles**:
1. **Single Responsibility**: Each tool does one thing exceptionally well
2. **Error Resilience**: Tools should fail gracefully with helpful error messages
3. **User Experience**: Clear output formatting and source attribution
4. **Maintainability**: Easy to update or replace individual tools

### **Step 2: Putting It All Together - The Complete Graph Architecture**

**State Management - The Assistant's Memory**:
The state dictionary is the central nervous system of your assistant, passing information between nodes. A well-designed state includes:
- `question`: The user's original query
- `answer`: The formatted response from the selected tool
- `tool_used`: Which tool processed the question (for debugging and analytics)
- `next_node`: The router's decision on where to send the question
- `source`: Attribution for where information came from

**Graph Construction - Connecting the Dots**:
Building the complete workflow involves:
1. **Defining the State Schema**: Creating a TypedDict that specifies what data flows through your graph
2. **Initializing the Graph**: Setting up the StateGraph with your state schema
3. **Adding Nodes**: Registering each tool function (calculator, Wikipedia, search) and the router
4. **Setting Entry Point**: Defining where the graph starts (typically the router)
5. **Creating Conditional Edges**: The router analyzes the question and dynamically chooses which tool to use
6. **Adding Tool Nodes**: Each tool processes the question and returns an answer
7. **Setting Exit Points**: Defining where the graph ends after tools complete their work

**The Complete Workflow**:
```
[START]
   ↓
[Router Node] → Analyzes question type
   ↓
[Conditional Edge] → Routes to appropriate tool
   ↓
[Tool Node] → Calculator, Wikipedia, or Search
   ↓
[END] → Returns formatted answer
```

**Error Handling Strategy**:
- **Import Errors**: Provide clear installation instructions for missing packages
- **Network Errors**: Graceful fallbacks with user-friendly error messages
- **Tool Failures**: Alternative tool suggestions when one tool fails
- **Invalid Inputs**: Clear guidance on how to rephrase questions

### **Step 3: Running Our Assistant - From Development to Deployment**

**Testing Your Assistant**:
1. **Unit Testing**: Test each tool independently with various inputs
2. **Integration Testing**: Test the complete workflow end-to-end
3. **Edge Case Testing**: Test with unusual or problematic inputs
4. **Performance Testing**: Ensure reasonable response times

**Interactive Usage Patterns**:
- **Command Line Interface**: Simple Python script that takes user input
- **Batch Processing**: Process multiple questions from a file or list
- **API Integration**: Wrap the assistant in a web service
- **Interactive Chat**: Continuous conversation with context preservation

**Production Considerations**:
1. **Dependency Management**: Use `requirements.txt` with version pinning for reproducibility
2. **Environment Isolation**: Use virtual environments or containers
3. **Error Logging**: Implement comprehensive logging for debugging
4. **Monitoring**: Track tool usage patterns and success rates
5. **Security**: Input sanitization and rate limiting

**Scaling Your Assistant**:
- **Adding New Tools**: Follow the same pattern - create node, update router, test thoroughly
- **Performance Optimization**: Cache frequent queries, parallelize independent operations
- **User Experience**: Add conversation history, personalization, and feedback mechanisms

**Next Steps for Your Assistant**:
1. **Add More Specialized Tools**: Weather, translation, code execution, etc.
2. **Improve Router Intelligence**: Use embeddings or fine-tuned models for better tool selection
3. **Add Memory**: Remember conversation context and user preferences
4. **Create UI/UX**: Build web or mobile interfaces for your assistant
5. **Deploy at Scale**: Containerize and deploy to cloud platforms

---

## 📊 **Tool Performance Comparison**

| **Tool** | **Best For** | **Limitations** | **Cost** |
|----------|--------------|-----------------|----------|
| **Calculator** | Math problems | Basic operations only | Free |
| **Wikipedia** | Facts, definitions | May not have latest info | Free |
| **Web Search** | Current events, specific queries | Quality varies, rate limits | Free/Paid |
| **Custom APIs** | Your specific needs | Requires API key/development | Varies |

---

## 🎓 **Key Takeaways**

1. **Tools transform LLMs** from "know-it-alls" to "can-do-its"
2. **Router nodes are crucial** for picking the right tool
3. **Safety first**: Always sanitize inputs, handle errors
4. **Start simple**: Calculator → Wikipedia → Search
5. **Expand gradually**: Add tools one at a time

**Remember the Flow**:
```
Question → Router → [Tool] → Answer
     ↓                     ↑
  Analyze              Format & Return
```

---