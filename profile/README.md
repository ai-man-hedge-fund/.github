## ai-man-hedge-fund

Core service + infra map:

```mermaid
flowchart LR
    user[Users / Operators]

    subgraph services[Services]
        iris[iris\nAPI + domain logic + batch control plane]
        batch[iris-batch\nworker + scheduler + standalone batch runtime]
        ai[iris-ai-service\nAI gateway + model orchestration]
    end

    subgraph infra[Infra]
        db[(PostgreSQL)]
        queue[Procrastinate / PG jobs]
        obj[Broker / parser inputs]
        models[LLM providers]
    end

    user --> iris
    batch -->|/api/v1/batch/runtime/*| iris
    iris -->|AI task requests| ai
    iris <--> db
    batch <--> db
    db --> queue
    queue --> batch
    obj --> iris
    obj --> batch
    ai --> models
    batch -. shared contracts .-> iris
```

### Repositories
- `iris`: main product/backend/frontend repository
- `iris-batch`: standalone batch worker and scheduler runtime
- `iris-ai-service`: isolated AI service for model/gateway execution

### Current architecture intent
- `iris` owns business logic, API contracts, and control-plane state
- `iris-batch` scales independently for async/batch execution while reusing backend runtime contracts
- `iris-ai-service` isolates model/provider integration from product services
- PostgreSQL remains the shared persistence/control substrate for the current runtime split
