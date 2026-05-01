# check_chrony

[![License: GPL-3.0](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](LICENSE)

Icinga2 / Nagios monitoring plugin that checks the **Chrony** NTP & PTP time offset against configurable warning and critical thresholds. Returns performance data for graphing. No sudo privileges required, no Python or Perl — pure Bash.

---

## Requirements

- `bash`
- `chronyc` (part of the `chrony` package)
- `bc` (for floating-point threshold comparison)

### Tested on

- Debian 11, 12
- Ubuntu 22.04, 24.04

---

## Installation

```bash
cp check_chrony /usr/local/nagios/plugins/check_chrony
chmod +x /usr/local/nagios/plugins/check_chrony
```

---

## Usage

```
check_chrony -w <warning> -c <critical> [--debug]
```

| Option | Required | Description |
|:-------|:---------|:------------|
| `-w` | yes | Warning threshold in seconds (float) |
| `-c` | yes | Critical threshold in seconds (float) |
| `--debug` | no | Print raw `chronyc tracking` output and parsed values |

---

## Examples

```bash
# Warn at 1 s, critical at 3 s
check_chrony -w 1 -c 3

# Stricter thresholds for PTP environments
check_chrony -w 0.0001 -c 0.001

# Debug — show raw chronyc output
check_chrony -w 0.5 -c 2 --debug
```

**Sample output:**

```
OK: Time offset of +0.000046626 seconds to reference. |offset=0.000046626s;1.000000000;3.000000000
```

---

## Exit codes

| Code | State | Condition |
|:-----|:------|:----------|
| 0 | OK | Offset below warning threshold, leap status normal |
| 1 | WARNING | Offset >= warning threshold |
| 2 | CRITICAL | Offset >= critical threshold, or `chronyd` not running, or leap status not `Normal` |
| 3 | UNKNOWN | `chronyc` command failed or output could not be parsed |

---

## Icinga2 / Nagios configuration

**Command definition:**

```
object CheckCommand "check_chrony" {
  command = [ "/usr/local/nagios/plugins/check_chrony" ]
  arguments = {
    "-w" = "$chrony_warn$"
    "-c" = "$chrony_crit$"
  }
}
```

**Service definition:**

```
object Service "ntp-offset" {
  host_name = "myhost"
  check_command = "check_chrony"
  vars.chrony_warn = "0.5"
  vars.chrony_crit = "2"
}
```

---

## License

[GPL-3.0](LICENSE)
