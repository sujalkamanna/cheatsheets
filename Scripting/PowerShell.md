# PowerShell Cheatsheet

![PowerShell Logo](https://raw.githubusercontent.com/PowerShell/PowerShell/master/assets/ps_black_128.svg)

---

## Table of Contents
1. [What is PowerShell?](#1-what-is-powershell)
2. [Core Concepts](#2-core-concepts)
3. [Installation & Setup](#3-installation--setup)
4. [Basic Commands](#4-basic-commands)
5. [Variables & Data Types](#5-variables--data-types)
6. [Operators](#6-operators)
7. [Control Structures](#7-control-structures)
8. [Functions & Scripts](#8-functions--scripts)
9. [File Operations](#9-file-operations)
10. [Pipeline & Filtering](#10-pipeline--filtering)
11. [Best Practices](#11-best-practices)
12. [Practical Examples](#12-practical-examples)

---

## 1. What is PowerShell?

PowerShell is a cross-platform task automation and configuration management framework. It's an object-oriented shell and scripting language that allows administrators to automate system tasks, manage cloud infrastructure, and build powerful automation scripts.

**Key Benefits:**
- Object-oriented (not text-based)
- Cross-platform (Windows, Linux, macOS)
- Powerful scripting capabilities
- Full .NET Framework access
- Remoting capabilities
- Cmdlets (lightweight commands)
- Pipeline support
- Built-in help system

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Cmdlet** | Lightweight PowerShell command (verb-noun format) |
| **Pipeline** | Send output from one command as input to another |
| **Object** | Data with properties and methods |
| **Property** | Attribute of an object |
| **Method** | Action that can be performed on object |
| **Alias** | Shorthand for cmdlet name |
| **Module** | Collection of cmdlets and functions |
| **Script** | .ps1 file containing PowerShell code |
| **Variable** | Named storage for data ($var) |
| **Parameter** | Input argument to cmdlet |

---

## 3. Installation & Setup

### Windows Installation

```powershell
# PowerShell 5.1 is built-in on Windows 10/11
Get-PSVersionTable              # Check version

# Install PowerShell 7+ (recommended)
# Option 1: Winget
winget install Microsoft.PowerShell

# Option 2: Chocolatey
choco install powershell-core

# Option 3: Download from
# https://github.com/PowerShell/PowerShell/releases
```

### Linux/macOS Installation

```bash
# Ubuntu
sudo apt-get update
sudo apt-get install -y powershell

# Fedora
sudo yum install powershell

# macOS
brew install powershell

# Or download from GitHub releases
```

### Initial Setup

```powershell
# Check version
$PSVersionTable
pwsh -Version

# Update help
Update-Help -Force

# Check execution policy
Get-ExecutionPolicy

# Set execution policy (allow scripts)
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

# View profile location
$PROFILE

# Create profile
if (!(Test-Path $PROFILE)) {
    New-Item -ItemType File -Path $PROFILE -Force
}

# Edit profile
notepad $PROFILE
```

### Execution Policies

```powershell
# Policies (from least to most restrictive)
Set-ExecutionPolicy -ExecutionPolicy Unrestricted  # Run all
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned  # Signed required for downloads
Set-ExecutionPolicy -ExecutionPolicy AllSigned     # All signed
Set-ExecutionPolicy -ExecutionPolicy Restricted    # No scripts (default)
Set-ExecutionPolicy -ExecutionPolicy Bypass        # Bypass all (temporary)

# Current user vs system-wide
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
```

---

## 4. Basic Commands

### Navigation & Directory Operations

```powershell
# Get current location
pwd
Get-Location

# Change directory
cd C:\Users\Username
Set-Location C:\

# List directory contents
ls
dir
Get-ChildItem
Get-ChildItem -Recurse              # Recursive
Get-ChildItem -Filter *.txt         # With filter
Get-ChildItem -Hidden               # Include hidden

# Create directory
mkdir new_folder
New-Item -ItemType Directory -Name "new_folder"

# Create file
New-Item -ItemType File -Name "file.txt"

# Remove items
Remove-Item file.txt
Remove-Item -Path "folder" -Recurse -Force

# Copy items
Copy-Item source.txt destination.txt
Copy-Item -Path "C:\folder" -Destination "D:\" -Recurse

# Move/Rename
Move-Item old_name.txt new_name.txt
Rename-Item old_name.txt new_name.txt
```

### File Content Operations

```powershell
# Display file content
Get-Content file.txt
cat file.txt
type file.txt

# First/last lines
Get-Content file.txt -Head 10
Get-Content file.txt -Tail 10

# Write to file
"Content" | Out-File file.txt       # Create/overwrite
"Content" | Add-Content file.txt    # Append

# Using here-string
@"
Line 1
Line 2
Line 3
"@ | Set-Content output.txt
```

### System Information

```powershell
# Computer info
Get-ComputerInfo
systeminfo

# OS information
[System.Environment]::OSVersion
Get-CimInstance Win32_OperatingSystem

# Disk space
Get-Volume
Get-Volume | Format-Table
dir C:\ | Measure-Object -Sum

# Network info
Get-NetIPAddress
Get-NetAdapter
ipconfig
```

### Help & Documentation

```powershell
# Get help
Get-Help cmdlet-name
help Get-Process
man Get-Service

# Detailed help
Get-Help Get-Process -Full
Get-Help Get-Process -Detailed
Get-Help Get-Process -Examples

# Search help
Get-Help *Process*
Get-Help *Service*

# Update help
Update-Help -Force

# Get specific parameter help
Get-Help Get-Process -Parameter Name
```

---

## 5. Variables & Data Types

### Variable Declaration & Usage

```powershell
# Create variable ($ prefix)
$name = "John"
$age = 30
$path = "C:\Users\Documents"

# Data types
$string = "Hello"                   # String
$number = 42                        # Integer
$decimal = 3.14                     # Double
$bool = $true                       # Boolean
$array = @(1, 2, 3)               # Array
$hash = @{key="value"}            # Hash table
$null                              # Null

# Variable types
[string]$name = "John"             # Typed
[int]$age = 30

# Get variable type
$var.GetType()
$var.GetType().Name

# Variable scope
$global:var = "value"              # Global
$script:var = "value"              # Script
$local:var = "value"               # Local (default)
```

### Special Variables

```powershell
# Built-in variables
$PSVersionTable                     # PowerShell version info
$PSScriptRoot                       # Script directory
$PSCommandPath                      # Script path
$PSBoundParameters                  # Function parameters
$args                               # Command-line arguments
$?                                  # Last command success ($true/$false)
$LASTEXITCODE                       # Last exit code
$PSItem                             # Current object (in loops)
$_                                  # Current object (alias for $PSItem)
$null                               # Null value

# Environment variables
$env:USERNAME                       # Current user
$env:COMPUTERNAME                  # Computer name
$env:PATH                           # PATH variable
$env:TEMP                           # Temp directory
[System.Environment]::GetEnvironmentVariable("VAR")
```

### Variable Expansion & Manipulation

```powershell
# String interpolation
$name = "World"
"Hello, $name"                      # Interpolated
'Hello, $name'                      # Literal (no expansion)

# String formatting
"Name: {0}, Age: {1}" -f $name, $age
"{0:N2}" -f 3.14159                # Format decimal

# Substring
$str = "PowerShell"
$str.Substring(0, 5)               # "Power"
$str[0..4] -join ''                # "Power" (array indexing)

# String methods
$str.ToUpper()
$str.ToLower()
$str.Replace("Shell", "Core")
$str.Contains("Shell")
$str.StartsWith("Power")
$str.EndsWith("Shell")
$str.Split(" ")                    # Split string
$str.Trim()                        # Remove whitespace
```

---

## 6. Operators

### Arithmetic Operators

```powershell
# Basic operations
$a = 10
$b = 3

$a + $b                             # 13 (addition)
$a - $b                             # 7 (subtraction)
$a * $b                             # 30 (multiplication)
$a / $b                             # 3.33... (division)
$a % $b                             # 1 (modulo)
[Math]::Pow($a, 2)                # 100 (power)

# Increment/Decrement
$x = 5
$x++                                # Post-increment (6)
++$x                                # Pre-increment (6)
$x--                                # Post-decrement (5)
--$x                                # Pre-decrement (4)

# Compound assignment
$x += 5                             # $x = $x + 5
$x -= 3
$x *= 2
$x /= 4
```

### Comparison Operators

```powershell
# Numeric comparison
$a -eq $b                           # Equal
$a -ne $b                           # Not equal
$a -lt $b                           # Less than
$a -le $b                           # Less than or equal
$a -gt $b                           # Greater than
$a -ge $b                           # Greater than or equal

# String comparison
"abc" -eq "abc"                     # Equal
"abc" -ne "abc"                     # Not equal
"abc" -like "a*"                    # Wildcard match
"abc" -notlike "a*"                 # No wildcard match
"abc" -match "^a"                   # Regex match
"abc" -notmatch "^a"                # No regex match

# Containment
@(1, 2, 3) -contains 2              # Array contains value
2 -in @(1, 2, 3)                    # Value in array
"file.txt" -like "*.txt"            # String match
```

### Logical Operators

```powershell
# AND / OR / NOT
$true -and $true                    # AND
$true -or $false                    # OR
-not $false                         # NOT
!$false                             # NOT (shorthand)

# Combining conditions
if ($age -gt 18 -and $verified -eq $true) {
    "Adult and verified"
}

if ($status -eq "Active" -or $status -eq "Pending") {
    "Process"
}
```

---

## 7. Control Structures

### If/Else Statements

```powershell
# Simple if
if ($age -gt 18) {
    Write-Host "Adult"
}

# If-Else
if ($age -lt 13) {
    Write-Host "Child"
} else {
    Write-Host "Not child"
}

# If-ElseIf-Else
if ($score -ge 90) {
    "A"
} elseif ($score -ge 80) {
    "B"
} elseif ($score -ge 70) {
    "C"
} else {
    "F"
}

# Switch statement
switch ($fruit) {
    "apple" {
        Write-Host "Red fruit"
    }
    "banana" {
        Write-Host "Yellow fruit"
    }
    default {
        Write-Host "Unknown fruit"
    }
}
```

### Loops

```powershell
# For loop
for ($i = 0; $i -lt 5; $i++) {
    Write-Host $i
}

# ForEach loop
$array = @("one", "two", "three")
foreach ($item in $array) {
    Write-Host $item
}

# ForEach-Object (pipeline)
$array | ForEach-Object {
    Write-Host $_
}

# While loop
$count = 0
while ($count -lt 5) {
    Write-Host $count
    $count++
}

# Do-While (executes at least once)
do {
    Write-Host $count
    $count++
} while ($count -lt 5)

# Do-Until (opposite of while)
$count = 0
do {
    Write-Host $count
    $count++
} until ($count -ge 5)
```

### Loop Control

```powershell
# Break statement
for ($i = 0; $i -lt 10; $i++) {
    if ($i -eq 5) { break }
    Write-Host $i
}

# Continue statement
for ($i = 0; $i -lt 5; $i++) {
    if ($i -eq 2) { continue }
    Write-Host $i
}

# Return from loop
function test {
    foreach ($item in $array) {
        if ($item -eq "stop") { return }
        Write-Host $item
    }
}
```

---

## 8. Functions & Scripts

### Function Definition

```powershell
# Basic function
function Hello {
    Write-Host "Hello, World!"
}

# Function with parameters
function Greet {
    param([string]$name)
    Write-Host "Hello, $name!"
}

# Multiple parameters
function Add {
    param(
        [int]$a,
        [int]$b
    )
    return $a + $b
}

# Optional parameters with defaults
function Process {
    param(
        [string]$input,
        [string]$format = "json"
    )
    Write-Host "Processing as $format"
}

# Advanced function with help
function Get-FileInfo {
    <#
    .SYNOPSIS
    Get file information
    .DESCRIPTION
    Retrieves detailed file information
    .PARAMETER Path
    File path
    .EXAMPLE
    Get-FileInfo -Path "C:\file.txt"
    #>
    param([string]$Path)
    
    Get-Item $Path | Select-Object Name, Length, LastWriteTime
}
```

### Function Features

```powershell
# Return values
function Double {
    param([int]$num)
    return $num * 2
}
$result = Double -num 5              # $result = 10

# Implicit return (last value)
function Triple {
    param([int]$num)
    $num * 3                         # Returned implicitly
}

# Pipeline output
function Get-Numbers {
    1..5
}
Get-Numbers | ForEach-Object { $_ * 2 }

# Splat parameters
$params = @{
    Name = "John"
    Age = 30
}
MyFunction @params

# Variadic parameters
function Sum {
    param([int[]]$numbers)
    $numbers | Measure-Object -Sum | Select-Object -ExpandProperty Sum
}
Sum -numbers 1, 2, 3, 4, 5          # Returns 15
```

### Script Files

```powershell
# Create script file
New-Item -ItemType File -Name "script.ps1"

# Run script
.\script.ps1
& "C:\path\to\script.ps1"
Invoke-Expression (Get-Content script.ps1)

# Script with parameters
# myscript.ps1
param(
    [string]$name,
    [int]$count = 1
)

Write-Host "Name: $name"
Write-Host "Count: $count"

# Execute with parameters
.\myscript.ps1 -name "John" -count 5
.\myscript.ps1 -name John             # Parameters
.\myscript.ps1 John 3                 # Positional

# Script scope
$script:var = "value"               # Available to script
$global:var = "value"               # Available globally
```

---

## 9. File Operations

### File & Directory Checking

```powershell
# Test path existence
Test-Path "C:\file.txt"             # $true/$false
Test-Path -Path "C:\file.txt" -PathType Leaf    # File
Test-Path -Path "C:\folder" -PathType Container  # Directory

# Get item details
Get-Item "C:\file.txt"
Get-ItemProperty "C:\file.txt"

# File information
$file = Get-Item "C:\file.txt"
$file.Name                          # Filename
$file.FullName                      # Full path
$file.Length                        # File size
$file.LastWriteTime                 # Last modified
$file.Attributes                    # Attributes
```

### File Reading & Writing

```powershell
# Read file content
Get-Content "file.txt"
Get-Content -Path "file.txt" -Encoding UTF8

# Write to file
"Content" | Set-Content "file.txt"  # Overwrite
"Content" | Add-Content "file.txt"  # Append
Out-File -FilePath "file.txt" -InputObject "Content"

# Read line by line
Get-Content "file.txt" | ForEach-Object {
    Write-Host $_
}

# Read as array
[string[]]$lines = Get-Content "file.txt"

# CSV operations
Import-Csv "data.csv"
Export-Csv -InputObject $data -Path "output.csv"

# JSON operations
Get-Content "data.json" | ConvertFrom-Json
$data | ConvertTo-Json | Set-Content "output.json"
```

### File System Operations

```powershell
# Copy files
Copy-Item -Path "source.txt" -Destination "dest.txt"
Copy-Item -Path "C:\folder" -Destination "D:\" -Recurse

# Move files
Move-Item -Path "old.txt" -Destination "new.txt"

# Remove files
Remove-Item -Path "file.txt"
Remove-Item -Path "folder" -Recurse -Force

# Get directory size
Get-ChildItem -Path "C:\folder" -Recurse | Measure-Object -Property Length -Sum

# List files with filter
Get-ChildItem -Path "C:\folder" -Filter "*.txt"
Get-ChildItem -Path "C:\folder" -Include "*.txt", "*.csv"
Get-ChildItem -Path "C:\folder" -Exclude "*.tmp"
```

---

## 10. Pipeline & Filtering

### Pipeline Basics

```powershell
# Pass output as input
Get-Process | Stop-Process
Get-ChildItem | Select-Object Name, Length
Get-Service | Where-Object { $_.Status -eq "Running" }

# Pipeline continuation
Get-Process |
    Where-Object { $_.Handles -gt 1000 } |
    Sort-Object -Property Handles -Descending |
    Select-Object Name, Handles

# Select-Object (choose properties)
Get-Process | Select-Object Name, Id, Handles
Get-Process | Select-Object -First 5
Get-Process | Select-Object -Last 5

# Where-Object (filter)
Get-Process | Where-Object { $_.Memory -gt 100MB }
Get-Service | Where-Object { $_.Status -eq "Stopped" }
Get-ChildItem | Where-Object { $_.Extension -eq ".txt" }

# Sort-Object
Get-Process | Sort-Object -Property Handles -Descending
Get-ChildItem | Sort-Object -Property LastWriteTime

# Group-Object
Get-Process | Group-Object -Property Name
Get-Service | Group-Object -Property Status
```

### Data Manipulation

```powershell
# Measure-Object
Get-ChildItem | Measure-Object -Sum -Property Length
Get-Process | Measure-Object                   # Count

# Format-Table
Get-Process | Format-Table -AutoSize
Get-Process | Format-Table Name, Id, Memory

# Format-List
Get-Process | Format-List
Get-Service | Format-List Name, Status, DisplayName

# Convert-ToHtml
Get-Process | Convert-ToHtml | Out-File report.html

# Out-Null (suppress output)
Remove-Item file.txt | Out-Null

# Tee-Object (view and pass through)
Get-Process | Tee-Object -Variable procs | Format-Table
```

---

## 11. Best Practices

### Code Style

```powershell
# Use meaningful names
$userName = "John"                  # Good
$un = "John"                        # Bad

# Use PascalCase for functions
function Get-UserName { }
function Set-ServiceStatus { }

# Use camelCase for variables
$userName = "John"
$errorCount = 0

# Quote strings
Write-Host "Message"                # Good
Write-Host Message                  # Only if no spaces

# Use full cmdlet names in scripts
Get-Process                         # Good in scripts
gps                                 # Alias (use in console)

# Comments
# Single line comment
<#
Multi-line comment
Useful for documentation
#>

# Proper indentation
if ($condition) {
    Write-Host "Indented"
}
```

### Error Handling

```powershell
# Try-Catch-Finally
try {
    # Code that might fail
    Get-Item "nonexistent.txt" -ErrorAction Stop
}
catch {
    Write-Host "Error: $_"
}
finally {
    Write-Host "Cleanup"
}

# Error action preference
Get-Item "file.txt" -ErrorAction Stop       # Throw error
Get-Item "file.txt" -ErrorAction Continue   # Continue
Get-Item "file.txt" -ErrorAction SilentlyContinue  # Ignore

# Check command success
if ($?) {
    Write-Host "Success"
} else {
    Write-Host "Failed"
}

# Trap errors globally
trap {
    Write-Host "Error trapped: $_"
    continue
}
```

### Performance Tips

```powershell
# Use specific filters
Get-Process -Name "notepad"         # Better
Get-Process | Where-Object { $_.Name -eq "notepad" }  # Slower

# Use -Filter parameter when available
Get-ChildItem -Filter "*.txt"       # Better
Get-ChildItem | Where-Object { $_ -like "*.txt" }    # Slower

# Avoid loops when possible
# Bad
$total = 0
foreach ($item in $array) {
    $total += $item.Value
}

# Good
$total = ($array | Measure-Object -Property Value -Sum).Sum

# Use Select-Object -First to limit results
Get-Process | Select-Object -First 10  # More efficient
```

---

## 12. Practical Examples

### System Administration Script

```powershell
# Get system information
function Get-SystemInfo {
    $os = Get-CimInstance Win32_OperatingSystem
    $comp = Get-CimInstance Win32_ComputerSystem
    $memory = Get-CimInstance Win32_PhysicalMemory
    
    [PSCustomObject]@{
        ComputerName = $comp.Name
        OS = $os.Caption
        Kernel = $os.Version
        Memory = "$([math]::Round($memory.Capacity / 1GB)) GB"
        LastBoot = $os.LastBootUpTime
    }
}

Get-SystemInfo | Format-Table
```

### File Backup Script

```powershell
function Backup-Files {
    param(
        [string]$source,
        [string]$destination
    )
    
    if (-not (Test-Path $destination)) {
        New-Item -ItemType Directory -Path $destination | Out-Null
    }
    
    $timestamp = Get-Date -Format "yyyyMMdd_HHmmss"
    $backupPath = Join-Path $destination "backup_$timestamp"
    
    Copy-Item -Path $source -Destination $backupPath -Recurse -Force
    Write-Host "Backup completed: $backupPath"
}

# Usage
Backup-Files -source "C:\Documents" -destination "D:\Backups"
```

### Service Management Script

```powershell
function Manage-Service {
    param(
        [string]$serviceName,
        [ValidateSet("Start", "Stop", "Restart")]
        [string]$action
    )
    
    $service = Get-Service -Name $serviceName -ErrorAction SilentlyContinue
    
    if ($null -eq $service) {
        Write-Error "Service not found: $serviceName"
        return
    }
    
    try {
        switch ($action) {
            "Start" { Start-Service $serviceName }
            "Stop" { Stop-Service $serviceName }
            "Restart" { Restart-Service $serviceName }
        }
        Write-Host "Service $serviceName $action successfully"
    }
    catch {
        Write-Error "Failed to $action service: $_"
    }
}

# Usage
Manage-Service -serviceName "W3SVC" -action "Restart"
```

---

## Quick Reference

```powershell
# Navigation
pwd, cd, ls, dir, mkdir, rmdir

# File operations
Get-Content, Set-Content, Add-Content, Copy-Item, Move-Item, Remove-Item

# Process management
Get-Process, Start-Process, Stop-Process, Get-Service, Start-Service, Stop-Service

# Variables & types
$var, [type]$var, Get-Variable, Set-Variable, Remove-Variable

# Conditionals
if, else, elseif, switch

# Loops
for, foreach, while, do-while, do-until

# Functions
function, param, return

# Pipeline & filtering
|, Select-Object, Where-Object, Sort-Object, Group-Object, Measure-Object

# Help
Get-Help, Update-Help, help, man

# Arrays & Collections
@(), @{}, [array], [hashtable]

# Formatting & Output
Write-Host, Write-Output, Format-Table, Format-List, Out-File
```

---

**Last Updated:** 2026
**PowerShell Version:** 7.x+
**License:** MIT

For detailed help: `Get-Help`, `Get-Help cmdlet -Full`, `Update-Help`

---

**Tips:**
- Use Get-Help for everything
- Prefer full cmdlet names in scripts
- Use aliases only in console
- Implement error handling with try-catch
- Comment code thoroughly
- Test scripts in development
- Use Write-Host for display
- Use Write-Output for pipeline data