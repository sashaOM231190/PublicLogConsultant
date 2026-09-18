# High-level architecture

This document describes the public operational architecture. Internal parsing
algorithms, correlation rules, implementation classes, source code, and private
design contracts are intentionally excluded.

## Component flow

```text
User
  |
  v
GitHub Copilot CLI
  |
  +--> LogsConsultant MCP
  |      |
  |      +--> Reads local CBS servicing evidence
  |      +--> Correlates the current servicing outcome
  |      +--> Produces reports and a short-lived signed handoff
  |
  +--> Get Help CBS sidecar MCP
         |
         +--> Validates the unchanged handoff
         +--> Resolves an approved action from C:\GetHelp\DAF
         +--> Validates the selected package
         +--> Diagnose -> Resolve -> Verify
```

## Installed components

```text
%COPILOT_HOME%
├── mcp-config.json
├── servers
│   ├── logs-consultant
│   └── get-help-side-car
└── skills
    ├── cbs-servicing-analysis
    └── logs-consultant-get-help

C:\GetHelp\DAF
├── catalog.json
├── manifests
└── packages
```

## Trust boundaries

1. CBS evidence remains on the machine during the local workflow.
2. Copilot cannot supply an arbitrary executable, script, command, or package
   path to the remediation sidecar.
3. The handoff is short-lived and integrity-protected.
4. The DAF action is selected through the installed catalog.
5. The selected package is checked before execution.
6. Diagnosis and remediation approval are separate decisions.
7. Remediation requires explicit approval; simulation remains available for a
   safe first test.
8. Long-running DISM execution runs as a sidecar job. Copilot polls the job
   status instead of holding one MCP request open.
9. A new CBS analysis is required after execution; a process exit code alone is
   not servicing proof.

## Current packaged actions

The lab repository contains separate actions for:

```text
0x800F0915 - CBS_E_REPAIR_CONTENT_MISSING
0x800F0912 - CBS_E_ONDEMAND_LOCALSOURCE_NOT_FOUND
```

The `0x800F0915` resolver runs:

```text
DISM /Online /Cleanup-Image /RestoreHealth
```

The `0x800F0912` resolver runs:

```text
DISM /Online /Enable-Feature /FeatureName:NetFx3 /All
```

Other findings can still be analyzed, but unregistered findings do not receive
a local remediation action.

## Production boundary

This release does not modify Get Help, DiagSvc, or Windows servicing binaries.
The local DAF sidecar is replaceable if a future approved package service or
local diagnostic-package interface becomes available.
