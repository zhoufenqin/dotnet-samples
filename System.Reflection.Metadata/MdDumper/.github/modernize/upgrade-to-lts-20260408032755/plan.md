# Upgrade Plan: MdDumper to .NET 10.0

## Overview

Upgrade **MdDumper** from `netcoreapp2.2;net461` to `net10.0` (latest LTS).

| Property | Value |
|----------|-------|
| Source Frameworks | `netcoreapp2.2`, `net461` |
| Target Framework | `net10.0` |
| Projects | 1 (MdDumper) |
| Strategy | All-At-Once |
| Risk | Low |

### Selected Strategy
**All-At-Once** — All projects upgraded simultaneously in a single operation.
**Rationale**: 1 project, straightforward TFM/package bump, no breaking API changes, no code modifications required.

---

## Projects

| Project | Current Framework | Target Framework |
|---------|------------------|-----------------|
| `MdDumper.csproj` | `netcoreapp2.2;net461` | `net10.0` |

---

## Tasks

### Task 001 — Upgrade MdDumper to net10.0

**File**: `System.Reflection.Metadata/MdDumper/MdDumper.csproj`

**Steps**:

1. In `MdDumper.csproj`, replace:
   ```xml
   <TargetFrameworks>netcoreapp2.2;net461</TargetFrameworks>
   ```
   with:
   ```xml
   <TargetFramework>net10.0</TargetFramework>
   ```
   Note: Change the element name from `TargetFrameworks` (plural) to `TargetFramework` (singular) since there is only one target.

2. In `MdDumper.csproj`, remove the entire `PackageReference` for `System.Reflection.Metadata`:
   ```xml
   <PackageReference Include="System.Reflection.Metadata" Version="1.7.0" />
   ```
   This package is included in the net10.0 shared framework and does not require an explicit reference.

3. Restore and build:
   ```
   dotnet restore
   dotnet build
   ```

4. Verify the build succeeds with 0 errors and 0 warnings.

**Expected result**: Project builds successfully targeting net10.0 with no compilation errors.

---

## Success Criteria

- [ ] `MdDumper.csproj` targets `net10.0`
- [ ] `System.Reflection.Metadata` package reference removed
- [ ] `dotnet build` succeeds with 0 errors
