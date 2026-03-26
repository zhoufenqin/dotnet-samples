# Dependency Map

PerMonitorDemo is a .NET Framework 4.7 WinForms application with 12 framework assembly references and no external NuGet packages.

## Dependencies

```mermaid
flowchart LR
    App["PerMonitorDemo\n(.NET Framework 4.7)"]

    subgraph UI["UI Frameworks"]
        WinForms["System.Windows.Forms"]
        Drawing["System.Drawing"]
    end
    subgraph Core["Core Runtime"]
        System["System"]
        SystemCore["System.Core"]
        MicrosoftCSharp["Microsoft.CSharp"]
    end
    subgraph Data["Data"]
        SystemData["System.Data"]
        DataSetExtensions["System.Data.DataSetExtensions"]
        XmlLinq["System.Xml.Linq"]
        SystemXml["System.Xml"]
    end
    subgraph Infra["Infrastructure"]
        Configuration["System.Configuration"]
        Deployment["System.Deployment"]
        NetHttp["System.Net.Http"]
    end

    App -->|"ui"| UI
    App -->|"core"| Core
    App -->|"data"| Data
    App -->|"infrastructure"| Infra
```
