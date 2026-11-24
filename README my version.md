# Metasploit-Labs
## Overview
### 1. Introduction to Metasploit

Metasploit Framework is an open-source penetration testing platform used to identify, assess, and validate security weaknesses in computer systems, applications, and network services.

It provides a collection of modules that support reconnaissance, vulnerability assessment, exploitation, payload handling, and post-exploitation activities.

Security professionals use these modules to conduct structured assessments against authorized systems.

### 2. Metasploit Module Architecture

Metasploit organizes its functionality into different module categories.

#### Exploit Modules

Exploit modules attempt to take advantage of vulnerabilities in target systems or applications.

Depending on the vulnerability and target conditions, successful exploitation may result in unintended code execution or another security impact.

#### Auxiliary Modules

Auxiliary modules support security assessment activities that do not necessarily involve exploitation.

Examples include:

- Service discovery
- Version detection
- Network scanning
- Protocol enumeration
- Information gathering
- Authentication testing

#### Payloads

A payload defines the action or code delivered following successful exploitation.

Depending on the selected payload, this may provide command execution, a shell, or another supported capability.

Selecting a payload does not mean exploitation has succeeded.

#### Post Modules

Post modules operate in an established session or another supported execution context.

They can collect system information, inspect configurations, and perform other post-exploitation activities.

#### Encoders

Encoders transform payload representations into different formats.

Encoding alone does not guarantee evasion of security controls.

#### NOP Modules

NOP modules generate no-operation instructions used in certain exploit-development and payload construction contexts.

### 3. Metasploit Console

The Metasploit console is the primary interactive interface for using the framework.

It supports:

- Searching for modules.
- Reviewing module documentation.
- Selecting modules.
- Configuring options.
- Executing supported operations.
- Managing sessions.
- Interacting with the database.

The main console prompt is:

`msf >`

After selecting a module, the prompt reflects the selected module.

For example:

`msf auxiliary(scanner/ssh/ssh_version) >`

This indicates that the SSH version scanner is selected.

### 4. Module Configuration Options

Modules expose options that determine how they operate.

The SSH version scanner examined in this lab includes:

| Option | Purpose |
|---|---|
| RHOSTS | Specifies the target host or hosts |
| RPORT | Specifies the target SSH port |
| THREADS | Controls concurrent operations |
| TIMEOUT | Defines the probe timeout |
| EXTENDED_CHECKS | Enables additional cryptographic checks |

RHOSTS is required because the scanner needs a target before it can perform an assessment.

The default SSH port is 22, although SSH services can operate on other ports.

Understanding these options before execution helps ensure that the assessment is properly scoped.

### 5. Database Integration

Metasploit supports PostgreSQL database integration.

The database can store assessment information such as:

- Hosts
- Services
- Vulnerabilities
- Credentials
- Loot
- Workspace-specific information

Database integration helps organize assessment results and maintain separation between different exercises.

The `db_status` command checks the console's database connection.

### 6. Workspaces

Workspaces provide logical separation of assessment data.

Separate workspaces can be created for different:

- Lab environments
- Training exercises
- Security assessments
- Investigation activities

This helps prevent information from unrelated assessments from being mixed.

The `workspace` command displays and manages workspaces.

### 7. SSH Version Detection

Secure Shell (SSH) is a protocol used for secure remote administration and communication.

An SSH service provides protocol and implementation information during connection negotiation.

A version detection scanner can identify SSH version information and inspect certain cryptographic characteristics.

The module examined in this lab was:

`auxiliary/scanner/ssh/ssh_version`

Its documented purpose is to detect SSH version information and server encryption characteristics.

The module also provides extended checks related to cryptographic issues.

However, identifying an SSH version or an older cryptographic algorithm does not automatically prove that the service is vulnerable.

Further validation is necessary before making a vulnerability determination.

### 8. Reconnaissance and Exploitation

Reconnaissance and exploitation are separate stages of security testing.

Reconnaissance focuses on discovering hosts, services, versions, and configurations.

Exploitation attempts to use a security weakness to produce an unintended outcome.

An auxiliary scanner can collect useful information without exploiting a target.

This distinction is important when documenting findings and communicating security risk.

### 9. Importance of the Fundamentals

Before performing advanced security testing, a Metasploit user should understand:

- Module categories and their purposes.
- How to search for relevant modules.
- How to inspect module documentation.
- Which configuration options are mandatory.
- How target information is supplied.
- How database integration works.
- How workspaces organize assessment information.
- Why authorization and scope validation are essential.

These concepts provide the foundation for subsequent reconnaissance, vulnerability assessment, and controlled exploitation exercises.

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

## Lab Objectives

### Primary Objective

Establish a functional Metasploit Framework environment and develop an understanding of its console, module architecture, database integration, and workspace management.

### Technical Objectives

1. Verify the installation and availability of Metasploit Framework on Ubuntu 24.04.5 LTS under WSL.

2. Understand the purpose of the primary module categories:
   - Exploit
   - Auxiliary
   - Payload
   - Post
   - Encoder
   - NOP

3. Explore the Metasploit module inventory using console commands.

4. Search for auxiliary scanner modules using module type and functionality filters.

5. Identify the SSH version detection module and examine its documentation.

6. Understand the purpose of the SSH scanner's configuration options, including RHOSTS, RPORT, THREADS, TIMEOUT, and EXTENDED_CHECKS.

7. Recognize why a required target parameter must be configured before executing a scanner.

8. Initialize PostgreSQL database integration and verify connectivity from the Metasploit console.

9. Create a dedicated workspace and confirm that it is selected as the active workspace.

10. Understand how workspaces help organize information across separate assessment activities.

### Troubleshooting Objectives

1. Understand why Metasploit database management commands should run under the normal user account.

2. Investigate filesystem permission errors by examining directory ownership and permissions.

3. Correct a missing temporary directory without changing ownership of the entire framework installation.

4. Resume incomplete database initialization using the recovery command recommended by Metasploit.

5. Distinguish non-blocking Ruby gem warnings from errors that prevent framework functionality.

### Evidence Collection Objectives

- Record actual installation and initialization output.
- Document the framework version.
- Capture database connectivity confirmation.
- Record module search and inspection results.
- Document the SSH scanner configuration.
- Verify workspace creation and selection.
- Maintain a clear distinction between module inspection and module execution.

### Expected Learning Outcome

By completing this exercise, the learner should be able to launch Metasploit, navigate its module inventory, inspect scanner requirements, verify database connectivity, and organize future exercises using dedicated workspaces.

The environment should be ready for the next stage of controlled reconnaissance and service discovery.

## Lab Scenario

### Background

A security analyst is preparing a dedicated penetration testing environment to conduct controlled security assessments against authorized lab systems.

Metasploit Framework has been selected as the primary platform because it provides a structured collection of modules for reconnaissance, vulnerability assessment, exploitation, and post-exploitation activities.

Before assessing any target, the analyst must verify that the framework is correctly installed, its database is operational, and its basic module architecture is understood.

### Scenario

The analyst installs Metasploit Framework on Ubuntu 24.04.5 LTS running under Windows Subsystem for Linux.

During the initial database setup, initialization fails because Metasploit attempts to create a temporary directory inside its root-owned installation path. The required directory does not exist, and the normal user does not have permission to create it.

The analyst investigates the directory ownership and permissions, identifies the missing temporary directory, and applies a narrowly scoped correction without modifying the ownership of the wider framework installation.

Database initialization is subsequently resumed and completes successfully.

With the framework operational, the analyst launches the Metasploit console and begins exploring its module architecture.

The exploit, auxiliary, payload, and post module categories are examined. An auxiliary scanner search is then performed to identify modules available for service discovery and enumeration.

The analyst selects the SSH version detection module for further inspection. Its documentation, references, and configuration options are reviewed to understand how it identifies SSH services and what information is required before execution.

During this review, the analyst observes that RHOSTS is a mandatory option. Since no authorized target has yet been selected, the target field is left unconfigured and the scanner is not executed.

To keep future assessment information organized, the analyst creates a dedicated workspace named Metasploit-Labs and verifies that it is active.

Finally, database connectivity is checked from the console, confirming that the PostgreSQL integration is operational.

### Investigation Scope

The exercise covers:

- Metasploit installation and verification.
- Database initialization and troubleshooting.
- Framework module exploration.
- Auxiliary scanner discovery.
- SSH scanner documentation and option inspection.
- Workspace creation and management.
- Database connectivity verification.

### Out of Scope

The following activities are not part of this exercise:

- Scanning external or unauthorized systems.
- Executing the SSH scanner against a target.
- Exploiting vulnerabilities.
- Delivering payloads.
- Establishing remote sessions.
- Performing post-exploitation activities.

### Expected Outcome

At the end of the exercise, the analyst should have a functional Metasploit installation, a successfully initialized PostgreSQL database, and a dedicated workspace for future security testing.

The analyst should also understand the difference between finding a module, reviewing its requirements, and executing it against a target.

This foundation prepares the environment for the next lab, which will introduce controlled network reconnaissance and service discovery against an explicitly authorized target.

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


