# Public LogConsultant

LogConsultant is a Windows lab preview that connects GitHub Copilot CLI to two
local MCP servers:

- **LogsConsultant** analyzes Windows CBS servicing logs.
- **Get Help CBS sidecar** accepts a trusted diagnosis handoff and runs a local
  Diagnose, Resolve, and Verify workflow from `C:\GetHelp\DAF`.

This public repository is a **binary distribution and lab-testing repository**.
It intentionally does not publish the analyzer source, parser/correlation
implementation, internal contracts, or private design documents.

## Supported environment

- x64 Windows or Windows Server
- Administrator access for installation
- Internet access to GitHub releases
- An active GitHub Copilot subscription
- An organization policy that permits Copilot CLI

The bootstrap installs PowerShell 7 and Copilot CLI from their official GitHub
releases when they are missing. Public LogConsultant downloads do not require a
GitHub repository login. Copilot itself still requires GitHub authentication.

## Install

1. Download `Bootstrap-LogConsultant.ps1` from the latest release.
2. Verify that the file came from this repository.
3. Run:

   ```powershell
   Unblock-File .\Bootstrap-LogConsultant.ps1
   powershell.exe -NoLogo -NoProfile -ExecutionPolicy Bypass -File .\Bootstrap-LogConsultant.ps1
   ```

The bootstrap:

1. validates Windows and x64 architecture;
2. installs verified PowerShell 7 and Copilot CLI prerequisites when missing;
3. downloads the latest public LogConsultant ZIP;
4. verifies its SHA-256 checksum;
5. requests elevation;
6. installs both MCP servers and both Copilot skills;
7. installs and secures `C:\GetHelp\DAF`;
8. registers the MCP servers with Copilot;
9. verifies the installed version; and
10. starts Copilot CLI.

If Copilot asks for authentication, enter `/login` and complete the browser
sign-in.

## First safe test

Start with simulation:

```text
Analyze the CBS servicing logs under C:\Windows\Logs\CBS. Trace the current
failure. If a supported current failure is found, prepare the Get Help handoff
and simulate remediation only. Do not execute remediation.
```

For the supported `0x800F0915` repair-content scenario, the expected plan is:

```text
DISM /Online /Cleanup-Image /RestoreHealth
```

Simulation must report:

```text
CommandExecuted=false
```

See [LAB-TESTING.md](LAB-TESTING.md) before enabling real lab execution.

## Important boundary

The included DAF package is a local sidecar package, not a Microsoft-signed
Windows `.diagcab`. It does not execute through the Windows Diagnostic
Execution Service (`diagsvc`). This preview reproduces the Diagnose, Resolve,
and Verify behavior locally while the production Get Help package service is
unavailable.

## Release contents

- `Bootstrap-LogConsultant.ps1`
- `LogConsultant-<version>-win-x64.zip`
- `LogConsultant-<version>-win-x64.zip.sha256`

The installer ZIP contains self-contained MCP executables, installation and
update scripts, Copilot skills, and the runtime DAF package. It does not contain
the .NET analyzer source tree.

## Documentation

- [High-level architecture](ARCHITECTURE.md)
- [Lab testing procedure](LAB-TESTING.md)
- [Security and safety model](SECURITY.md)
- [Preview notice](NOTICE.md)
