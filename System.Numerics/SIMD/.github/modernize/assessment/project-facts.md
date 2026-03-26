# Project Facts

## Application Overview

| Property | Value |
|----------|-------|
| Application Name | System.Numerics SIMD Samples |
| Application Type | Desktop (WPF) |
| Language | C# |
| Target Framework | .NET Framework 4.5 |
| Build Tool | MSBuild |
| Output Type | Windows Executable (WinExe) |

## Projects

| Project | Description |
|---------|-------------|
| Mandelbrot | WPF app rendering the Mandelbrot set using scalar and SIMD (System.Numerics.Vectors) implementations |
| RayTracer | WPF app performing 3D ray tracing using SIMD-accelerated vector math |

## Key Dependencies

| Dependency | Version | Purpose |
|------------|---------|---------|
| System.Numerics.Vectors | 1.1.5-beta | SIMD vector acceleration |
| WPF (PresentationCore, PresentationFramework, WindowsBase) | .NET 4.5 | Desktop UI framework |
| System.Drawing | .NET 4.5 | Image/bitmap operations (RayTracer) |

## Assessment Summary

| Metric | Value |
|--------|-------|
| Total Projects | 2 |
| Total Issues | 1 |
| Total Incidents | 2 |
| Total Story Points | 6 |
| Assessment Tool | .NET AppCAT CLI |

## Migration Considerations

| Issue | Severity | Details |
|-------|----------|---------|
| Old .NET Framework dependency (Runtime.0003) | Potential | Both projects target .NET Framework 4.5, which may have compatibility issues on Azure. Upgrade to .NET Framework 4.8 or modern .NET recommended. |

## Runtime Environment

- **Platform**: Windows only (WPF)
- **Architecture**: x64
- **UI Framework**: Windows Presentation Foundation (WPF)
- **Compute Pattern**: SIMD (Single Instruction Multiple Data) via System.Numerics.Vectors
