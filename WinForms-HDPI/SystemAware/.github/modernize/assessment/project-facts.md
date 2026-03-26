# Project Facts

## Application Overview

| Fact | Value |
|------|-------|
| **Application Name** | HighDpiDemo |
| **Application Type** | Windows Desktop Application (WinForms) |
| **Version** | 1.0.0.0 |
| **License** | MIT |

## Runtime Environment

| Fact | Value |
|------|-------|
| **Runtime** | .NET Framework 4.7 |
| **Target Framework** | v4.7 (`net47`) |
| **Output Type** | WinExe |
| **Language** | C# |
| **Platform** | Windows (x86/x64, AnyCPU) |

## Architecture

| Fact | Value |
|------|-------|
| **Architecture Pattern** | Monolithic WinForms Desktop Application |
| **UI Framework** | System.Windows.Forms (.NET Framework 4.7) |
| **DPI Awareness Mode** | PerMonitorV2 (declared in app.manifest; runtime setting via App.config) |
| **Entry Point** | `Program.cs` — `[STAThread] static void Main()` |
| **Forms / Screens** | MainForm, Calendar, DataGridViewHeadersScaling, MenuStripAndCheckedListBox, MultipleScalingPasses, RightAnchor |

## Dependencies

| Fact | Value |
|------|-------|
| **External NuGet Packages** | None |
| **Framework References** | System, System.Core, System.Configuration, System.Data, System.Drawing, System.Net.Http, System.Windows.Forms, System.Xml, System.Xml.Linq, System.Deployment, System.Data.DataSetExtensions, Microsoft.CSharp |

## Testing

| Fact | Value |
|------|-------|
| **Testing Frameworks** | None detected |
| **Test Projects** | None |

## Configuration

| Fact | Value |
|------|-------|
| **App.config** | Present — defines `DpiAwareness=PerMonitorV2` and optional per-control DPI fix toggles |
| **app.manifest** | Present — declares OS compatibility and UAC execution level (`asInvoker`) |
| **Supported Runtime** | `v4.0` (`.NETFramework,Version=v4.7`) |

## Assessment Summary

| Fact | Value |
|------|-------|
| **Discovered Issues** | 1 |
| **Story Points** | 3 |
| **Severity Breakdown** | Optional: 1, Mandatory: 0, Potential: 0, Informational: 0 |
