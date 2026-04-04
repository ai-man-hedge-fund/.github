## ai-man-hedge-fund

Core service map:

```mermaid
flowchart LR
    iris[iris\nAPI + domain logic + batch control plane]
    batch[iris-batch\nworker + scheduler + standalone batch runtime]
    ai[iris-ai-service\nAI gateway + model orchestration]

    batch -->|/api/v1/batch/runtime/*| iris
    iris -->|AI task requests| ai
    batch -. shared contracts .-> iris
```

### Repositories
- `iris`: main product/backend/frontend repository
- `iris-batch`: standalone batch worker and scheduler runtime
- `iris-ai-service`: isolated AI service for model/gateway execution

### Current architecture intent
- `iris` owns business logic and API contracts
- `iris-batch` scales independently for async/batch execution
- `iris-ai-service` isolates model/provider integration from product services
