# API Reference Guide

This reference details the core Python class classes and REST endpoints for the AI Customer Service Agent.

---

## 🐍 Python Classes Reference

### 1. `CustomerServiceAgent`
Located in [agent.py](file:///c:/Users/hp/OneDrive/Desktop/Code/AI-Customer-Support-Agent/src/customer_service_agent/agent.py).

*   `__init__(self, config: Optional[AgentConfig] = None, model: Optional[str] = None)`
    *   Initializes the client, creates tools registries, checks OpenAI key availability (falls back to **Demo Mode** if missing).
*   `chat(self, user_message: str, customer_id: Optional[str] = None) -> str`
    *   Main loop: sanitizes user inputs, parses sentiment analysis, runs tool execution routines, calls evaluations, logs the conversation, and returns the final response string.
*   `execute_workflow(self, workflow_steps: List[WorkflowStep]) -> List[WorkflowResult]`
    *   Executes structured workflows sequentially and returns results.
*   `get_performance_report(self, last_n_interactions: Optional[int] = 50) -> str`
    *   Compiles performance stats into a console dashboard report.
*   `get_conversation_summary(self) -> str`
    *   Queries the LLM for a brief summary of the active session.

### 2. `PerformanceEvaluator`
Located in [evaluator.py](file:///c:/Users/hp/OneDrive/Desktop/Code/AI-Customer-Support-Agent/src/customer_service_agent/evaluator.py).

*   `evaluate_interaction(self, user_input, agent_response, interaction_id, ...)`
    *   Evaluates accuracy, politeness, tool choices, and issues scores from 1 to 10.
*   `calculate_metrics(self, last_n)`
    *   Aggregates scoring metrics, sentiment trends, and tool usage frequencies.

---

## 🌐 FastAPI REST Endpoints

The web interface is defined in [api.py](file:///c:/Users/hp/OneDrive/Desktop/Code/AI-Customer-Support-Agent/src/customer_service_agent/api.py). Start it using:
```powershell
python src/customer_service_agent/api.py
```

### Endpoints Schema

#### `GET /`
*   **Description**: Retrieves general API system status and operational version.
*   **Response**: `{"status": "operational", "version": "1.0.0"}`

#### `GET /health`
*   **Description**: Checks if the agent instance is active.
*   **Response**: Returns initialized model, offline/demo flag status, and loaded tools.

#### `POST /chat`
*   **Description**: Submits a query to the agent session.
*   **Request Body**:
    ```json
    {
      "message": "Is order ORD-12345 delivered?",
      "customer_id": "CUST-001",
      "conversation_id": "optional-uuid"
    }
    ```
*   **Response**: Returns the agent's textual response alongside metadata.

#### `POST /workflow`
*   **Description**: Runs a sequence of multi-step customer inquiries.
*   **Request Body**:
    ```json
    {
      "steps": [
        {"name": "Check order status", "instruction": "Where is ORD-12345?"},
        {"name": "Request refund", "instruction": "I want a refund for ORD-12345"}
      ],
      "customer_id": "CUST-001"
    }
    ```

#### `GET/DELETE /conversation/{conversation_id}`
*   **Description**: Retrieve all message logs or clear conversation memory for a session ID.

#### `GET /performance`
*   **Description**: Returns performance metric reports (average scores, tool rates, response speeds).
