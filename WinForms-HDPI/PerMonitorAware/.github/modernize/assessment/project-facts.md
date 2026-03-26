# Project Facts

## Application Overview

| Property | Value |
|----------|-------|
| **Application Name** | PerMonitorDemo |
| **Language** | C# |
| **Framework** | .NET Framework 4.7 |
| **Application Type** | Windows Forms (WinExe) |
| **Build Tool** | MSBuild |

## Assessment Summary

| Metric | Value |
|--------|-------|
| **Total Projects** | 1 |
| **Total Issues** | 1 |
| **Total Incidents** | 1 |
| **Total Story Points** | 3 |
| **Assessment Tool** | .NET AppCAT CLI |
| **Assessment Date** | 2026-03-26 |

## Issue Severity Breakdown

| Severity | Count |
|----------|-------|
| Mandatory | 0 |
| Optional | 1 |
| Potential | 0 |
| Information | 0 |

## Issue Details

### Runtime.0002 — Upgrade to newer target framework

**Severity:** Optional  
**Effort:** 3 story points  
**Location:** PerMonitorDemo.csproj  

The application targets .NET Framework 4.7, which may be missing features needed for cloud scenarios such as scaling or accessing secrets from secure external sources. It is recommended to:

1. Validate the application runs correctly on .NET Framework 4.8 (no retargeting required, just validation).
2. In the long term, upgrade to .NET 8+ using the .NET Upgrade Assistant for improved performance and cloud readiness.

**Severity per Azure target:**

| Target | Severity |
|--------|----------|
| Azure App Service (Windows) | Optional |
| Azure App Service (Linux) | Mandatory |
| Azure Kubernetes Service (Linux) | Mandatory |
| Azure Kubernetes Service (Windows) | Information |
| Azure Container Apps | Mandatory |
| Azure App Service Container (Linux) | Mandatory |
| Azure App Service Container (Windows) | Information |
| Azure App Service Managed Instance (Windows) | Optional |

## Cloud Readiness

| Azure Target | Readiness |
|-------------|-----------|
| Azure App Service (Windows) | Mostly ready (1 optional issue) |
| Azure App Service (Linux) | Needs attention (WinForms is Windows-only) |
| Azure Kubernetes Service (Linux) | Needs attention (WinForms is Windows-only) |
| Azure Kubernetes Service (Windows) | Mostly ready |
| Azure Container Apps | Needs attention (WinForms is Windows-only) |

## Dependencies

The project uses only .NET Framework built-in assemblies — no external NuGet packages:

- System
- System.Configuration
- System.Core
- System.Data
- System.Data.DataSetExtensions
- System.Deployment
- System.Drawing
- System.Net.Http
- System.Windows.Forms
- System.Xml
- System.Xml.Linq
- Microsoft.CSharp
