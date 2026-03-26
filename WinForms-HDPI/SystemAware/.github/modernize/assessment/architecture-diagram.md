# Architecture Diagram

This is a Windows Forms desktop application targeting .NET Framework 4.7, demonstrating System-Aware High-DPI scaling across multiple UI forms.

## Application Architecture

```mermaid
flowchart TD
    subgraph Entry["Entry Point"]
        Program["Program.cs\n[STAThread]"]
    end
    subgraph UI["Presentation Layer - WinForms (.NET Framework 4.7)"]
        MainForm["MainForm\n(Shell / Navigator)"]
        Calendar["Calendar"]
        DataGridViewHeadersScaling["DataGridViewHeadersScaling"]
        MenuStripAndCheckedListBox["MenuStripAndCheckedListBox"]
        MultipleScalingPasses["MultipleScalingPasses"]
        RightAnchor["RightAnchor"]
    end
    subgraph Config["Configuration"]
        AppConfig["App.config\n(DpiAwareness settings)"]
        AppManifest["app.manifest\n(DPI mode declaration)"]
    end
    subgraph System["System / OS"]
        WinForms["System.Windows.Forms"]
        Drawing["System.Drawing"]
        ConfigManager["ConfigurationManager"]
    end

    Program -->|"Application.Run"| MainForm
    MainForm -->|"launches"| Calendar
    MainForm -->|"launches"| DataGridViewHeadersScaling
    MainForm -->|"launches"| MenuStripAndCheckedListBox
    MainForm -->|"launches"| MultipleScalingPasses
    MainForm -->|"launches"| RightAnchor
    MainForm -->|"reads DPI settings"| ConfigManager
    AppConfig -->|"loaded by"| ConfigManager
    AppManifest -->|"declares DPI awareness"| System
    UI -->|"built on"| WinForms
    UI -->|"rendering"| Drawing
```

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation Layer"]
        MainForm["MainForm"]
        Calendar["Calendar"]
        DGVHeaders["DataGridViewHeadersScaling"]
        MenuStrip["MenuStripAndCheckedListBox"]
        MultiScale["MultipleScalingPasses"]
        RightAnchor["RightAnchor"]
    end
    subgraph Infra["Infrastructure / Config"]
        AppConfig["App.config"]
        AppManifest["app.manifest"]
        ConfigMgr["ConfigurationManager"]
    end

    MainForm -->|"opens"| Calendar
    MainForm -->|"opens"| DGVHeaders
    MainForm -->|"opens"| MenuStrip
    MainForm -->|"opens"| MultiScale
    MainForm -->|"opens"| RightAnchor
    AppConfig -->|"provides DpiAwareness"| ConfigMgr
    ConfigMgr -.->|"read at load"| MainForm
    AppManifest -.->|"OS DPI mode"| MainForm
```
