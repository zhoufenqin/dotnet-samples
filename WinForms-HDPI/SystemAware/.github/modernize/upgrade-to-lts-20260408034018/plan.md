# Upgrade Plan: HighDpiDemo from .NET 4.7 to .NET 10.0

## Overview

Upgrade the **HighDpiDemo** WinForms project from `.NET Framework 4.7` (net47) to `.NET 10.0 for Windows` (net10.0-windows).

- **Source framework**: net47 (.NET Framework 4.7)
- **Target framework**: net10.0-windows (.NET 10.0 LTS, Windows-only)
- **Projects**: 1 (HighDpiDemo.csproj)
- **Assessment**: 1,288 issues — primarily Windows Forms API availability resolved by targeting net10.0-windows; 102 legacy control replacements required; GDI+ and legacy config require NuGet packages.

### Selected Strategy
**All-At-Once** — All projects upgraded simultaneously in a single operation.  
**Rationale**: 1 project (HighDpiDemo.csproj), currently net47, distinct work areas (SDK conversion, TFM update, legacy control migration, package additions, code fixes).

---

## Tasks

### 01-sdk-style-and-tfm

**Convert HighDpiDemo.csproj to SDK-style and update target framework to net10.0-windows**

- Convert `HighDpiDemo.csproj` from legacy format to SDK-style (`<Project Sdk="Microsoft.NET.Sdk">`)
- Set `<TargetFramework>net10.0-windows</TargetFramework>` and `<UseWindowsForms>true</UseWindowsForms>`
- Set `<OutputType>WinExe</OutputType>`
- Remove all explicit assembly references that are included in the framework (System, System.Core, System.Drawing, System.Windows.Forms, etc.)
- Remove all `<Compile>`, `<EmbeddedResource>`, `<None>` items from the project file (SDK-style auto-includes)
- Keep `<ApplicationManifest>app.manifest</ApplicationManifest>` and `<ApplicationIcon>` if present
- Add `<System.Drawing.Common>` NuGet package (latest stable) for GDI+/System.Drawing support
- Add `<System.Configuration.ConfigurationManager>` NuGet package (latest stable) for legacy app.config support
- Run `dotnet restore` to verify package resolution
- Files: `HighDpiDemo.csproj`

### 02-legacy-controls-migration

**Replace legacy Windows Forms controls removed from .NET Core/5+**

Files affected: `MenuStripAndCheckedListBox.Designer.cs`, `MenuStripAndCheckedListBox.cs`, `Calendar.Designer.cs`, `Calendar.cs`

Legacy controls to migrate (102 occurrences):
- `System.Windows.Forms.StatusBar` → `System.Windows.Forms.StatusStrip`
- `System.Windows.Forms.DataGrid` → `System.Windows.Forms.DataGridView`
- `System.Windows.Forms.ContextMenu` → `System.Windows.Forms.ContextMenuStrip`
- `System.Windows.Forms.MainMenu` → `System.Windows.Forms.MenuStrip`
- `System.Windows.Forms.MenuItem` → `System.Windows.Forms.ToolStripMenuItem`
- `System.Windows.Forms.ToolBar` → `System.Windows.Forms.ToolStrip`

Update all Designer files to use modern control equivalents. Update corresponding code-behind files to handle API differences in event signatures and properties. Verify all menu, toolbar, and status bar functionality is preserved.

### 03-build-and-fix

**Build solution and fix remaining compilation errors**

Run `dotnet build HighDpiDemo.sln`. Fix all remaining compilation errors from:
- Source-incompatible APIs (40 issues in `Properties\Settings.Designer.cs`, `MultipleScalingPasses.Designer.cs`, `MenuStripAndCheckedListBox.cs`, `MainForm.cs`, `Calendar.Designer.cs`, `Calendar.cs`)
- Any issues from SDK-style conversion or legacy control migration not yet addressed
- `System.Drawing.Font` / `System.Drawing.FontStyle` / `System.Drawing.GraphicsUnit` — resolved by `System.Drawing.Common` package
- App.config / Settings — resolved by `System.Configuration.ConfigurationManager` package
- Build must complete with 0 errors

---

## Dependency Order

Tasks must be executed in order:
1. `01-sdk-style-and-tfm` — must complete before any other task
2. `02-legacy-controls-migration` — requires SDK-style conversion complete
3. `03-build-and-fix` — final pass after all code changes

---

## Notes

- The net10.0-windows target suffix is **required** — Windows Forms is not available in platform-agnostic net10.0
- Most of the 1,246 "binary incompatible" API flags are Windows Forms types that become available once `-windows` TFM is applied; no code changes needed for those
- Legacy controls (StatusBar, DataGrid, ContextMenu, MainMenu, MenuItem, ToolBar) were removed from .NET Core and require code changes
- `app.manifest` and DPI-awareness settings must be preserved — this project is a High DPI demo
- No NuGet packages existed pre-upgrade; packages added are: `System.Drawing.Common`, `System.Configuration.ConfigurationManager`
