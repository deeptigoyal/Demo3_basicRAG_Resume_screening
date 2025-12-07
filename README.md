# Demo3_basicRAG_Resume_screening

**Intially About Agents:**

_Reflection, Tool, Planning, ReAct, and ReWOO_ are common agent patterns that guide how an AI reasons and takes actions. They are not full cognitive architectures, but each loosely aligns with parts of a modular system like perception, cognition, action, and learning.

- ReAct combines reasoning (“think”) and acting (“do”) step by step. The agent observes the input (perception), reasons about what to do next (cognition), and performs tool actions (action).
- Tool pattern focuses mainly on using external tools. The agent identifies the right tool and executes it. This is strongest on the “action” side.
- Planning pattern separates planning from execution. First the agent generates a detailed plan (cognition), then uses tools to execute it.
- Reflection pattern adds a feedback loop. After producing an answer or taking an action, the agent re-evaluates its own output, learns from mistakes, and improves—similar to a lightweight “learning” module.
- ReWOO (Reasoning WithOut Observation) separates reasoning from tool-use. The agent produces a reasoning trace without interleaving tool calls, ensuring consistency. Tools are used only after reasoning is complete. This emphasizes structured cognition before action.

All these patterns are modular, but they do not formally implement full perception–cognition–action–learning architectures; they simply borrow the structure to make reasoning cleaner and more reliable.
