# Usage Guide

This guide explains how to configure, run, and simulate the AI Customer Service Agent.

## ⚙️ Configuration

The agent loads its settings from environment variables. A template is provided in [.env.example](file:///c:/Users/hp/OneDrive/Desktop/Code/AI-Customer-Support-Agent/.env.example).

### 1. Set up Environment Variables
Copy `.env.example` to `.env` in the root workspace folder:
```powershell
cp .env.example .env
```

### 2. Supported Configuration Keys
Configure the following keys inside your `.env` file:
*   `OPENAI_API_KEY`: Your OpenAI organization/user key.
*   `DEFAULT_MODEL`: The default chat completion model (defaults to `gpt-4o`).
*   `ENABLE_EVALUATION`: Turn on/off real-time answer assessment (`true`/`false`).
*   `ENABLE_SENTIMENT_ANALYSIS`: Turn on/off customer emotional rating (`true`/`false`).
*   `MAX_CONVERSATION_HISTORY`: Limits the maximum count of historical chat turns loaded in memory (defaults to `50`).
*   `LOG_FILE`: Relative path where conversation transcripts are stored (defaults to `customer_agent.log`).

---

## 🐍 Python Interface Usage

You can import and run the agent directly within your python scripts:

```python
from customer_service_agent import CustomerServiceAgent, AgentConfig

# 1. Initialize config and agent
config = AgentConfig.from_env()
agent = CustomerServiceAgent(config=config)

# 2. Trigger a conversation
response = agent.chat(
    "Hi, I'd like to check the status of my order ORD-12345",
    customer_id="CUST-001"
)
print(f"Agent Response: {response}")

# 3. Retrieve evaluation summary report
print(agent.get_performance_report())
```

---

## 🎮 Running the Simulation Demos

The repository includes a helper utility script to run predefined customer support scenarios:

### Execute basic dialogue flow:
```powershell
$env:PYTHONIOENCODING="utf-8"
python scripts/run_demo.py --demo-type basic
```
This runs a CLI conversation simulating order lookups, stock checks, and returns.

### Run multi-step workflow evaluations:
```powershell
$env:PYTHONIOENCODING="utf-8"
python scripts/run_demo.py --demo-type workflow
```

### Generate performance logs:
```powershell
$env:PYTHONIOENCODING="utf-8"
python scripts/run_demo.py --demo-type performance
```

---

## 📝 Conversation Logging

All client interactions are sanitized and appended to the logs as raw JSON records:
*   Default path: `customer_agent.log`
*   Logged details include: Timestamps, unique `customer_id`, customer messages, generated agent responses, and active tool calls.
