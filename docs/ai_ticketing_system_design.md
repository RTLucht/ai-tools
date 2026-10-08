# AI-Assisted IT Ticketing System — Design Document

**Concept:** Automated ticket intake with remote diagnostics and AI-drafted summaries.

## Design Principles

- **Deterministic diagnostics** — Python code decides pass/fail; the AI never guesses from raw data.
- **Read-only by default** — the collector gathers evidence; it does not change anything on the endpoint.
- **Confidence scores** — every check reports how reliable its verdict is.
- **Human in the loop** — the AI drafts, the tech confirms. Nothing is auto-fixed.

## System Flow

1. **Ticket Intake** — User submits a complaint (email, portal, chat) with computer name and issue description.
2. **Collector Dispatch** — Python collector connects to the endpoint over WinRM (Windows) or SSH (Linux).
3. **Deterministic Checks** — Registry of checks: disk space, memory, CPU, event logs, network, pending updates. Same input, same output, every time.
4. **Structured Report** — JSON output: status (pass/fail/warn), severity, confidence, metrics, root-cause hypothesis, suggested fix per check.
5. **AI Narration** — AI receives only structured JSON (never raw logs) and writes the human-readable ticket summary.
6. **Human Review** — Technician verifies findings and approves or overrides.
7. **Resolution** — Tech applies fixes, updates ticket, closes it. JSON report stays as audit trail.

## Architecture Components

| Component | Role | Notes |
|---|---|---|
| Ticket intake | Receives complaints | Email parser, portal form, chat webhook |
| Collector service | Orchestrates remote checks | Python; WinRM/SSH; runs check registry |
| Check registry | Catalog of diagnostic checks | Config-driven; new checks don't require code changes |
| JSON report | Structured diagnostic output | Pass/fail/warn, severity, confidence, metrics, hypotheses |
| AI narrator | Drafts ticket summary | LLM reads JSON only; writes root causes + fixes |
| Ticket store | Persists tickets + reports | Existing helpdesk platform |
| Human review UI | Tech verifies and acts | Drafted summary alongside raw JSON for audit |

## Example Diagnostic Report

Ticket **TKT-2026-1042**, workstation **WS-FINANCE-07**. User reported: computer slow, Excel freezing.

| Check | Status | Severity | Confidence | Key Finding |
|---|---|---|---|---|
| disk_space | FAIL | high | 98% | C: drive 1.6% free (4.2 GB of 256 GB) |
| memory | FAIL | medium | 95% | 1.8 GB available; Excel using 3.2 GB; 94% commit |
| event_log_errors | FAIL | medium | 90% | 47 system errors in 24h; repeated DWM crashes (event 2019) |
| cpu | PASS | low | 99% | Avg 34%, peak 88% |
| network_connectivity | PASS | low | 99% | 2 ms ping, DNS OK, gateway reachable |
| windows_updates | WARN | low | 85% | 6 pending updates, reboot pending since 2026-09-14 |

**Summary:** 6 checks, 2 passed, 3 failed, 1 warning. Overall severity: **high**.

- **Primary root cause:** Disk space critical (1.6% free) cascading into memory pressure.
- **Secondary root cause:** DWM/GPU driver crashes.
- **Recommended action:** Address disk space first, then GPU driver, schedule reboot for updates.

## Sample JSON Report

```json
{
  "ticket_id": "TKT-2026-1042",
  "computer_name": "WS-FINANCE-07",
  "user_reported_issue": "Computer is slow and Excel keeps freezing",
  "collected_at": "2026-10-08T15:30:00-05:00",
  "collection_duration_seconds": 42,
  "checks": [
    {
      "check_id": "CHK-001",
      "name": "disk_space",
      "status": "FAIL",
      "severity": "high",
      "confidence": 0.98,
      "details": {"drive": "C:", "total_gb": 256, "free_gb": 4.2, "free_percent": 1.6, "threshold_percent": 10},
      "root_cause_hypothesis": "Disk nearly full - likely causing paging and app hangs",
      "suggested_fix": "Clear temp files, empty recycle bin, check for large log files"
    },
    {
      "check_id": "CHK-002",
      "name": "memory",
      "status": "FAIL",
      "severity": "medium",
      "confidence": 0.95,
      "details": {"total_gb": 16, "available_gb": 1.8, "commit_percent": 94, "top_process": "EXCEL.EXE", "top_process_mb": 3200},
      "root_cause_hypothesis": "Excel consuming 3.2GB with only 1.8GB free - memory pressure",
      "suggested_fix": "Restart Excel, check for memory leaks, consider 32GB upgrade"
    },
    {
      "check_id": "CHK-003",
      "name": "event_log_errors",
      "status": "FAIL",
      "severity": "medium",
      "confidence": 0.90,
      "details": {"source": "System", "last_24h_errors": 47, "top_event_id": 2019, "top_event_count": 31, "description": "The Desktop Window Manager process has exited"},
      "root_cause_hypothesis": "DWM crashes correlate with GPU driver instability",
      "suggested_fix": "Update or roll back GPU driver, check for pending Windows updates"
    },
    {
      "check_id": "CHK-004",
      "name": "cpu",
      "status": "PASS",
      "severity": "low",
      "confidence": 0.99,
      "details": {"avg_percent": 34, "peak_percent": 88, "top_process": "EXCEL.EXE"},
      "root_cause_hypothesis": null,
      "suggested_fix": null
    },
    {
      "check_id": "CHK-005",
      "name": "network_connectivity",
      "status": "PASS",
      "severity": "low",
      "confidence": 0.99,
      "details": {"ping_ms": 2, "dns_resolution": "ok", "gateway_reachable": true},
      "root_cause_hypothesis": null,
      "suggested_fix": null
    },
    {
      "check_id": "CHK-006",
      "name": "windows_updates",
      "status": "WARN",
      "severity": "low",
      "confidence": 0.85,
      "details": {"pending_updates": 6, "last_installed": "2026-09-14", "reboot_pending": true},
      "root_cause_hypothesis": "Reboot pending may contribute to instability",
      "suggested_fix": "Schedule maintenance window for updates and reboot"
    }
  ],
  "summary": {
    "total_checks": 6,
    "passed": 2,
    "failed": 3,
    "warnings": 1,
    "overall_severity": "high",
    "primary_root_cause": "CHK-001 disk space critical (1.6% free) cascading into memory pressure",
    "secondary_root_cause": "CHK-003 DWM/GPU driver crashes",
    "recommended_action": "Address disk space first, then GPU driver, schedule reboot for updates"
  }
}
```

## Next Steps

- Build the check registry as config (YAML/JSON) so new checks are data, not code.
- Add a confidence threshold that flags borderline checks for human attention.
- Prototype the collector against one Windows endpoint via WinRM.
- Define the JSON schema as a formal contract between collector and AI narrator.
