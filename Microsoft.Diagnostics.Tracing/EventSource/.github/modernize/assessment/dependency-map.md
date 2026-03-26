# Dependency Map

The EventSourceSamples project declares 3 external NuGet dependencies, all from the Microsoft.Diagnostics.Tracing namespace, supporting ETW-based event tracing and registration.

## Dependencies

```mermaid
flowchart LR
    App["EventSourceSamples\n.NET Framework 4.5"]

    subgraph Observability["Observability / Tracing"]
        EventSource["Microsoft.Diagnostics.Tracing\n.EventSource v1.0.26"]
        EventRegister["Microsoft.Diagnostics.Tracing\n.EventRegister v1.0.26"]
        EventSourceRedist["Microsoft.Diagnostics.Tracing\n.EventSource.Redist v1.0.26"]
    end

    App -->|"tracing"| Observability
    EventRegister -.->|"registers manifests for"| EventSource
    EventSourceRedist -.->|"redistributable for"| EventSource
```
