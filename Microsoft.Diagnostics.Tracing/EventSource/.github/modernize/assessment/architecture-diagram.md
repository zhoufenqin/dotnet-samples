# Architecture Diagram

A .NET Framework 4.5 console application demonstrating ETW (Event Tracing for Windows) patterns using Microsoft.Diagnostics.Tracing.EventSource, with multiple sample implementations and an event listener for console output.

## Application Architecture

```mermaid
flowchart TD
    subgraph Entry["Entry Point"]
        Program["Program.cs\n(Main Entry)"]
    end
    subgraph App["Application Layer - .NET Framework 4.5"]
        AllSamples["AllSamples\n(Sample Orchestrator)"]
        CommonListener["CommonEventListener\n(Console EventListener)"]
    end
    subgraph Samples["ETW Sample Implementations"]
        Minimal["MinimalEventSource"]
        Customized["CustomizedEventSource"]
        PerfOpt["CustomizedForPerfEventSource"]
        EventLog["EventLogEventSource"]
        Localized["LocalizedEventSource"]
        LocalizedEL["LocalizedEventLogEventSource"]
    end
    subgraph Infrastructure["Infrastructure"]
        ETW["ETW - Event Tracing for Windows"]
        WinEventLog[("Windows Event Log")]
        ResxFiles[("Localization Resources\n.resx files")]
    end
    subgraph NuGet["NuGet Dependencies"]
        EventSourceLib["Microsoft.Diagnostics.Tracing\n.EventSource v1.0.26"]
        EventRegister["Microsoft.Diagnostics.Tracing\n.EventRegister v1.0.26"]
    end

    Program -->|"invokes"| AllSamples
    AllSamples -->|"enables"| CommonListener
    AllSamples -->|"runs"| Minimal
    AllSamples -->|"runs"| Customized
    AllSamples -->|"runs"| PerfOpt
    AllSamples -->|"runs"| EventLog
    AllSamples -->|"runs"| Localized
    AllSamples -->|"runs"| LocalizedEL
    CommonListener -->|"captures events via"| ETW
    Minimal -->|"emits events via"| ETW
    Customized -->|"emits events via"| ETW
    PerfOpt -->|"emits events via"| ETW
    EventLog -->|"writes to"| WinEventLog
    Localized -->|"reads"| ResxFiles
    LocalizedEL -->|"reads"| ResxFiles
    LocalizedEL -->|"writes to"| WinEventLog
    EventSourceLib -->|"provides API"| Samples
    EventRegister -->|"registers manifests"| ETW
```

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        Program["Program"]
    end
    subgraph BusinessLogic["Business Logic"]
        AllSamples["AllSamples\n(Orchestrator)"]
        CommonListener["CommonEventListener"]
    end
    subgraph EventSources["EventSource Implementations"]
        Minimal["MinimalEventSource"]
        Customized["CustomizedEventSource"]
        PerfOpt["CustomizedForPerfEventSource"]
        EventLog["EventLogEventSource"]
        Localized["LocalizedEventSource"]
        LocalizedEL["LocalizedEventLogEventSource"]
    end
    subgraph Infra["Infrastructure"]
        ETW["ETW Runtime"]
        ResX["Resx Resources"]
    end

    Program -->|"delegates to"| AllSamples
    AllSamples -->|"registers"| CommonListener
    AllSamples -->|"invokes"| Minimal
    AllSamples -->|"invokes"| Customized
    AllSamples -->|"invokes"| PerfOpt
    AllSamples -->|"invokes"| EventLog
    AllSamples -->|"invokes"| Localized
    AllSamples -->|"invokes"| LocalizedEL
    CommonListener -.->|"listens via"| ETW
    Minimal -.->|"emits to"| ETW
    Customized -.->|"emits to"| ETW
    PerfOpt -.->|"emits to"| ETW
    EventLog -.->|"emits to"| ETW
    Localized -->|"loads"| ResX
    LocalizedEL -->|"loads"| ResX
    LocalizedEL -.->|"emits to"| ETW
```
