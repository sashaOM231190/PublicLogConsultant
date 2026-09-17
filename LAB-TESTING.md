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
Get-Content "$copilotHome\servers\get-help-side-car\VERSION"
Test-Path "$copilotHome\servers\logs-consultant\LogsConsultant.Mcp.exe"
Test-Path "$copilotHome\servers\get-help-side-car\GetHelpCmd.LogsConsultant.exe"
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

For a current supported `0x800F0915` or `0x800F0912` finding, verify:

- the handoff is accepted without repeating machine detection;
- the `0x800F0915` proposed command is
  `DISM /Online /Cleanup-Image /RestoreHealth`;
- the `0x800F0912` proposed sequence adds
  `DISM /Online /Enable-Feature /FeatureName:NetFx3 /All` after RestoreHealth;
- `CommandExecuted=false`;
- no DISM process is launched; and
- the result requests a fresh LogsConsultant analysis for verification.

The two HRESULTs must resolve to separate action IDs and packages. If the
current machine contains another failure, analysis should still work, but no
compatible remediation package should be offered. HTTPS enrichment must not
be offered.

## 5. Optional real lab execution

Real execution is for an isolated reproduction lab only. Start Copilot from an
elevated PowerShell session:

```powershell
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
- lab execution mode.

`run_cbs_remediation` should return a job ID without waiting for DISM.
Copilot must call `get_cbs_remediation_status` until the job completes or
fails. Do not retry `run_cbs_remediation` with the consumed plan.

After completion, verify that Copilot prints labeled output and exit codes for
each DISM operation, then performs a completely new CBS analysis. Do
not treat DISM exit code `0` as sufficient evidence of recovery. A
`0x800F0912` NetFX3 failure may remain if the required Features-on-Demand source
is still unavailable; report that result rather than claiming success.

The workflow should display six numbered phases with amber running markers,
green completed markers, an orange consent marker, and red failure markers.

## 6. Provide feedback

Open a GitHub issue using the lab feedback template. Do not attach customer
logs, machine secrets, access tokens, or other sensitive information. Include
only redacted symptoms, observed behavior, release version, and whether the run
was simulation or lab execution.
