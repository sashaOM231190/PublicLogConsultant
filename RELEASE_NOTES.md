# LogConsultant 0.1.7

NetFx3 remediation sequence for `0x800F0912`.

## Included

- Standalone prerequisite and product bootstrap.
- Self-contained LogsConsultant CBS analysis MCP server.
- Self-contained Get Help CBS sidecar MCP server.
- Copilot skills and MCP registration.
- Fixed local DAF repository under `C:\GetHelp\DAF`.
- Separate DAF actions for current `0x800F0915` repair-content and `0x800F0912`
  on-demand local-source findings.
- The `0x800F0912` DAF runs RestoreHealth followed by
  `DISM /Online /Enable-Feature /FeatureName:NetFx3`.
- Final status labels output and exit codes for both DISM steps.
- `run_cbs_remediation` now returns a background job ID immediately, avoiding
  the MCP request timeout while DISM runs.
- `get_cbs_remediation_status` returns the final DISM output, exit code, or
  explicit failure.
- Removed the environment-variable dependency. Explicit approval, Lab mode,
  and elevation remain required.
- Renamed the installed MCP server to `get-help-side-car`.
- Public, anonymous release download and update path.
- CBS analysis routes supported current findings to the local Get Help sidecar
  rather than offering HTTPS knowledge enrichment.
- Approved lab execution prints the DISM response and requires fresh CBS
  analysis to establish whether the original signature remains.

## Safety

- Begin with remediation simulation.
- Real execution is restricted to an elevated, explicitly approved lab mode.
- The current fixed resolver plan is
  `DISM /Online /Cleanup-Image /RestoreHealth`.
- A new CBS analysis is required after execution.

## Boundary

This is a binary-only lab distribution. It does not include the .NET analyzer
source, internal parser/correlation implementation, private contracts, or core
design documents. The sidecar DAF package is not a Windows `.diagcab` and does
not execute through DiagSvc.
