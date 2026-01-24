# 🚀 LangGraph Intro Session

## 🎯 **Session Goals**
By the end of this hour, you'll:
- Understand what LangGraph is and why it matters
- Build your first AI workflow with cycles and decisions
- Know when to use LangGraph vs simple chains
- Have code you can immediately build upon

---

## 🎈 **Question**
1. You've ever needed an AI to "think step-by-step"?
2. You've been frustrated by linear/rigid AI flows?

**Today, we are going to solve this problems!**

---

## Part 1: ❓ **Why LangGraph? The Big Picture**

### **The Limitation: Linear Thinking**
```python
# Traditional LLM flow - one straight line
input → LLM → output
# What if you need to: research → analyze → decide → research more?
```

### **Real-World Example: Travel Assistant**
Imagine asking: *"Plan a trip to Japan"*

**Bad linear flow:**
```
You: "Plan trip to Japan"
AI: [One huge response with flights, hotels, itinerary...]
→ No fact-checking
→ No budgeting step
→ No personalization feedback
```

**Good graph flow:**
```
1. RESEARCH → Find flights, hotels, attractions
2. CHECK BUDGET → Is this under $2000?
   │
   ├─→ If NO: BACK TO RESEARCH (find cheaper options)
   │
   └─→ If YES: 3. PERSONALIZE → Add your preferences
                4. FORMAT → Create beautiful itinerary
```

### **Enter LangGraph: Flowcharts for AI**
> "LangGraph lets you build **state machines** where your AI can think, loop, and decide like a human."

---

## Part 2: 🧠 **Core Concepts**

### **Concept 1: The STATE (Shared Memory)**
```python
# Think of this as a shared whiteboard
state = {
    "question": "What is LangGraph?",
    "research": "",  # Will be filled
    "answer": ""     # Will be filled
}

# Every node can read AND write to this state
```

### **Concept 2: NODES (Workers/Functions)**
```python
def research_node(state):
    """Node 1: Does research"""
    state["research"] = "LangGraph is for building AI workflows!"
    return state  # Passes updated state forward

def answer_node(state):
    """Node 2: Creates answer"""
    state["answer"] = f"Based on research: {state['research']}"
    return state
```

### **Concept 3: EDGES (Pathways)**
```
research_node → answer_node → END
```

### **Concept 4: CYCLES (The Superpower!)**
```
           ┌─────┐
           │     ▼
research → check_quality
           │     │
           └─────┘ (if quality bad, loop back!)
```

### **Visualize It!** ✨
```
        [START]
           │
           ▼
    ┌─────────────┐
    │  RESEARCH   │
    └─────────────┘
           │
           ▼
    ┌─────────────┐
    │CHECK QUALITY│
    └─────────────┘
           │
    ┌──────┴──────┐
    ▼             ▼
[GOOD]        [BAD]
    │             │
    ▼             │
┌─────────┐       │
│ ANSWER  │◄──────┘
└─────────┘
    │
    ▼
   END
```

---

## Part 3: 👨‍💻 **Let's Build Our First Graph!**

### **🛠️ Environment Setup (2 mins)**
```bash
# Run this NOW in your terminal!
pip install langgraph
```

*While installing, let me show you what we're building...*

### **📁 Our Project: Research Assistant**
**Goal**: Ask a question → Research → Answer
**Special feature**: Quality check that can loop!

### **Step 1: The Complete Code (We'll Build Together)**
```python
# research_assistant.py
from langgraph.graph import StateGraph, END
from typing import TypedDict, Literal
import time  # For simulating work

# ========================
# 1. DEFINE OUR STATE
# ========================
class GraphState(TypedDict):
    """Shared memory between all nodes"""
    question: str      # User's question
    research: str      # Research results
    answer: str        # Final answer
    attempts: int      # How many research attempts?
    quality: str       # 'good' or 'bad'

# ========================
# 2. CREATE OUR NODES
# ========================
def research_node(state: GraphState) -> GraphState:
    """Simulates researching a question"""
    print(f"🔍 Researching: '{state['question']}'...")
    time.sleep(0.5)  # Simulate API call
    
    # Mock research results
    research_data = {
        "What is LangGraph?": "LangGraph is a library for building stateful, multi-actor applications with LLMs. It's like flowcharts for AI!",
        "What is Python?": "Python is a popular programming language known for its simplicity.",
        "Tell me about AI": "Artificial Intelligence is transforming how we build software."
    }
    
    state["research"] = research_data.get(
        state["question"], 
        "No specific research found."
    )
    state["attempts"] = state.get("attempts", 0) + 1
    print(f"   Found: {state['research'][:50]}...")
    
    return state

def quality_check_node(state: GraphState) -> GraphState:
    """Checks if research is good enough"""
    print("✅ Checking research quality...")
    
    research = state["research"]
    
    # Simple quality check: is it long enough?
    if len(research) > 30 and "No specific" not in research:
        state["quality"] = "good"
        print("   Quality: GOOD ✓")
    else:
        state["quality"] = "bad"
        print(f"   Quality: BAD ✗ (too short: {len(research)} chars)")
    
    return state

def answer_node(state: GraphState) -> GraphState:
    """Creates final answer from research"""
    print("📝 Writing answer...")
    
    state["answer"] = f"""
    🤖 **ANSWER TO: '{state['question']}'**
    
    **Research Findings:** {state['research']}
    
    **Process:** Took {state['attempts']} attempt(s)
    
    ---
    Generated by your LangGraph assistant!
    """
    
    print(f"   Answer ready! ({len(state['answer'])} chars)")
    return state

# ========================
# 3. ROUTING LOGIC
# ========================
def route_quality(state: GraphState) -> Literal["research", "answer"]:
    """Decides where to go next based on quality"""
    MAX_ATTEMPTS = 3
    
    if state["quality"] == "good":
        print("   Routing: research → answer")
        return "answer"
    elif state.get("attempts", 0) >= MAX_ATTEMPTS:
        print(f"   Maximum attempts ({MAX_ATTEMPTS}) reached, routing to answer")
        return "answer"
    else:
        print(f"   Routing: research → research (attempt {state['attempts']})")
        return "research"

# ========================
# 4. BUILD THE GRAPH!
# ========================
print("🏗️ Building our LangGraph...")

# Create graph builder
graph_builder = StateGraph(GraphState)

# Add nodes
graph_builder.add_node("research", research_node)
graph_builder.add_node("quality_check", quality_check_node)
graph_builder.add_node("answer", answer_node)

# Set starting point
graph_builder.set_entry_point("research")

# Add edges
graph_builder.add_edge("research", "quality_check")
graph_builder.add_conditional_edges(
    "quality_check",
    route_quality,  # Decision function
    {
        "research": "research",  # If returns "research", go to research node
        "answer": "answer"       # If returns "answer", go to answer node
    }
)
graph_builder.add_edge("answer", END)

# Compile the graph
graph = graph_builder.compile()

print("✅ Graph built successfully!")
print("\n" + "="*50 + "\n")

# ========================
# 5. RUN IT!
# ========================
if __name__ == "__main__":
    # Test with different questions
    test_questions = [
        "What is LangGraph?",
        "Hi",  # Will trigger loop (too short)
        "What is Python?"
    ]
    
    for question in test_questions:
        print(f"\n🚀 Running for: '{question}'")
        print("-" * 30)
        
        # Initialize state
        initial_state = {
            "question": question,
            "research": "",
            "answer": "",
            "attempts": 0,
            "quality": ""
        }
        
        # Run the graph!
        result = graph.invoke(initial_state)
        
        print(f"\n📋 FINAL ANSWER:")
        print(result["answer"])
        print("=" * 50)

```

### **Step 2: Run It Together!** 🎮
```bash
python research_assistant.py
```

**Expected Output:**
```
🏗️ Building our LangGraph...
✅ Graph built successfully!

==================================================

🚀 Running for: 'What is LangGraph?'
------------------------------
🔍 Researching: 'What is LangGraph?'...
   Found: LangGraph is a library for building stateful...
✅ Checking research quality...
   Quality: GOOD ✓
   Routing: research → answer
📝 Writing answer...
   Answer ready! (214 chars)

📋 FINAL ANSWER:
    🤖 **ANSWER TO: 'What is LangGraph?'** ...
```

---

## Part 4: 🚀 **Real-World Patterns You Can Use**

### **Pattern 1: Agent Supervisor**
#### 📌 What It Is
A supervisor agent that delegates tasks to specialist agents, then synthesizes their work. Think of it like a project manager coordinating a team.

##### 🎯 Real-World Use Cases
- Customer Support: Route ticket → Tech agent + Billing agent → Combine solutions
- Content Creation: Topic → Researcher + Writer + Editor → Final article
- Code Review: PR → Security checker + Style checker + Performance analyzer → Report


```
                [SUPERVISOR]
                /    |    \
               /     |     \
          [Coder] [Writer] [Researcher]
               \     |     /
                \    |    /
              [Synthesizer] → Answer
```

#### ✨ Key Benefits
- Specialization: Each agent excels at one thing
- Scalability: Easy to add more specialists
- Maintainability: Fix one agent without breaking others
- Quality: Multiple perspectives synthesized

### **Pattern 2: Human-in-the-Loop**
#### 📌 What It Is
Pauses execution to get human input/approval before continuing. Essential for high-stakes or regulated applications.

#### 🎯 Real-World Use Cases
- Medical diagnosis: AI suggestion → Doctor review → Treatment plan
- Legal documents: Draft contract → Lawyer review → Finalize
- Content moderation: Flagged content → Human moderator → Action
- Financial trades: AI recommendation → Trader approval → Execute

```python
def human_review_node(state):
    """Pauses for human input"""
    print(f"👤 HUMAN REVIEW NEEDED!")
    print(f"Research: {state['research'][:200]}...")
    
    # In real app, this would wait for API/UI input
    approval = input("Approve? (y/n): ")
    
    if approval.lower() == 'y':
        state["approved"] = True
        return "continue"
    else:
        state["needs_correction"] = input("What's wrong?: ")
        return "research_again"
```

#### ✨ Key Benefits
- Safety: Human oversight for critical decisions
- Quality control: Catch AI hallucinations/errors
- Regulatory compliance: Required for healthcare/finance
- Customization: Humans provide domain-specific guidance

### **Pattern 3: Parallel Execution**
#### 📌 What It Is
Run multiple operations simultaneously, then combine results. Like having multiple workers tackling different parts of a problem at the same time.

#### 🎯 Real-World Use Cases
- Market research: Simultaneously check Google + Twitter + News sites
- Product comparison: Query Amazon + eBay + Walmart APIs together
- Due diligence: Check legal + financial + reputation databases in parallel
- Content aggregation: Fetch from multiple sources simultaneously


```python
# Run multiple tools at once
graph_builder.add_node("search_web", search_web)
graph_builder.add_node("search_docs", search_docs)

# Both start from same point
graph_builder.add_edge("start", "search_web")
graph_builder.add_edge("start", "search_docs")

# Wait for both to finish
graph_builder.add_node("synthesize", synthesize)
graph_builder.add_edge("search_web", "synthesize")
graph_builder.add_edge("search_docs", "synthesize")
```

#### ✨ Key Benefits
- Speed: Dramatically faster than sequential execution
- Efficiency: No waiting for independent operations
- Robustness: One source failing doesn't block others
- Comprehensiveness: Multiple perspectives simultaneously

---

## 📊 **When to Use LangGraph vs Simple Chains**

| **Use LangGraph When...** | **Use Simple Chains When...** |
|---------------------------|-------------------------------|
| Need loops/retry logic | Linear flow is sufficient |
| Multiple decision points | One-pass generation |
| Human approval needed | Fully autonomous |
| Complex agent workflows | Simple Q&A or summarization |

---

## 🎓 **Key Takeaways**
1. **LangGraph = Flowcharts for AI** with shared state
2. **Nodes** do work, **edges** define pathways
3. **Cycles** enable retry/improvement loops
4. **Start simple**: Research → Check → Answer pattern
5. **Graduate to**: Multi-agent, human-in-loop, parallel

---

## 🔗 **Resources to Continue**
- **Official Docs**: [langchain.com/langgraph](https://langchain.com/langgraph)
- **Tutorials**: "LangGraph in 17 minutes" (YouTube)
- **Templates**: GitHub `langchain-ai/langgraph`

---