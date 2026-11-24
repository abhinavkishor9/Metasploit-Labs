# Lab 01 – Metasploit Framework Fundamentals

## Overview

This lab focuses on installing, configuring, and exploring the Metasploit Framework in an Ubuntu 24.04.5 LTS environment running under Windows Subsystem for Linux (WSL).

The exercise establishes a functional Metasploit environment, verifies PostgreSQL database integration, introduces the framework's module architecture, and demonstrates basic workspace management.

The lab also covers a filesystem permission issue encountered during database initialization and the steps taken to resolve it without reinstalling the framework or changing ownership of the entire installation.

## Environment

| Component | Details |
|---|---|
| Operating System | Ubuntu 24.04.5 LTS |
| Environment | Windows Subsystem for Linux |
| Architecture | x86_64 |
| Metasploit Framework | 6.5.12 |
| Console Version | 6.5.12-dev- |
| Database | PostgreSQL |
| Database Connectivity | Verified |
| Workspace | Metasploit-Labs |

## Objectives

- Install and verify Metasploit Framework.
- Initialize PostgreSQL database integration.
- Understand the main Metasploit module categories.
- Search and explore auxiliary scanner modules.
- Inspect an SSH version detection module.
- Understand module configuration options and target requirements.
- Create and manage a dedicated workspace.
- Document installation and database initialization troubleshooting.

## Activities Performed

### 1. Metasploit Installation

Downloaded and executed the official Rapid7 installation script.

The installation completed successfully, making the following commands available:

- msfconsole
- msfdb
- msfvenom

### 2. Database Initialization

The initial database initialization encountered a filesystem permission error because the required framework temporary directory did not exist.

The missing directory was created with restricted permissions and assigned to the normal user account.

Database initialization was subsequently resumed using the framework's recommended recovery command.

The framework reported successful database schema initialization.

### 3. Framework Verification

Launched the Metasploit console and checked the framework version.

Explored the following module categories:

- Exploit
- Auxiliary
- Payload
- Post
- Encoder
- NOP

### 4. Auxiliary Module Exploration

Searched the framework for auxiliary scanner modules.

Selected the following module for inspection:

`auxiliary/scanner/ssh/ssh_version`

Reviewed its documentation and configuration options.

The module requires a target through RHOSTS and uses TCP port 22 by default.

The scanner was inspected but not executed.

### 5. Workspace Management

Created a dedicated workspace named:

`Metasploit-Labs`

Selected the workspace and verified that it appeared as the active workspace.

### 6. Database Verification

Executed `db_status` from the Metasploit console.

Observed output:

`[*] Connected to msf. Connection type: postgresql.`

This confirmed successful database connectivity.

## Key Findings

- Metasploit Framework was installed and launched successfully.
- PostgreSQL database initialization completed successfully.
- The database connection was verified from the console.
- The framework provides separate module categories for different security testing activities.
- Auxiliary modules support activities such as service discovery and enumeration.
- The SSH scanner requires a configured target before execution.
- Workspaces provide logical separation for assessment data.

## Scope and Limitations

This lab covered framework setup and module exploration only.

No target scanning, vulnerability exploitation, payload execution, remote session creation, or post-exploitation activity was performed.

The SSH version scanner was selected and inspected, but no SSH service was tested.


## Next Steps

The next lab will focus on network reconnaissance and service discovery using Metasploit against an explicitly authorized lab target.

## Safety and Authorization

All practical testing activities are intended for isolated lab environments or systems for which explicit authorization has been obtained.

Testing against public or third-party infrastructure is outside the scope of these exercises.
