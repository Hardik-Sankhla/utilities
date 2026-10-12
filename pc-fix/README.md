# pc-fix

One-command Ubuntu system optimization — like `hermes doctor --fix` but for your PC.

## Quick Start

```bash
# From utilities root
./pc-fix/pc-fix --dry-run   # Preview
./pc-fix/pc-fix             # Run
```

Or add to PATH:
```bash
export PATH="$HOME/.hermes/utilities/pc-fix:$PATH"
pc-fix --dry-run
pc-fix
```

## Usage

```bash
pc-fix [OPTIONS]

Options:
  --dry-run     Show what would be done without executing
  --no-snap     Skip snap cleanup
  --no-update   Skip security updates
  --help        Show help
```

## What It Does (12 Steps)

| Step | Action | Command |
|------|--------|---------|
| 1 | Security updates | `apt list --upgradable \| grep -i security \| xargs apt install` |
| 2 | Package cache cleanup | `apt clean && apt autoclean && apt autoremove -y` |
| 3 | Journal vacuum | `journalctl --vacuum-size=200M` |
| 4 | Snap cleanup | Remove disabled revisions |
| 5 | fstrim.timer | `systemctl enable --now fstrim.timer` |
| 6 | VA-API verify | `vainfo \| grep VAProfile` |
| 7 | Install mpv | `apt install -y mpv` |
| 8 | Fix VLC snap | `snap connect vlc:desktop-legacy/wayland` |
| 9 | Swappiness | `vm.swappiness=60` via sysctl.d |
| 10 | Temp cleanup | `rm -rf /tmp/* /var/tmp/*` |
| 11 | Disk health | `smartctl -H /dev/sda /dev/nvme0` |
| 12 | Summary | `df -h /`, `journalctl --disk-usage`, `systemd-analyze time`, `free -h` |

## Requirements

- Ubuntu 22.04+ (tested on 24.04/26.04)
- `sudo` access
- Internet for package updates
- `snapd` (for snap cleanup)
- `smartmontools` (for disk health, auto-installed if missing)

## Example Output

```
=== pc-fix: Ubuntu System Optimization ===
=== 1/12: Security Updates ===
[sudo] password for user:
Reading package lists... Done
12 packages upgraded.
=== 2/12: Package Cache Cleanup ===
Freed 342 MB
=== 3/12: Journal Vacuum (200 MB) ===
Vacuuming done, freed 349.5M
=== 4/12: Disabled Snap Revisions ===
Removed 23 disabled revisions (2.1 GB freed)
=== 5/12: fstrim.timer (SSD TRIM) ===
● fstrim.timer - Discard unused blocks once a week
   Active: active (waiting)
=== 6/12: VA-API Verification ===
VAProfileH264Main: VAEntrypointVLD
VAProfileHEVCMain: VAEntrypointVLD
VAProfileVP9Profile0: VAEntrypointVLD
=== 7/12: mpv Installation ===
mpv is already the newest version.
=== 8/12: VLC Snap Integration ===
=== 9/12: Swappiness Tuning ===
vm.swappiness = 60
=== 10/12: Temp File Cleanup ===
=== 11/12: Disk Health (SMART) ===
/dev/sda: SMART overall-health self-assessment test result: PASSED
=== 12/12: Status Summary ===
Filesystem      Size  Used Avail Use% Mounted on
/dev/mapper/... 322G   70G  238G  23% /
Archived and active journals take up 199.5M in the file system.
Startup finished in 1min 40.675s
               total        used        free      shared  buff/cache   available
Mem:            14Gi       8.3Gi       3.3Gi       1.3Gi       4.9Gi       6.4Gi
=== pc-fix complete ===
```

## Troubleshooting

| Issue | Fix |
|-------|-----|
| `sudo: terminal required` | Run in interactive terminal (not via SSH without -t) |
| `--security-only not understood` | Fixed in v1.1 — now filters security packages via grep |
| VA-API not working | Run `sudo apt install intel-media-va-driver-non-free && vainfo` |
| Snap connect fails | Ensure snapd running: `systemctl status snapd` |
| smartctl not found | `sudo apt install smartmontools` |

## License

MIT