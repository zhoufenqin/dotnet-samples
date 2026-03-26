# Architecture Diagram

This WinForms application demonstrates per-monitor DPI awareness techniques across multiple demo forms, each showcasing a different approach to high-DPI scaling.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer - WinForms (.NET Framework 4.7)"]
        MainForm["MainForm\n(Hub / Launcher)"]
    end
    subgraph Demo["Demo Forms Layer"]
        SimpleLayout["SimpleLayout\n(Auto DPI Scaling)"]
        CustomDrawing["CustomDrawing\n(Custom Graphics)"]
        ScaleButtonImages["ScaleButtonImages\n(Image Scaling)"]
    end
    subgraph Controls["Custom Controls"]
        CurrentDPILabel["CurrentDPILabel\n(DPI Display Label)"]
        ImageButton["ImageButton\n(DPI-Aware Button)"]
    end
    subgraph Config["Configuration"]
        AppConfig["App.config\n(PerMonitorV2 DPI Awareness)"]
        AppManifest["app.manifest\n(Windows 10 Compatibility)"]
    end

    AppConfig -->|"configures DPI mode"| MainForm
    AppManifest -->|"UAC & compatibility"| MainForm
    MainForm -->|"launches"| SimpleLayout
    MainForm -->|"launches"| CustomDrawing
    MainForm -->|"launches"| ScaleButtonImages
    SimpleLayout -->|"uses"| CurrentDPILabel
    CustomDrawing -->|"uses"| CurrentDPILabel
    ScaleButtonImages -->|"uses"| CurrentDPILabel
    ScaleButtonImages -->|"contains"| ImageButton
```

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        MainForm["MainForm"]
        SimpleLayout["SimpleLayout"]
        CustomDrawing["CustomDrawing"]
        ScaleButtonImages["ScaleButtonImages"]
    end
    subgraph Controls["Custom Controls"]
        CurrentDPILabel["CurrentDPILabel"]
        ImageButton["ImageButton"]
    end
    subgraph Entry["Entry Point"]
        Program["Program"]
    end

    Program -->|"creates and runs"| MainForm
    MainForm -->|"opens"| SimpleLayout
    MainForm -->|"opens"| CustomDrawing
    MainForm -->|"opens"| ScaleButtonImages
    MainForm -->|"embeds"| CurrentDPILabel
    SimpleLayout -->|"embeds"| CurrentDPILabel
    CustomDrawing -->|"embeds"| CurrentDPILabel
    ScaleButtonImages -->|"embeds"| CurrentDPILabel
    ScaleButtonImages -->|"contains"| ImageButton
```
