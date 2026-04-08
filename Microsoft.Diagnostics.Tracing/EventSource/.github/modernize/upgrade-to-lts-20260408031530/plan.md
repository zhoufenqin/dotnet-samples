# .NET Upgrade Plan: net45 → net10.0

## Solution
**File**: `EventSource.sln`
**Source framework**: .NET Framework 4.5 (`net45`)
**Target framework**: .NET 10.0 LTS (`net10.0`)

## Projects
| Project | Current TFM | Target TFM |
|---------|-------------|------------|
| `EventSource\EventSource.csproj` | net45 | net10.0 |

## Assessment Summary
- 1 project, 4 mandatory issues, 0 security vulnerabilities
- Project requires SDK-style conversion (currently legacy format)
- 2 NuGet packages must be removed (incompatible or now included in .NET runtime)
- 1 NuGet package remains compatible

### Selected Strategy
**All-At-Once** — All projects upgraded simultaneously in a single operation.
**Rationale**: 1 project on .NET Framework 4.5, clear upgrade path, no breaking API changes detected.

## Package Changes for `EventSource\EventSource.csproj`
| Package | Current Version | Action | Reason |
|---------|----------------|--------|--------|
| `Microsoft.Diagnostics.Tracing.EventSource` | 1.0.26 | **Remove** | Incompatible with net10.0; functionality is built into .NET runtime |
| `Microsoft.Diagnostics.Tracing.EventSource.Redist` | 1.0.26 | **Remove** | Functionality included with .NET framework reference |
| `Microsoft.Diagnostics.Tracing.EventRegister` | 1.0.26 | **Keep** | Compatible with net10.0 |

## Tasks

### Task 01: Upgrade EventSource project to net10.0

**Scope**: `EventSource\EventSource.csproj` (12 source files)

**Steps**:
1. **Convert to SDK-style**: Run SDK-style conversion on `EventSource\EventSource.csproj` — remove legacy project format, MSBuild targets, and `packages.config` if present. Do NOT change target framework during this step.
2. **Update TargetFramework**: Change `<TargetFramework>net45</TargetFramework>` to `<TargetFramework>net10.0</TargetFramework>` in `EventSource\EventSource.csproj`.
3. **Remove incompatible packages**: Remove `Microsoft.Diagnostics.Tracing.EventSource` (1.0.26) and `Microsoft.Diagnostics.Tracing.EventSource.Redist` (1.0.26) from `EventSource\EventSource.csproj`.
4. **Update source code**: Replace any `using Microsoft.Diagnostics.Tracing;` references to `System.Diagnostics.Tracing` namespace since `EventSource` is now in the .NET runtime. Verify all 12 files compile correctly.
5. **Restore and build**: Run `dotnet restore` then `dotnet build` on `EventSource.sln`. Fix all compilation errors in a single pass.
6. **Run tests**: Run `dotnet test` on the solution to validate correctness.

**Validation**: Solution builds with 0 errors; all tests pass.
