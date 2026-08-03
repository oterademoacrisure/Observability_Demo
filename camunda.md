
Here’s your full Markdown file ready to paste into GitHub as camunda_setup.md:

markdown
# `camunda_setup.md`

## 🏗️ Architecture Overview

The system implements a **Dual-Path Execution Architecture** allowing votes to enter the processing pipeline either directly through the user interface or via a Camunda BPMN workflow process.

```mermaid
flowchart TD
    subgraph DUAL_PATH ["🏛️ DUAL-PATH ARCHITECTURE"]
        direction TB

        subgraph PATH_DIRECT ["Path 1: Direct User Action"]
            UI["🖥️ Web UI<br/><code>(:5000)</code>"]
        end

        subgraph PATH_CAMUNDA ["Path 2: Camunda BPMN Orchestration"]
            ENGINE["⚡ Camunda Engine<br/><code>(:8081)</code>"]
            WORKER["🐍 Python Task Worker"]
        end

        APP["🔥 Flask Vote App<br/><code>(Tagged: DIRECT UI)</code>"]
        DB[("💾 Redis / PostgreSQL DB")]
    end

    UI -->|HTTP Direct POST| APP
    ENGINE -->|REST Start / Poll| WORKER
    WORKER -->|HTTP Orchestrated POST| APP
    APP -->|Pushes Vote Data| DB

    style DUAL_PATH fill:#fafafa,stroke:#333,stroke-width:2px
    style PATH_DIRECT fill:#e6f3ff,stroke:#2b6cb0,stroke-width:1px
    style PATH_CAMUNDA fill:#fff5eb,stroke:#c05621,stroke-width:1px
    style UI fill:#ebf8ff,stroke:#3182ce,color:#2b6cb0,stroke-width:2px
    style ENGINE fill:#fffaf0,stroke:#dd6b20,color:#c05621,stroke-width:2px
    style WORKER fill:#feebc8,stroke:#d69e2e,color:#744210,stroke-width:2px
    style APP fill:#e6fffa,stroke:#319795,color:#234e52,stroke-width:2px
    style DB fill:#edf2f7,stroke:#4a5568,color:#1a202c,stroke-width:2px
🔄 Execution Sequence Diagram
mermaid
sequenceDiagram
    autonumber
    actor User
    participant Browser as Web UI (:5000)
    participant Flask as Flask App (K8s Pod)
    participant Camunda as Camunda Engine (:8081)
    participant Worker as Python External Worker
    participant Storage as Redis / PostgreSQL

    %% Path 1: Direct UI Vote
    rect rgb(240, 248, 255)
        Note over User, Storage: Path 1: Direct UI Vote Path
        User->>Browser: Casts vote (clicks option)
        Browser->>Flask: HTTP POST / (User-Agent: Mozilla/5.0...)
        Flask->>Storage: Store vote directly
        Flask-->>Browser: Return confirmation & update UI
    end

    %% Path 2: Camunda Orchestrated Vote
    rect rgb(255, 245, 238)
        Note over User, Storage: Path 2: Camunda BPMN Orchestration Path
        Camunda->>Camunda: Workflow instance started via REST API
        Worker->>Camunda: Polls external task topics (vote-processing)
        Camunda-->>Worker: Lock and fetch task
        Worker->>Flask: HTTP POST / with X-Source-Orchestrator: Camunda (User-Agent: python-requests)
        Flask->>Storage: Store orchestrated vote
        Flask-->>Worker: Return success
        Worker->>Camunda: Complete external task
    end
🛠️ Step-by-Step Deployment & Commands
1. Port Forwarding (K8s → Local)
Forward the Camunda Engine REST & Cockpit service port to 8081:

powershell
kubectl port-forward svc/camunda 8081:8080
2. Deploy the BPMN Process Diagram
Deploy vote.bpmn from the project repository:

powershell
curl.exe -X POST "http://localhost:8081/engine-rest/deployment/create" `
  -F "deployment-name=vote-deployment" `
  -F "data=@.\worker-camunda\vote.bpmn"
3. Start a Workflow Instance via REST
Trigger the VoteProcess workflow instance with parameters:

powershell
$body = @{
    variables = @{
        vote = @{ value = "a"; type = "String" }
    }
} | ConvertTo-Json

Invoke-RestMethod -Uri "http://localhost:8081/engine-rest/process-definition/key/VoteProcess/start" `
  -Method Post `
  -ContentType "application/json" `
  -Body $body
4. Run External Task Worker
Execute the Python polling worker:

powershell
python worker-camunda/Camundaflow.py
📊 Prometheus & Grafana Monitoring
To track Camunda engine performance and CPU consumption, use the PromQL metric query:

promql
rate(container_cpu_usage_seconds_total{pod=~"camunda-.*", container="camunda"}[2m])
📑 Log Breakdown & Flow Verification
Run the following command to observe incoming execution logs:

powershell
kubectl logs -f deployment/vote
Log Output Analysis
plaintext
# ----------------------------------------------------------------------
# PATH 1: Camunda-Orchestrated Votes
# Identified by User-Agent: "python-requests/2.31.0"
# Triggered via REST process execution through External Worker
# ----------------------------------------------------------------------
[2026-08-03 05:52:11,816] INFO in app: Received vote for a
127.0.0.1 - - [03/Aug/2026:05:52:11 +0000] "POST / HTTP/1.1" 200 1697 "-" "python-requests/2.31.0"

[2026-08-03 05:53:40,615] INFO in app: Received vote for a
127.0.0.1 - - [03/Aug/2026:05:53:40 +0000] "POST / HTTP/1.1" 200 1697 "-" "python-requests/2.31.0"

# ----------------------------------------------------------------------
# PATH 2: Direct UI Votes
# Identified by User-Agent: "Mozilla/5.0... Chrome/..."
# Triggered directly by end-users via Browser (http://localhost:5000)
# ----------------------------------------------------------------------
[2026-08-03 05:54:00,391] INFO in app: Received vote for a
127.0.0.1 - - [03/Aug/2026:05:54:00 +0000] "POST / HTTP/1.1" 200 1697 "http://localhost:5000/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64)..."

[2026-08-03 05:54:00,755] INFO in app: Received vote for b
127.0.0.1 - - [03/Aug/2026:05:54:00 +0000] "POST / HTTP/1.1" 200 1697 "http://localhost:





<img width="959" height="374" alt="image" src="https://github.com/user-attachments/assets/bd14d9b6-4e93-487d-8157-f4f10b6d4ee0" />
🛠️ Step-by-Step Deployment & Commands
1. Port Forwarding (K8s to Local)
Forward the Camunda Engine REST & Cockpit service port to 8081:

PowerShell
kubectl port-forward svc/camunda 8081:8080
2. Deploy the BPMN Process Diagram
Deploy vote.bpmn from the project repository:

PowerShell
curl.exe -X POST "http://localhost:8081/engine-rest/deployment/create" `
  -F "deployment-name=vote-deployment" `
  -F "data=@.\worker-camunda\vote.bpmn"
3. Start a Workflow Instance via REST
Trigger the VoteProcess workflow instance with parameters:

PowerShell
$body = @{
    variables = @{
        vote = @{ value = "a"; type = "String" }
    }
} | ConvertTo-Json

Invoke-RestMethod -Uri "http://localhost:8081/engine-rest/process-definition/key/VoteProcess/start" `
  -Method Post `
  -ContentType "application/json" `
  -Body $body
4. Run External Task Worker
Execute the Python polling worker:

PowerShell
python worker-camunda/Camundaflow.py

The system implements a **Dual-Path Execution Architecture** allowing votes to enter the processing pipeline either directly through the user interface or via a Camunda BPMN workflow process.

```mermaid
flowchart TD
    subgraph DUAL_PATH ["🏛️ DUAL-PATH ARCHITECTURE"]
        direction TB

        subgraph PATH_DIRECT ["Path 1: Direct User Action"]
            UI["🖥️ Web UI<br/><code>(:5000)</code>"]
        end

        subgraph PATH_CAMUNDA ["Path 2: Camunda BPMN Orchestration"]
            ENGINE["⚡ Camunda Engine<br/><code>(:8081)</code>"]
            WORKER["🐍 Python Task Worker"]
        end

        APP["🔥 Flask Vote App<br/><code>(Tagged: DIRECT UI)</code>"]
        DB[("💾 Redis / PostgreSQL DB")]
    end

    UI -->|HTTP Direct POST| APP
    ENGINE -->|REST Start / Poll| WORKER
    WORKER -->|HTTP Orchestrated POST| APP
    APP -->|Pushes Vote Data| DB

    style DUAL_PATH fill:#fafafa,stroke:#333,stroke-width:2px
    style PATH_DIRECT fill:#e6f3ff,stroke:#2b6cb0,stroke-width:1px
    style PATH_CAMUNDA fill:#fff5eb,stroke:#c05621,stroke-width:1px
    style UI fill:#ebf8ff,stroke:#3182ce,color:#2b6cb0,stroke-width:2px
    style ENGINE fill:#fffaf0,stroke:#dd6b20,color:#c05621,stroke-width:2px
    style WORKER fill:#feebc8,stroke:#d69e2e,color:#744210,stroke-width:2px
    style APP fill:#e6fffa,stroke:#319795,color:#234e52,stroke-width:2px
    style DB fill:#edf2f7,stroke:#4a5568,color:#1a202c,stroke-width:2px



