🏗️ Architecture Overview
The system implements a Dual-Path Execution Architecture allowing votes to enter the processing pipeline either directly through the user interface or via a Camunda BPMN workflow process.

Code snippet
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
Code snippet
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
