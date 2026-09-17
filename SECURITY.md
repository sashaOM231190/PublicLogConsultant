# Security and safety

LogConsultant is designed for controlled lab evaluation of Windows servicing
diagnosis and a narrowly scoped local remediation workflow.

## Safety properties

- CBS evidence is analyzed locally.
- Raw CBS text is not placed in the remediation handoff.
- Handoffs are short-lived and integrity-protected.
- The remediation sidecar accepts only registered action identifiers.
- Packages are validated before extraction and execution.
- Archive size, entry count, expanded size, and path traversal are constrained.
- Script and executable paths are fixed by the installed package.
- Copilot cannot provide arbitrary remediation commands or arguments.
- Remediation consent is separate from the diagnosis decision.
- Simulation is the default.
- Real execution requires elevation, explicit approval, lab mode, and an
  environment gate.
- Remediation plans cannot be reused.
- `C:\GetHelp\DAF` is installed read/execute-only for normal authenticated
  users; Administrators and SYSTEM retain control.

## Limitations

- This is not a production Microsoft Get Help or DiagSvc integration.
- The DAF package is not a Windows `.diagcab`.
- The currently packaged action is limited to the reviewed CBS
  repair-content scenario.
- Lab execution can modify Windows servicing state.
- A successful process exit code does not prove servicing recovery.

## Reporting a security concern

Do not publish sensitive logs, credentials, access tokens, customer data, or
exploit details in a public issue. Contact the repository owner privately for
security-sensitive reports.
