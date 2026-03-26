# Dependency Map

The SIMD solution contains two WPF desktop projects (Mandelbrot and RayTracer) targeting .NET Framework 4.5, with a total of 4 external/framework dependencies each relying on System.Numerics.Vectors for SIMD-accelerated computation.

## Dependencies

```mermaid
flowchart LR
    Mandelbrot["Mandelbrot App"]
    RayTracer["RayTracer App"]

    subgraph SIMD["SIMD / Numerics"]
        Vectors["System.Numerics.Vectors v1.1.5-beta"]
    end
    subgraph UI["UI Framework (WPF)"]
        WPF["WPF (WindowsBase + PresentationCore + PresentationFramework)"]
        Xaml["System.Xaml 4.0"]
    end
    subgraph Runtime[".NET Framework Runtime"]
        NetFx["System / System.Core / System.Data (.NET 4.5)"]
        Drawing["System.Drawing"]
    end

    Mandelbrot -->|"vector math"| SIMD
    Mandelbrot -->|"desktop UI"| UI
    Mandelbrot -->|"runtime"| Runtime
    RayTracer -->|"vector math"| SIMD
    RayTracer -->|"desktop UI"| UI
    RayTracer -->|"runtime"| Runtime
```
