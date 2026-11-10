# AI Agent Execution Policy

## Runtime

This repository uses Python 3 as the primary automation runtime on Windows.

The agent must prefer Python over PowerShell whenever Python can perform
the requested operation safely and reliably.

## Tool Priority

1. Python Standard Library
2. Existing project dependencies
3. Approved Python packages
4. Native application CLI
5. PowerShell as a last resort

## Python Execution

Check the interpreter before execution:

```text
py -3 --version
python --version
```

Preferred command:

```text
py -3 script.py
```

Fallback command:

```text
python script.py
```

## Python Development Rules

- Use `pathlib.Path` for Windows-compatible paths.
- Use `subprocess.run()` with arguments supplied as a list.
- Do not use `shell=True` without explicit user approval.
- Avoid complex `python -c` commands.
- Store multi-step logic in a `.py` file.
- Validate return codes and handle exceptions.
- Use timeouts when invoking external programs.
- Do not hardcode secrets.
- Do not overwrite existing files unless explicitly authorized.
- Write generated output to a new file by default.

## PowerShell Restrictions

PowerShell may be used only when Python cannot directly support required
Windows-specific functionality, including:

- Windows services
- Windows Registry
- NTFS ACLs
- Active Directory
- Group Policy
- Windows Event Log
- Windows Optional Features
- Machine-level environment variables

Before running PowerShell, the agent must:

1. Explain why Python is insufficient.
2. Show the exact command.
3. Explain the impact.
4. Provide a rollback method.
5. Request explicit approval.

## Safety Policy

The agent must begin with inspection and validation.

The agent must not perform any of the following without explicit approval:

- Delete files or directories
- Overwrite existing files
- Stop or restart services
- Restart or shut down Windows
- Change firewall rules
- Change network settings
- Modify Registry values
- Change system-wide environment variables
- Install software globally
- Modify production infrastructure
- Execute operations that may cause downtime

If there is uncertainty, stop and ask the user before making changes.
Keeping the system available takes priority over completing the task.

## Execution Report

Before running an operation, report:

- Execution method
- Target files or systems
- Expected changes
- Risk level
- Rollback plan

After running an operation, report:

- Exit code
- Completed changes
- Modified or created files
- Errors or warnings
- Recommended next action
