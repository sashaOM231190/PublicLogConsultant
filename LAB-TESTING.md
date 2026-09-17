# Lab testing

Use an isolated Windows lab or disposable test server. Start with simulation.

## 1. Install

Download `Bootstrap-LogConsultant.ps1` from the latest public release:

```powershell
Unblock-File .\Bootstrap-LogConsultant.ps1
powershell.exe -NoLogo -NoProfile -ExecutionPolicy Bypass -File .\Bootstrap-LogConsultant.ps1
```

Approve the elevation request. If Copilot starts without an authenticated
session, enter `/login`.

## 2. Verify installation

```powershell
$copilotHome = if ($env:COPILOT_HOME) {
    $env:COPILOT_HOME
} else {
    Join-Path $env:USERPROFILE '.copilot'
}

Get-Content "$copilotHome\servers\logs-consultant\VERSION"
Get-Content "$copilotHome\servers\get-help-cbs\VERSION"
Test-Path "$copilotHome\servers\logs-consultant\LogsConsultant.Mcp.exe"
Test-Path "$copilotHome\servers\get-help-cbs\GetHelpCmd.LogsConsultant.exe"
Get-ChildItem C:\GetHelp\DAF -Recurse
```

Both executable checks should return `True`. The version files should match the
release version.

## 3. Analyze CBS logs

Use the live CBS directory:

```text
Analyze the CBS servicing logs under C:\Windows\Logs\CBS. Identify and trace
only the current servicing failure.
```

LogsConsultant should report the latest substantive servicing outcome rather
than presenting an older resolved event as current.

## 4. Test remediation simulation

```text
For the current supported finding, prepare the Get Help handoff, assess it with
fDetectionNeeded=false, and simulate remediation only. Do not execute it.
```

For a current supported `0x800F0915` finding, verify:

- the handoff is accepted without repeating machine detection;
- the proposed command is
  `DISM /Online /Cleanup-Image /RestoreHealth`;
- `CommandExecuted=false`;
- no DISM process is launched; and
- the result requests a fresh LogsConsultant analysis for verification.

If the current machine does not contain the supported failure, analysis should
still work, but no compatible remediation package should be offered.

In particular, a current `0x800F0912`
(`CBS_E_ONDEMAND_LOCALSOURCE_NOT_FOUND`) result is analysis-only in this
release. The expected response is that no reviewed local DAF action is
registered; HTTPS enrichment must not be offered.

## 5. Optional real lab execution

Real execution is for an isolated reproduction lab only. Start Copilot from an
elevated PowerShell session:

```powershell
$env:LOGS_CONSULTANT_ENABLE_LAB_REMEDIATION = '1'
copilot
```

Then:

1. perform a new CBS analysis;
2. trace the current supported finding;
3. prepare and assess the handoff;
4. review the fixed command;
5. explicitly approve remediation; and
6. request lab execution rather than simulation.

All gates must be satisfied:

- elevated sidecar process;
- current supported finding;
- unchanged and unexpired handoff;
- explicit user approval;
- lab execution mode; and
- `LOGS_CONSULTANT_ENABLE_LAB_REMEDIATION=1`.

After execution, request a completely new CBS analysis. Do not treat DISM exit
code `0` as sufficient evidence of recovery.

## 6. Provide feedback

Open a GitHub issue using the lab feedback template. Do not attach customer
logs, machine secrets, access tokens, or other sensitive information. Include
only redacted symptoms, observed behavior, release version, and whether the run
was simulation or lab execution.
