# Understanding `with_structured_output()` vs `PydanticOutputParser`

## Overview
There are two main ways to get structured (JSON-like) output from an LLM in LangChain:

- **`llm.with_structured_output()`** → Forces the LLM to output *only* a JSON object  
- **`PydanticOutputParser`** → Lets the LLM output *normal text + JSON*, and the parser extracts the JSON

---

## Core Difference

| Feature | `with_structured_output()` | `PydanticOutputParser` |
|--------|------------------------------|-------------------------|
| Output must be JSON only | ✅ Yes | ❌ No |
| Allows natural language + JSON | ❌ No | ✅ Yes |
| Strict schema enforcement | ✅ Strong | ⚠️ Medium |
| Best for backend pipelines | ✅ Yes | ⚠️ Depends |
| Best for RAG/chatbots | ❌ No | ✅ Yes |

---

## 1. Using `llm.with_structured_output()`

### When to Use
- You want **only JSON output**
- No reasoning or explanation  
- LLM acts as a strict API  
- Good for extraction, classification, scoring  

### Example
```python
from pydantic import BaseModel

class Summary(BaseModel):
    summary: str
    keywords: list[str]

structured_llm = llm.with_structured_output(Summary)
response = structured_llm.invoke("Explain transformers.")
print(response)
