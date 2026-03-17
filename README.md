# Ex.No.6 Development of Python Code Compatible with Multiple AI Tools

# Date:
# Register no. 212223060091
# Aim:
Write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights with Multiple AI Tools

# AI Tools Required:
 .ChatGPT 
 · Google Gemini 
 · Microsoft Copilot 
 ·

# Explanation:
Experiment the persona pattern as a programmer for any specific applications related with your interesting area. 
Generate the outoput using more than one AI tool and based on the code generation analyse and discussing that. 

# Output:
Below is a **programmer-grade Python solution** that demonstrates how to integrate with **multiple AI tools**, automate **API interactions**, **compare outputs**, and **generate actionable insights** in a clean, extensible way.

This design follows **real software engineering practices** (modularity, abstraction, metrics, extensibility).

---

## 🔹 System Goals

1. Call **multiple AI APIs** with the same prompt
2. Normalize and store responses
3. Compare outputs using objective metrics
4. Generate **actionable insights** automatically

---

## 🔹 Architecture

```
Prompt
  ├── OpenAI
  ├── Anthropic
  ├── Local / Mock AI
  ↓
Response Normalization
  ↓
Comparison Engine
  ↓
Insight Generator
```

---

## 🔹 Python Code: Multi-AI Orchestration Engine

```python
import requests
from dataclasses import dataclass
from difflib import SequenceMatcher
from typing import Dict, List
import statistics

# ===============================
# CONFIGURATION
# ===============================

OPENAI_API_KEY = "YOUR_OPENAI_KEY"
ANTHROPIC_API_KEY = "YOUR_ANTHROPIC_KEY"

PROMPT = "Explain blockchain technology in simple terms."

# ===============================
# DATA MODELS
# ===============================

@dataclass
class AIResponse:
    model: str
    text: str
    word_count: int


# ===============================
# AI CONNECTORS
# ===============================

class OpenAIClient:
    def generate(self, prompt: str) -> str:
        url = "https://api.openai.com/v1/chat/completions"
        headers = {
            "Authorization": f"Bearer {OPENAI_API_KEY}",
            "Content-Type": "application/json"
        }
        payload = {
            "model": "gpt-4o-mini",
            "messages": [{"role": "user", "content": prompt}],
            "temperature": 0.5
        }
        response = requests.post(url, headers=headers, json=payload)
        return response.json()["choices"][0]["message"]["content"]


class AnthropicClient:
    def generate(self, prompt: str) -> str:
        url = "https://api.anthropic.com/v1/messages"
        headers = {
            "x-api-key": ANTHROPIC_API_KEY,
            "anthropic-version": "2023-06-01",
            "content-type": "application/json"
        }
        payload = {
            "model": "claude-3-haiku-20240307",
            "max_tokens": 300,
            "messages": [{"role": "user", "content": prompt}]
        }
        response = requests.post(url, headers=headers, json=payload)
        return response.json()["content"][0]["text"]


# ===============================
# RESPONSE PROCESSING
# ===============================

def normalize_response(model: str, text: str) -> AIResponse:
    return AIResponse(
        model=model,
        text=text.strip(),
        word_count=len(text.split())
    )


# ===============================
# COMPARISON ENGINE
# ===============================

def similarity(a: str, b: str) -> float:
    return SequenceMatcher(None, a, b).ratio()


def compare_responses(responses: List[AIResponse]) -> Dict:
    similarities = []
    for i in range(len(responses)):
        for j in range(i + 1, len(responses)):
            similarities.append(
                similarity(responses[i].text, responses[j].text)
            )

    return {
        "average_word_count": statistics.mean(r.word_count for r in responses),
        "max_detail_model": max(responses, key=lambda r: r.word_count).model,
        "avg_similarity": statistics.mean(similarities) if similarities else 1.0
    }


# ===============================
# INSIGHT GENERATOR
# ===============================

def generate_insights(metrics: Dict) -> List[str]:
    insights = []

    if metrics["avg_similarity"] > 0.7:
        insights.append("Models largely agree on the core explanation.")
    else:
        insights.append("Models differ significantly in interpretation or emphasis.")

    insights.append(
        f"Use {metrics['max_detail_model']} when detailed explanations are required."
    )

    if metrics["average_word_count"] < 120:
        insights.append("Responses are concise; suitable for summaries.")
    else:
        insights.append("Responses are detailed; suitable for learning content.")

    return insights


# ===============================
# MAIN ORCHESTRATOR
# ===============================

def run_multi_ai_pipeline(prompt: str):
    print("▶ Running Multi-AI Evaluation Pipeline\n")

    clients = [
        OpenAIClient(),
        AnthropicClient()
    ]

    responses = []

    for client in clients:
        model_name = client.__class__.__name__
        text = client.generate(prompt)
        responses.append(normalize_response(model_name, text))

    metrics = compare_responses(responses)
    insights = generate_insights(metrics)

    print("▶ RESPONSES\n")
    for r in responses:
        print(f"[{r.model}] ({r.word_count} words)\n{r.text}\n")

    print("▶ METRICS\n", metrics)

    print("\n▶ ACTIONABLE INSIGHTS")
    for insight in insights:
        print("•", insight)


# ===============================
# EXECUTION
# ===============================

if __name__ == "__main__":
    run_multi_ai_pipeline(PROMPT)
```

---

## 🔹 What This Code Does Well

### ✅ Multi-AI Integration

* Clean abstraction per AI provider
* Easy to add **Gemini**, **Azure OpenAI**, **local LLMs**

### ✅ Automated Comparison

* Word count (depth proxy)
* Text similarity (agreement proxy)
* Cross-model metrics

### ✅ Actionable Insights

* Select best model per task
* Detect disagreement
* Decide summary vs deep explanation use-cases

---

## 🔹 Where This Is Used in Real Systems

* AI benchmarking platforms
* Prompt-engineering experiments
* Enterprise AI routing (best-model selection)
* Research & academic evaluations
* Automated report generators

---





# Conclusion:

This experiment demonstrates that persona-based prompting combined with Python automation enables efficient interaction with multiple AI tools. The results are generated and compared with actionable insights.


# Result: 
The corresponding Prompt is executed successfully.
