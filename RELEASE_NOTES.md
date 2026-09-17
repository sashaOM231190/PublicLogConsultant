# LogConsultant 0.1.3

Initial public lab preview.

## Included

- Standalone prerequisite and product bootstrap.
- Self-contained LogsConsultant CBS analysis MCP server.
- Self-contained Get Help CBS sidecar MCP server.
- Copilot skills and MCP registration.
- Fixed local DAF repository under `C:\GetHelp\DAF`.
- Simulation-first CBS repair-content workflow.
- Public, anonymous release download and update path.

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
