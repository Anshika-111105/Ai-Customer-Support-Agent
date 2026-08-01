# Installation Guide

This guide describes how to install and configure the AI Customer Service Agent.

## Requirements

*   **Python**: Version 3.8 to 3.11.
*   **Operating System**: Windows, macOS, or Linux.
*   **API Key**: OpenAI API Key (required for live production model interactions).

---

## 🛠️ Step-by-Step Setup

### 1. Clone your repository
Navigate to your project parent directory and ensure all files are structured at the root level:
```powershell
git clone https://github.com/Anshika-111105/Ai-Customer-Support-Agent.git
cd Ai-Customer-Support-Agent
```

### 2. Configure Virtual Environment
Create and activate an isolated Python virtual environment to manage dependencies locally:
```powershell
# Create venv
python -m venv venv

# Activate on Windows (PowerShell)
venv\Scripts\activate

# Activate on macOS/Linux
source venv/bin/activate
```

---

## ⚠️ Windows Application Control Bypass (AppLocker/WDAC)

If you are running on a Windows machine with active Software Restriction Policies, WDAC, or AppLocker, executing wrapper binaries like `venv\Scripts\pip.exe` or `venv\Scripts\pytest.exe` directly may result in a permission error:
> `Program 'pip.exe' failed to run: An Application Control policy has blocked this file`

### Resolution
You can bypass this security block by invoking the corresponding Python modules directly via the trusted `python.exe` binary:

*   **Install requirements**:
    ```powershell
    python -m pip install -r requirements.txt
    ```
*   **Upgrade Pip**:
    ```powershell
    python -m pip install --upgrade pip
    ```
*   **Run tests**:
    ```powershell
    python -m pytest tests/
    ```

---

## 🚀 Local Editable Package Installation

To map internal absolute and relative package paths correctly, install the customer service agent project in **editable mode**:
```powershell
python -m pip install -e .
```
This registers the `customer_service_agent` module directly inside your active environment so you can import it or execute tests from any workspace subdirectory.
