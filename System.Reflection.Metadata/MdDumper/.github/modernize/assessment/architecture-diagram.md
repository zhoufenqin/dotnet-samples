# Architecture Diagram

MdDumper is a .NET console application that reads PE (Portable Executable) files and visualizes all metadata tables using the System.Reflection.Metadata library.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        CLI["Command Line Interface"]
    end
    subgraph App["Application Layer - .NET Console App"]
        Program["Program Entry Point"]
        Visualizer["MetadataVisualizer"]
    end
    subgraph External["External Libraries"]
        SRM["System.Reflection.Metadata 1.7.0"]
        PE["System.Reflection.PortableExecutable"]
    end
    subgraph Input["Input"]
        PEFile[("PE / Assembly File")]
    end
    subgraph Output["Output"]
        Console[("Console / TextWriter")]
    end

    CLI -->|"file paths"| Program
    Program -->|"opens file"| PEFile
    PEFile -->|"stream"| PE
    PE -->|"MetadataReader"| Visualizer
    Visualizer -->|"uses"| SRM
    Visualizer -->|"writes tables"| Console
```

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        Program["Program"]
    end
    subgraph Business["Business Logic"]
        Visualizer["MetadataVisualizer"]
    end
    subgraph DataAccess["Data Access"]
        PEReader["PEReader"]
        MetadataReader["MetadataReader"]
        MetadataAggregator["MetadataAggregator"]
    end
    subgraph Infra["Infrastructure"]
        FileStream["FileStream"]
        TextWriter["TextWriter"]
    end

    Program -->|"creates"| PEReader
    Program -->|"delegates visualization"| Visualizer
    PEReader -->|"provides"| MetadataReader
    Visualizer -->|"reads metadata"| MetadataReader
    Visualizer -->|"aggregates delta"| MetadataAggregator
    Visualizer -->|"writes output"| TextWriter
    PEReader -->|"reads"| FileStream
```
