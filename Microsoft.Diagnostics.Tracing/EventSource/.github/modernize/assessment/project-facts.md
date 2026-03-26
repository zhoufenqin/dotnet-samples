# Project Facts

| Fact | Value | Confidence | Evidence |
|------|-------|-----------|----------|
| Application Name | EventSourceSamples | high | AssemblyName in EventSource.csproj |
| Application Type | Console Application | high | OutputType=Exe in EventSource.csproj |
| Runtime Environment | .NET Framework 4.5 | high | TargetFrameworkVersion=v4.5 in EventSource.csproj |
| Language | C# | high | .cs source files and .csproj project file |
| Architecture Pattern | Sample/Demo Application | high | Multiple numbered sample files (00_-50_) with orchestrator pattern |
| External Services | None | high | No external service dependencies detected |
| Testing Framework | None | high | No test projects or testing frameworks detected |
| Health Checks | None | high | No health check configuration detected |
| Compliance Requirements | None | medium | No compliance markers detected |
| Data Classification | None | medium | No data classification markers detected |
| Hardware Requirements | None | high | No hardware requirements defined |
| NuGet Dependencies | 3 | high | packages.config: Microsoft.Diagnostics.Tracing.EventSource, EventRegister, EventSource.Redist (all v1.0.26) |
| Localization Support | English, French (fr-FR) | high | LesResource.resx and LesResource.fr-FR.resx found |
| Windows Event Log Integration | Yes | high | EventLogEventSource and LocalizedEventLogEventSource samples present |
| ETW Integration | Yes | high | All samples use Event Tracing for Windows (ETW) |
