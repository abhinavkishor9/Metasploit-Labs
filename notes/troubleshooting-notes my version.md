# Troubleshooting Notes – Lab 01: Metasploit Framework Fundamentals

## 1. Overview

This document records the issues encountered during Metasploit installation and database initialization on Ubuntu 24.04.5 LTS under WSL.

The issues were investigated and resolved without reinstalling Metasploit or applying broad filesystem permission changes.

## 2. Issue: Running msfdb as Root

### Command

`sudo msfdb init`

### Observed Error

```text
Please run msfdb as a non-root user
```

### Cause

Metasploit database management is intended to run under the normal user account.

Running it as root can create ownership inconsistencies in user-specific configuration and database files.

### Resolution

The command was executed without sudo:

`msfdb init`

This allowed the initialization process to continue under the normal user account.

## 3. Issue: Permission Denied During Database Initialization

### Command

`msfdb init`

### Observed Error

```text
Errno::EACCES: Permission denied @ dir_s_mkdir - /opt/metasploit-framework/embedded/framework/tmp
```

### Investigation

The following commands were used to inspect the relevant directories:

```bash
ls -ld /opt/metasploit-framework
ls -ld /opt/metasploit-framework/embedded
ls -ld /opt/metasploit-framework/embedded/framework
ls -ld /opt/metasploit-framework/embedded/framework/tmp
```

Observed directory ownership:

| Directory | Owner | Group |
|---|---|---|
| /opt/metasploit-framework | root | root |
| /opt/metasploit-framework/embedded | root | root |
| /opt/metasploit-framework/embedded/framework | root | root |
| /opt/metasploit-framework/embedded/framework/tmp | Missing | Missing |

### Cause

The framework attempted to create a temporary directory under a root-owned installation path.

The normal user did not have permission to create the missing directory inside the root-owned parent directory.

### Resolution

Only the missing temporary directory was created:

```bash
sudo install -d -o abhinav -g abhinav -m 0750 /opt/metasploit-framework/embedded/framework/tmp
```

The directory was verified using:

```bash
ls -ld /opt/metasploit-framework/embedded/framework/tmp
```

Observed result:

```text
drwxr-x--- 2 abhinav abhinav ... /opt/metasploit-framework/embedded/framework/tmp
```

This corrected the specific directory permission issue.

The ownership of the wider Metasploit installation was left unchanged.

## 4. Issue: Incomplete Database Initialization

### Command

`msfdb status`

### Observed Output

```text
Database initialization is incomplete; run msfdb --component database init to resume it.
Database started
```

### Cause

The previous initialization attempt had stopped during schema creation because of the filesystem permission error.

PostgreSQL was running, but the database initialization had not completed.

### Resolution

The recovery command recommended by Metasploit was executed:

```bash
msfdb --component database init
```

Observed output:

```text
Resuming the incomplete private database initialization
Creating initial database schema
Database initialization successful
```

Database initialization completed successfully.

## 5. Ruby Gem Warnings

### Observed Warnings

Metasploit commands displayed warnings about ambiguous Ruby gem specifications.

The affected gems were:

| Gem | Installed Versions |
|---|---|
| base64 | 0.3.0 and 0.2.0 |
| logger | 1.7.0 and 1.6.4 |

The warning suggested running:

`gem cleanup`

### Assessment

The warnings did not prevent the following operations from succeeding:

- Database initialization
- Database status checks
- Metasploit console launch
- Module exploration
- PostgreSQL connectivity verification

### Action Taken

No gem cleanup or manual gem version modification was performed.

The warnings were documented and left unchanged because there was no evidence that they were causing a functional failure.

## 6. Database Connectivity Verification

### Command

From the Metasploit console:

`db_status`

### Observed Output

```text
[*] Connected to msf. Connection type: postgresql.
```

### Result

Successful PostgreSQL connectivity was confirmed.

## 7. Startup Banner Observation

### Observation

Metasploit displayed a large startup banner containing team names and other text.

### Assessment

This was normal framework startup output.

It was not an installation or database error.

No corrective action was required.

