
# Timeline – Lab 01: Metasploit Framework Fundamentals

## 2026-11-23 – Installation and Database Setup

### Installation

- Updated Ubuntu package information.
- Upgraded available packages.
- Verified installation prerequisites.
- Downloaded the official Rapid7 Metasploit installation script.
- Inspected the installer.
- Executed the installation.
- Confirmed successful Metasploit Framework installation.

### Initial Database Setup

- Attempted database initialization using sudo.
- Received an instruction to execute msfdb as a non-root user.
- Repeated initialization under the normal user account.
- Encountered a permission error involving the missing framework temporary directory.
- Inspected directory ownership and permissions.

### Permission Correction

- Confirmed that the framework installation directories were root-owned.
- Identified that the required tmp directory did not exist.
- Created the missing directory with restricted permissions and normal-user ownership.
- Verified the resulting directory permissions.

### Database Recovery

- Checked the database status.
- Confirmed that PostgreSQL was started but initialization was incomplete.
- Executed the recommended database component initialization command.
- Confirmed successful database schema creation.

## 2026-11-24 – Framework Fundamentals

### Console Verification

- Launched msfconsole.
- Completed the initial console setup.
- Reviewed the framework startup output.
- Checked the framework version.

### Module Exploration

- Explored exploit modules.
- Explored auxiliary modules.
- Explored payloads.
- Explored post modules.
- Searched for auxiliary scanner modules.

### SSH Scanner Inspection

- Selected auxiliary/scanner/ssh/ssh_version.
- Reviewed the module description and references.
- Examined the module configuration options.
- Confirmed that RHOSTS was required and remained empty.
- Did not execute the scanner.

### Workspace Management

- Listed the available workspaces.
- Created Metasploit-Labs.
- Selected the new workspace.
- Verified that it was the active workspace.

### Database Verification

- Executed db_status.
- Confirmed successful PostgreSQL connectivity.

## Final Outcome

Lab 01 completed successfully.

Metasploit Framework is operational, database integration is verified, and a dedicated workspace is ready for subsequent exercises.

No target scanning, exploitation, payload execution, or post-exploitation activity was performed.
