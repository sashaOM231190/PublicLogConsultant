# LogConsultant 0.1.4

Local Get Help workflow correction.

## Included

- Standalone prerequisite and product bootstrap.
- Self-contained LogsConsultant CBS analysis MCP server.
- Self-contained Get Help CBS sidecar MCP server.
- Copilot skills and MCP registration.
- Fixed local DAF repository under `C:\GetHelp\DAF`.
- Simulation-first CBS repair-content workflow.
- Public, anonymous release download and update path.
- CBS analysis routes supported current findings to the local Get Help sidecar
  rather than offering HTTPS knowledge enrichment.
- Current `0x800F0912` findings remain analysis-only because no reviewed local
  DAF action is registered for that scenario.

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
