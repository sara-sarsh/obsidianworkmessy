
### ✅ **SUPPORTED FEATURES:**
1. **Diagram Types:**
   - Flowcharts (`graph TD`, `flowchart TD`)
   - Sequence diagrams (`sequenceDiagram`)
   - State diagrams (`stateDiagram-v2`)
   - Timeline diagrams (`timeline`)
   - Gantt charts (`gantt`)
   - Class diagrams (`classDiagram`)
   - Pie charts (`pie`)

2. **Styling:**
   - Node styling: `style NodeName fill:#color,stroke:#color,stroke-width:px`
   - Class definitions: `classDef className fill:#color,stroke:#color`
   - Applying classes: `class NodeName className`
   - Subgraphs with colors

3. **Layout:**
   - Top to bottom (`graph TD`, `flowchart TD`)
   - Left to right (`graph LR`, `flowchart LR`)
   - Subgraphs for grouping

4. **Special Elements:**
   - Notes (`note right of Node: text`)
   - Loops (`loop Every X`)
   - Alt/Else paths (`alt Condition` / `else`)
   - Parallel actions (`par`)

### ❌ **NOT SUPPORTED / CAUSES ERRORS:**
1. **JavaScript in diagrams** (causes parse errors):
   - ❌ `click Node "javascript:alert('text')"`
   - ❌ Any JavaScript event handlers
   - ❌ Interactive click events

2. **Complex formatting in labels:**
   - ❌ HTML tags in node text
   - ❌ Complex markdown in labels
   - ❌ Special characters that break parsing

3. **Certain advanced features:**
   - ❌ Custom icons via HTML
   - ❌ Dynamic content
   - ❌ External links in nodes

### 📏 **SIZE LIMITATIONS:**
1. **Horizontal scrolling** is an issue in Obsidian
2. **Keep diagrams compact** - prefer vertical layouts
3. **Maximum 8-10 participants** in sequence diagrams
4. **Use subgraphs** to group related elements

### 🎨 **PREFERRED FORMAT:**
```mermaid
---
title: Descriptive Title
---
graph TD
A[Start] --> B{Decision}
B -->|Yes| C[Action]
B -->|No| D[Alternative]

subgraph "Group Name"
C --> E[Result]
D --> F[Other Result]
end

style A fill:#4CAF50,color:white
classDef green fill:#4CAF50,color:white
class A green

### 📋 **WHEN REQUESTING DIAGRAMS, SPECIFY:**
1. **Diagram type** (flowchart, sequence, state, etc.)
2. **Purpose** (architecture, flow, states, timeline)
3. **Key components** to include
4. **Level of detail** needed
5. **Preferred orientation** (vertical/horizontal)

### 🚫 **AVOID:**
- JavaScript click handlers
- Overly complex node text
- Too many participants/nodes
- Horizontal layouts that cause scrolling

```
## **Quick Reference Card:**

```markdown
## Mermaid Quick Reference

**For Architecture:** Use `graph TD` or `flowchart TD`
**For Process Flow:** Use `sequenceDiagram` or `timeline`
**For State Changes:** Use `stateDiagram-v2`
**For Timing:** Use `gantt` or `timeline`
**For Relationships:** Use `classDiagram`

**Remember:** No JavaScript, keep it compact, use vertical layouts!
```