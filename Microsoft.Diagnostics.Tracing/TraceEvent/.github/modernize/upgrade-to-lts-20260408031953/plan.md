# .NET Upgrade Plan: TraceEvent — net45 → net10.0

## Overview

Upgrade the `TraceEvent` project from **.NET Framework 4.5 (net45)** to **.NET 10.0 (LTS)**.

The project uses a legacy (non-SDK-style) project format and must be converted before the target framework can be changed. After conversion, the target framework and incompatible NuGet packages will be updated in a single atomic pass.

**Source framework**: net45  
**Target framework**: net10.0 (LTS)  
**Projects in scope**: 1  
- `TraceEvent\TraceEvent.csproj`

---

## Selected Strategy

**All-At-Once** — All projects upgraded simultaneously in a single operation.  
**Rationale**: 1 project, currently on net45 (legacy .NET Framework). Project requires SDK-style conversion as a prerequisite before TFM change. Work is split by concern into two tasks.

---

## Tasks

### Task 001 — Convert TraceEvent.csproj to SDK-style format

Convert `TraceEvent\TraceEvent.csproj` from the legacy non-SDK project format to a modern SDK-style project.

- Use the `sdk-style-conversion` built-in skill
- Do NOT change the target framework during conversion (keep net45)
- Remove `packages.config` and inline package references into the `.csproj` file
- Verify the project builds after conversion before proceeding

**Skill**: `sdk-style-conversion` (builtin)

---

### Task 002 — Upgrade target framework to net10.0 and update NuGet packages

Upgrade `TraceEvent\TraceEvent.csproj` from net45 to net10.0 and resolve all incompatible NuGet packages.

**Target framework change**:
- Change `<TargetFramework>net45</TargetFramework>` to `<TargetFramework>net10.0</TargetFramework>`

**NuGet package changes** (from assessment):

| Package | Action | Details |
|---------|--------|---------|
| `Microsoft.Diagnostics.Tracing.TraceEvent` | Upgrade | 1.0.21 -> 3.2.1 |
| `Rx-Main` | Keep | 2.1.30204.0 (compatible) |
| `Rx-Core` | Remove | No compatible version for net10.0 |
| `Rx-Interfaces` | Remove | No compatible version for net10.0 |
| `Rx-Linq` | Remove | No compatible version for net10.0 |
| `Rx-PlatformServices` | Remove | No compatible version for net10.0 |

**Steps**:
1. Update `<TargetFramework>` to `net10.0` in `TraceEvent\TraceEvent.csproj`
2. Upgrade `Microsoft.Diagnostics.Tracing.TraceEvent` to version 3.2.1
3. Remove `Rx-Core`, `Rx-Interfaces`, `Rx-Linq`, `Rx-PlatformServices` package references
4. Run `dotnet restore`
5. Build the solution and fix all compilation errors in a single bounded pass
6. Verify: solution builds with 0 errors

---

## Assessment Reference

Full assessment: `assessment.md`

**Issues summary (7 mandatory)**:
- `Project.0001`: Project file not in SDK-style format (1 occurrence)
- `Project.0002`: Target framework must change from net45 to net10.0 (1 occurrence)
- `NuGet.0001`: 5 incompatible NuGet packages (see table above)

---

## Execution Constraints

- SDK-style conversion (Task 001) must complete and build successfully before Task 002 begins
- Task 002 is a single atomic pass: update TFM + packages + fix build errors together
- Do not introduce tier ordering -- there is only one project
- Commit strategy: Single Commit at End
