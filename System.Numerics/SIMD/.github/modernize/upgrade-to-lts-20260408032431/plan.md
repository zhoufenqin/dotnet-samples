# .NET Upgrade Plan: .NET Framework 4.5 → .NET 10.0 (LTS)

## Overview

**Solution:** `System.Numerics.Vectors.SampleCode.sln`  
**Source framework:** .NET Framework 4.5 (`net45`)  
**Target framework:** .NET 10.0 LTS (`net10.0-windows`)  
**Projects:** 2 WPF applications

Both projects are classic WPF applications. The upgrade targets `net10.0-windows` to retain WPF support on Windows. The `System.Numerics.Vectors` NuGet package (1.1.5-beta) used in both projects is now built into the .NET runtime and must be removed. The RayTracer project additionally uses `System.Drawing.Bitmap` / GDI+ APIs that require the `System.Drawing.Common` NuGet package on .NET 10.

---

## Projects

| Project | Current TFM | Target TFM | SDK-style | Issues |
|---------|-------------|------------|-----------|--------|
| `Mandelbrot\Mandelbrot.csproj` | net45 | net10.0-windows | No | 146 |
| `RayTracer\RayTracer.csproj` | net45 | net10.0-windows | No | 158 |

---

## Task Breakdown

### Task 001 — Convert Mandelbrot.csproj to SDK-style and upgrade to net10.0-windows

**Files:** `Mandelbrot\Mandelbrot.csproj`

**Steps:**
1. Convert `Mandelbrot\Mandelbrot.csproj` to SDK-style format using `<Project Sdk="Microsoft.NET.Sdk">`.
2. Set `<TargetFramework>net10.0-windows</TargetFramework>` and add `<UseWPF>true</UseWPF>`.
3. Remove the `System.Numerics.Vectors` 1.1.5-beta `PackageReference` — this functionality is included in the .NET runtime.
4. Remove `packages.config` if present.
5. Build the project to validate: `dotnet build Mandelbrot\Mandelbrot.csproj`.

**Expected outcome:** Project compiles successfully targeting net10.0-windows.

---

### Task 002 — Fix WPF API binary-incompatible issues in Mandelbrot

**Files:** `Mandelbrot\MainWindow.xaml.cs`, `Mandelbrot\App.xaml.cs`, `Mandelbrot\WPFHelpers.cs`, `Mandelbrot\FlyThru.xaml.cs`

**Issues:** 143 instances of `Api.0001` (WPF APIs binary-incompatible with net10.0-windows)

The WPF APIs flagged are all available in .NET 10 WPF (net10.0-windows) but may require code adjustments for the following patterns detected:

- **`PresentationSource.FromVisual` / `CompositionTarget.TransformToDevice`** (`MainWindow.xaml.cs` line 253–261): Replace DPI detection via `CompositionTarget.TransformToDevice` with the modern WPF DPI API `VisualTreeHelper.GetDpi(Visual)` returning `DpiScale`. Example:
  ```csharp
  // Old (net45):
  var ps = PresentationSource.FromVisual(this);
  var ct = ps.CompositionTarget;
  double dpiX = (double)ct.TransformToDevice.M11;
  double dpiY = (double)ct.TransformToDevice.M22;

  // New (net10.0-windows):
  var dpiInfo = VisualTreeHelper.GetDpi(this);
  double dpiX = dpiInfo.PixelsPerInchX;
  double dpiY = dpiInfo.PixelsPerInchY;
  ```
- **Event handlers using `RoutedEventArgs`, `SizeChangedEventArgs`, `MouseButtonEventArgs`**: These APIs exist in net10.0-windows WPF — verify event handler signatures are correct and rebuild.
- **`System.Windows.Point`, `System.Windows.Media.Matrix` usages**: These APIs exist in net10.0-windows — verify usages compile after SDK-style conversion.
- All remaining `Api.0001` hits in `WPFHelpers.cs` and `FlyThru.xaml.cs` are WPF types present in net10.0-windows. Rebuild to confirm they resolve after SDK-style conversion.

**Steps:**
1. Update `MainWindow.xaml.cs` lines 253–261: replace `PresentationSource`/`CompositionTarget.TransformToDevice` DPI logic with `VisualTreeHelper.GetDpi(this)`.
2. Rebuild: `dotnet build Mandelbrot\Mandelbrot.csproj` and fix any remaining compiler errors.

---

### Task 003 — Convert RayTracer.csproj to SDK-style and upgrade to net10.0-windows

**Files:** `RayTracer\RayTracer.csproj`

**Steps:**
1. Convert `RayTracer\RayTracer.csproj` to SDK-style format: `<Project Sdk="Microsoft.NET.Sdk">`.
2. Set `<TargetFramework>net10.0-windows</TargetFramework>` and add `<UseWPF>true</UseWPF>`.
3. Remove the `System.Numerics.Vectors` 1.1.5-beta `PackageReference`.
4. Add `System.Drawing.Common` package reference (required for `System.Drawing.Bitmap` GDI+ usage):
   ```xml
   <PackageReference Include="System.Drawing.Common" Version="11.0.0-preview.2.26159.112" />
   ```
5. Remove `packages.config` if present.
6. Build: `dotnet build RayTracer\RayTracer.csproj`.

---

### Task 004 — Fix GDI+ source-incompatible issues in RayTracer

**Files:** `RayTracer\Camera.cs` (lines 101, 132, 160, 172), `RayTracer\MainWindow.xaml.cs` (lines 287, 289)

**Issues:** 8 instances of `Api.0002` (GDI+ / `System.Drawing.Bitmap` source-incompatible on non-Windows)

These APIs (`System.Drawing.Bitmap`, `Bitmap.SetPixel`, `Bitmap.GetHbitmap`) are available via `System.Drawing.Common` on Windows. After adding the `System.Drawing.Common` package (Task 003), these should compile. Additionally:

- `Bitmap.GetHbitmap()` is Windows-only. Add `[System.Runtime.Versioning.SupportedOSPlatform("windows")]` annotation to the `BitmapToSource` method in `MainWindow.xaml.cs` if analyzer warnings are raised.
- Add the following to `RayTracer.csproj` to suppress the cross-platform warning if needed:
  ```xml
  <PropertyGroup>
    <PlatformTarget>x86</PlatformTarget>
  </PropertyGroup>
  ```
  Or add `<RuntimeIdentifier>win-x64</RuntimeIdentifier>`.

**Steps:**
1. Confirm `System.Drawing.Common` package is added (done in Task 003).
2. Add `[System.Runtime.Versioning.SupportedOSPlatform("windows")]` to `BitmapToSource` method in `RayTracer\MainWindow.xaml.cs`.
3. Rebuild: `dotnet build RayTracer\RayTracer.csproj` and resolve any remaining errors.

---

### Task 005 — Full solution build validation

**Files:** `System.Numerics.Vectors.SampleCode.sln`

**Steps:**
1. Build entire solution: `dotnet build System.Numerics.Vectors.SampleCode.sln`.
2. Confirm zero errors for both projects.
3. Run any existing tests if present.

---

## Summary of Package Changes

| Project | Package | Action | New Version |
|---------|---------|--------|-------------|
| Mandelbrot | `System.Numerics.Vectors` 1.1.5-beta | **Remove** — built into .NET runtime | — |
| RayTracer | `System.Numerics.Vectors` 1.1.5-beta | **Remove** — built into .NET runtime | — |
| RayTracer | `System.Drawing.Common` | **Add** — required for GDI+ APIs | 11.0.0-preview.2.26159.112 |
