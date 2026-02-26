# journald Basics

## View logs for a specific service

```bash
journalctl -u <service-name> --no-pager
journalctl -u <service-name> -n 50 --no-pager   # last 50 lines
journalctl -u <service-name> -f                  # follow live
```

## Check journal disk usage

```bash
journalctl --disk-usage
```

## Config file location

```
/etc/systemd/journald.conf
```

## Current settings applied

```ini
Storage=persistent        # store logs on disk (survives reboot)
SystemMaxUse=200M         # max total journal size
SystemMaxFileSize=50M     # max size per journal file
MaxRetentionSec=1month    # auto-delete logs older than 1 month
```

## Apply config changes

```bash
sudo systemctl restart systemd-journald
```
