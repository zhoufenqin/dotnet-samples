# .NET Upgrade Plan: PerMonitorDemo

## Overview

Upgrade **PerMonitorDemo** from **.NET Framework 4.7** (`net47`) to **.NET 10.0 LTS** (`net10.0-windows`).

This is a Windows Forms (WinForms) application demonstrating per-monitor DPI awareness. The upgrade targets .NET 10.0, the current Long Term Support (LTS) release.

### Source → Target
| | Version |
|---|---|
| **Source** | .NET Framework 4.7 (`net47`) |
| **Target** | .NET 10.0 LTS (`net10.0-windows`) |

---

## Projects

| Project | Kind | Current TFM | Target TFM |
|---------|------|-------------|------------|
| `PerMonitorDemo.csproj` | ClassicWinForms | `net47` | `net10.0-windows` |

---

## Assessment Summary

**749 total issues** found across 13 files:

| Rule | Severity | Count | Description |
|------|----------|-------|-------------|
| `Api.0001` | Mandatory | 678 | Binary incompatible WinForms/GDI+ APIs — require recompilation |
| `Api.0002` | Potential | 69 | Source incompatible APIs — require code changes |
| `Project.0001` | Mandatory | 1 | Project file must be converted to SDK-style |
| `Project.0002` | Mandatory | 1 | TargetFramework must change from `net47` to `net10.0-windows` |

**Affected technologies:**
- **Windows Forms** (678 issues) — WinForms is fully supported on `net10.0-windows`; issues are due to binary incompatibility that resolves after recompilation
- **GDI+ / System.Drawing** (65 issues) — requires `System.Drawing.Common` NuGet package or usage of modern drawing APIs
- **Legacy Configuration System** (4 issues) — `System.Configuration.ApplicationSettingsBase` requires `System.Configuration.ConfigurationManager` NuGet package

**Files with issues:**
- `PerMonitorDemo.csproj` (2) — project structure issues
- `SimpleLayout.Designer.cs` (385) — largest WinForms designer file
- `MainForm.Designer.cs` (134)
- `ScaleButtonImages.Designer.cs` (66)
- `CustomDrawing.Designer.cs` (48)
- `CustomDrawing.cs` (29)
- `ScaleButtonImages.cs` (37)
- `MainForm.cs` (22)
- `CurrentDPILabel.cs` (11)
- `Program.cs` (6)
- `Properties\Resources.Designer.cs` (4)
- `Properties\Settings.Designer.cs` (2)
- `SimpleLayout.cs` (3)

---

## Upgrade Tasks

### Task 1: Convert PerMonitorDemo.csproj to SDK-Style Format

**What**: Convert the legacy non-SDK-style `.csproj` file to modern SDK-style format.

**Why**: Modern .NET (5+) requires SDK-style project files. The current `.csproj` uses the old MSBuild format (rules `Project.0001`).

**Steps:**
1. Use the `sdk-style-conversion` builtin skill to convert `PerMonitorDemo.csproj`.
2. Do NOT change the target framework during this step.
3. Remove `packages.config` if present (packages are moved to `<PackageReference>` in `.csproj`).
4. Verify the project still builds after conversion.

**Files affected:** `PerMonitorDemo.csproj`

---

### Task 2: Update Target Framework and Add Required NuGet Packages

**What**: Change `TargetFramework` from `net47` to `net10.0-windows` and add NuGet packages needed for compatibility.

**Why**: The project must target `net10.0-windows` (rules `Project.0002`). The `-windows` suffix is required for WinForms projects. Missing packages for `System.Configuration` and `System.Drawing` need to be added.

**Steps:**
1. In `PerMonitorDemo.csproj`:
   - Change `<TargetFramework>net47</TargetFramework>` to `<TargetFramework>net10.0-windows</TargetFramework>`
   - Set `<UseWindowsForms>true</UseWindowsForms>` if not already set
   - Add NuGet package `System.Configuration.ConfigurationManager` (latest stable, e.g., 9.0.x) for `System.Configuration.ApplicationSettingsBase` used in `Properties\Settings.Designer.cs`
   - Add NuGet package `System.Drawing.Common` (latest stable, e.g., 9.0.x) for `System.Drawing.Icon`, `System.Drawing.Font`, `System.Drawing.GraphicsUnit`, `System.Drawing.FontStyle` used in `Properties\Resources.Designer.cs`, `SimpleLayout.Designer.cs`, `CustomDrawing.Designer.cs`, and `ScaleButtonImages.Designer.cs`
2. Build the project; the majority of `Api.0001` (binary incompatible) issues resolve automatically on recompilation with the new TFM.

**Files affected:** `PerMonitorDemo.csproj`

---

### Task 3: Fix Source Incompatible API Issues (Api.0002)

**What**: Resolve the 69 source-incompatible API usages that require code changes.

**Why**: `Api.0002` issues indicate APIs that changed signatures or became obsolete and will not compile against .NET 10.

**Key issues and fixes:**

#### Program.cs (6 mandatory Api.0001 issues — WinForms bootstrap)
- `Application.SetCompatibleTextRenderingDefault(false)` — this method was removed in .NET 6+. Remove the call entirely; .NET 10 WinForms does not use GDI text rendering by default.
- `Application.EnableVisualStyles()` — still valid, keep it.
- `Application.Run(new MainForm())` — still valid, keep it.

**File:** `Program.cs` line 14 — remove `Application.SetCompatibleTextRenderingDefault(false);`

#### Properties\Settings.Designer.cs (2 Api.0002 issues — Legacy Configuration)
- `ApplicationSettingsBase` constructor — still exists in `System.Configuration.ConfigurationManager` package. After adding the NuGet package in Task 2, this should compile without changes. Verify after Task 2.

#### Properties\Resources.Designer.cs (4 Api.0002 issues — System.Drawing.Icon)
- `System.Drawing.Icon` — still available via `System.Drawing.Common` package added in Task 2. Verify compilation after Task 2; if still failing, ensure `using System.Drawing;` is present and the package reference resolves.

#### SimpleLayout.Designer.cs (19 Api.0002 issues — System.Drawing types)
- `System.Drawing.Font`, `System.Drawing.FontStyle`, `System.Drawing.GraphicsUnit` — available via `System.Drawing.Common`. Verify after Task 2.
- Font constructor with `byte` charset parameter: `new System.Drawing.Font("Microsoft Sans Serif", 9F, System.Drawing.FontStyle.Regular, System.Drawing.GraphicsUnit.Point, ((byte)(0)))` — this overload is still supported. Verify compilation.

#### MainForm.cs (2 Api.0002 issues — WinForms)
- Review and resolve any WinForms API source incompatibilities flagged at lines with `Api.0002`. Most are likely GDI+ related and resolve after adding `System.Drawing.Common`.

#### CustomDrawing.cs (13 Api.0002 issues)
- `CustomDrawing.cs` uses GDI+ drawing extensively. After adding `System.Drawing.Common`, verify all drawing calls compile. Check for any APIs that were moved or renamed (e.g., `Graphics.DrawXxx` methods are all present).

#### ScaleButtonImages.cs (27 Api.0002 issues)
- Similarly GDI+ related — resolved by `System.Drawing.Common` package.

**Files affected:** `Program.cs`, `Properties\Settings.Designer.cs`, `Properties\Resources.Designer.cs`, `SimpleLayout.Designer.cs`, `MainForm.cs`, `CustomDrawing.cs`, `ScaleButtonImages.cs`

---

## Build Validation

After all tasks, run:
```
dotnet build WinForms-HDPI\PerMonitorAware\PerMonitorDemo.sln
```

Expected: Build succeeds with 0 errors. Warnings about obsolete APIs may remain but should not block the build.

---

## Notes

- WinForms on .NET 10 requires the `-windows` platform suffix (`net10.0-windows`).
- `System.Drawing.Common` on .NET 6+ is Windows-only; since this project targets `net10.0-windows` this is acceptable.
- `Application.SetCompatibleTextRenderingDefault` was removed; its removal is safe as .NET 10 WinForms defaults to GDI+ text rendering equivalent.
- The large number of `Api.0001` (binary incompatible) issues (678) is expected for WinForms projects migrating from .NET Framework — they are not compile errors but indicate that the existing `.dll` is binary-incompatible. After recompiling with the new TFM, these issues disappear.
