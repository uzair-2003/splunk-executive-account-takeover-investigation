# Response & Containment Plan

This document describes simulated SOC response actions. No real production systems were modified.

| ID | Action | Purpose | Status |
|---|---|---|---|
| C-01 | Revoke active sessions | Remove attacker access | Simulated |
| C-02 | Reset CEO password | Invalidate suspected credential | Simulated |
| C-03 | Remove unauthorized Pixel 6 authenticator | Remove persistence mechanism | Simulated |
| C-04 | Delete unauthorized inbox rule | Restore mailbox behavior | Simulated |
| C-05 | Block observed suspicious IPs | Reduce further authentication attempts | Simulated |
| C-06 | Review affected accounts | Assess broader scope | Simulated |
| C-07 | Continue monitoring | Detect recurrence | Planned |

Important limitation: historical audit telemetry showing that an inbox rule was created does not prove its current mailbox state. In a real environment, current state would be verified through the appropriate Microsoft 365/Exchange administrative telemetry or interface.
