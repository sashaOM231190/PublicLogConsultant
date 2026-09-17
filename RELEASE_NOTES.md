# LogConsultant 0.1.5

Expanded local Get Help remediation coverage.

## Included

- Standalone prerequisite and product bootstrap.
- Self-contained LogsConsultant CBS analysis MCP server.
- Self-contained Get Help CBS sidecar MCP server.
- Copilot skills and MCP registration.
- Fixed local DAF repository under `C:\GetHelp\DAF`.
- Separate DAF actions for current `0x800F0915` repair-content and `0x800F0912`
  on-demand local-source findings.
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
