
# Investigation Notes – Lab 01: Metasploit Framework Fundamentals

## 1. Investigation Summary

The objective of this investigation was to establish a functional Metasploit Framework environment and understand its basic architecture.

The work covered installation verification, database initialization, module discovery, SSH scanner inspection, and workspace management.

No active target scanning or exploitation was performed.

## 2. Environment Details

| Attribute | Value |
|---|---|
| Operating System | Ubuntu 24.04.5 LTS |
| Platform | WSL |
| Architecture | x86_64 |
| Metasploit Package | 6.5.12~20261123055747~1rapid7-1 |
| Console Version | 6.5.12-dev- |
| Database | PostgreSQL |
| Active Workspace | Metasploit-Labs |

## 3. Installation Verification

Metasploit Framework was installed using the official Rapid7 installation script.

The installation completed successfully.

The framework provided the following commands:

- msfconsole
- msfdb
- msfvenom

The console was launched using:

`msfconsole`

The version was checked using:

`version`

Observed output:

`Framework: 6.5.12-dev-`

`Console : 6.5.12-dev-`

The console displayed the available module inventory, confirming that the framework could load its modules.

## 4. Database Initialization

The initial database initialization did not complete because the framework could not create its temporary directory.

The affected path was:

`/opt/metasploit-framework/embedded/framework/tmp`

Directory inspection showed that the parent framework directory was owned by root and the required temporary directory did not exist.

The missing directory was created with restricted permissions and assigned to the normal user.

The database initialization was then resumed using:

`msfdb --component database init`

Observed output:

```text
Resuming the incomplete private database initialization
Creating initial database schema
Database initialization successful
```

This confirmed successful database schema initialization.

## 5. Module Architecture Exploration

The following module categories were explored:

| Category | Purpose |
|---|---|
| Exploit | Attempts to take advantage of vulnerabilities |
| Auxiliary | Performs scanning, enumeration, and supporting activities |
| Payload | Defines actions or code delivered following successful exploitation |
| Post | Performs supported activities after access has been established |
| Encoder | Transforms payload representations |
| NOP | Provides no-operation instructions for relevant exploit-development contexts |

The framework's module inventory was explored using:

`show exploits`

`show auxiliary`

`show payloads`

`show post`

These commands demonstrated the different capabilities available within Metasploit.

## 6. Auxiliary Scanner Discovery

The following search was executed:

`search type:auxiliary scanner`

The results displayed a range of auxiliary scanner modules covering different services and protocols.

This demonstrated how Metasploit can filter its module inventory using search criteria.

Finding a module in the search results does not establish that the module has been executed or that a target is vulnerable.

## 7. SSH Version Scanner Inspection

The following module was selected:

`auxiliary/scanner/ssh/ssh_version`

Module information was reviewed using:

`info auxiliary/scanner/ssh/ssh_version`

The module description identified its purpose as:

`Detect SSH Version, and the server encryption`

The documentation also contained references relating to SSH protocol security and cryptographic algorithms.

### Module Options

The following configuration was observed:

| Option | Current Setting | Required | Description |
|---|---|---|---|
| EXTENDED_CHECKS | true | Yes | Check for cryptographic issues |
| RHOSTS | Empty | Yes | Target host or hosts |
| RPORT | 22 | Yes | Target SSH port |
| THREADS | 1 | Yes | Concurrent threads |
| TIMEOUT | 30 | Yes | SSH probe timeout |

The RHOSTS field was empty.

This indicated that a target had not been configured.

The module was not executed during this lab.

## 8. Workspace Management

The existing workspaces were initially displayed using:

`workspace`

A dedicated workspace was created:

`workspace -a Metasploit-Labs`

The framework confirmed:

`[*] Added workspace: Metasploit-Labs`

The new workspace was selected:

`workspace Metasploit-Labs`

A subsequent workspace listing showed:

- default
- Metasploit-Labs (active)

This established a separate workspace for future exercises.

## 9. Database Connectivity Verification

The database connection was verified using:

`db_status`

Observed output:

```text
[*] Connected to msf. Connection type: postgresql.
```

This confirmed that the Metasploit console was communicating successfully with PostgreSQL.

## 10. Findings

### Confirmed Findings

1. Metasploit Framework was installed successfully.
2. The console launched successfully.
3. PostgreSQL database initialization completed.
4. Database connectivity was verified.
5. The primary module categories were explored.
6. Auxiliary scanner modules were located through a framework search.
7. SSH version scanner documentation and options were inspected.
8. A dedicated workspace was created and selected.

### Activities Not Performed

- No target host was configured.
- No SSH service was scanned.
- No SSH version was identified.
- No vulnerability was validated.
- No exploit module was executed.
- No payload was delivered.
- No remote session was established.
- No post-exploitation activity occurred.

## 11. Conclusion

The investigation successfully established a functional Metasploit environment and verified its database integration.

The module exploration provided an understanding of the framework's structure and the importance of reviewing module requirements before execution.

The environment is ready for controlled reconnaissance exercises against authorized lab targets.
