# Architecture Diagram

Two WPF desktop applications targeting .NET Framework 4.5 that demonstrate SIMD (Single Instruction, Multiple Data) acceleration using System.Numerics.Vectors for compute-intensive graphics workloads.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Desktop Layer - WPF (.NET Framework 4.5)"]
        MandelbrotUI["Mandelbrot WPF App"]
        RayTracerUI["RayTracer WPF App"]
    end
    subgraph Compute["Computation Layer"]
        subgraph MandelbrotComp["Mandelbrot Computation"]
            ScalarFloat["ScalarFloat Renderer"]
            ScalarDouble["ScalarDouble Renderer"]
            VectorFloat["VectorFloat Renderer"]
            VectorDouble["VectorDouble Renderer"]
        end
        subgraph RayTracerComp["RayTracer Computation"]
            Scene["Scene Manager"]
            Camera["Camera"]
            Objects["Scene Objects"]
            Materials["Materials"]
        end
    end
    subgraph SIMD["SIMD Acceleration"]
        Vectors["System.Numerics.Vectors 1.1.5"]
    end

    MandelbrotUI -->|"triggers render"| MandelbrotComp
    RayTracerUI -->|"triggers render"| RayTracerComp
    MandelbrotComp -->|"vector math"| Vectors
    RayTracerComp -->|"vector math"| Vectors
```

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        MWin["MainWindow"]
        FlyThru["FlyThru"]
        RTWin["RayTracer MainWindow"]
    end
    subgraph Business["Business Logic"]
        Abstractions["Abstractions"]
        Interfaces["IMandelbrotCalculator"]
        ScalarF["ScalarFloat"]
        ScalarD["ScalarDouble"]
        VecF["VectorFloat"]
        VecD["VectorDouble"]
        SceneObj["Scene"]
        CameraComp["Camera"]
        RayComp["Ray"]
        LightComp["Light"]
    end
    subgraph DataAccess["Scene Objects"]
        Sphere["Sphere"]
        Disc["Disc"]
        Plane["InfinitePlane"]
        Quad["Quad"]
    end
    subgraph Infra["Materials"]
        MatBase["Material"]
        Solid["SolidMaterial"]
        Checker["CheckerboardMaterial"]
    end

    MWin -->|"uses"| Interfaces
    FlyThru -->|"uses"| Interfaces
    Interfaces -->|"implemented by"| ScalarF
    Interfaces -->|"implemented by"| ScalarD
    Interfaces -->|"implemented by"| VecF
    Interfaces -->|"implemented by"| VecD
    VecF -->|"extends"| Abstractions
    VecD -->|"extends"| Abstractions
    RTWin -->|"renders"| SceneObj
    SceneObj -->|"uses"| CameraComp
    SceneObj -->|"uses"| LightComp
    SceneObj -->|"contains"| Sphere
    SceneObj -->|"contains"| Disc
    SceneObj -->|"contains"| Plane
    SceneObj -->|"contains"| Quad
    Sphere -->|"uses"| MatBase
    Disc -->|"uses"| MatBase
    MatBase -->|"extended by"| Solid
    MatBase -->|"extended by"| Checker
    RayComp -.->|"intersection"| Sphere
    RayComp -.->|"intersection"| Disc
```
