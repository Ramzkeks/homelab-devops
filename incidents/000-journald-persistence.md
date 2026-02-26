# INC-000 — journald Persistence & Limits

| Field    | Value      |
|----------|------------|
| Date     | 2026-02    |
| Severity | Low        |
| Status   | Resolved   |

## Symptom

journald running with default config — logs stored in memory only (lost on reboot), no size limits set.

## Fix

Edited `/etc/systemd/journald.conf`:

```ini
Storage=persistent
SystemMaxUse=200M
SystemMaxFileSize=50M
MaxRetentionSec=1month
```

Restarted journald:

```bash
sudo systemctl restart systemd-journald
```

## Verification

```bash
journalctl --disk-usage
# Archived and active journals take up X.XM on disk.
```

## Prevention

Always configure journald persistence and limits on a fresh server before adding any services.
