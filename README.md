# utilities

Collection of system administration and optimization utilities for Ubuntu/Linux.

## Structure

```
utilities/
├── pc-fix/           # One-command Ubuntu system optimization
│   ├── pc-fix        # Main script
│   ├── README.md     # pc-fix documentation
│   └── CHANGELOG.md  # Version history
├── <future-util-1>/  # Future utilities
└── <future-util-2>/
```

## Installation

### Option 1: Symlink to ~/.local/bin (recommended)
```bash
# Add to ~/.bashrc or ~/.zshrc
export PATH="$HOME/.hermes/utilities/pc-fix:$PATH"

# Or symlink individually
ln -sf ~/.hermes/utilities/pc-fix/pc-fix ~/.local/bin/pc-fix
```

### Option 2: Source directly
```bash
source ~/.hermes/utilities/pc-fix/pc-fix
```

## Utilities

### pc-fix
One-command Ubuntu system optimization — like `hermes doctor --fix` but for your PC.

```bash
pc-fix              # Full optimization (12 steps)
pc-fix --dry-run    # Preview changes
pc-fix --no-snap    # Skip snap cleanup
pc-fix --no-update  # Skip security updates
```

**What it does:**
1. Security updates (filters for security packages only)
2. Package cache cleanup (`apt clean/autoclean/autoremove`)
3. Journal vacuum (200 MB limit)
4. Disabled snap revision removal (~2-3 GB)
5. fstrim.timer enable/verify (SSD TRIM)
6. VA-API verification (Intel hardware video decode)
7. mpv installation (native video player)
8. VLC snap desktop integration fix
9. Swappiness tuning (`vm.swappiness=60` for zram)
10. Temp file cleanup (`/tmp`, `/var/tmp`)
11. Disk health check (SMART)
12. Status summary (disk, journal, boot time, memory)

## Adding New Utilities

1. Create directory: `mkdir -p utilities/<name>`
2. Add executable script: `utilities/<name>/<name>`
3. Add `README.md` with usage docs
4. Add `CHANGELOG.md` for version history
5. Update this README's "Utilities" section

## Integration with Workspace

This `utilities/` directory is designed to be merged into the main workspace repo (`Hardik-Sankhla/hermes-workspace-backup-dell-7390`) as a `utilities/` subdirectory, preserving git history via `git subtree` or manual copy.