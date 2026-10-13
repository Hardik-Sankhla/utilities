# utilities

Collection of system administration and optimization utilities for Ubuntu/Linux.

## Structure

```
utilities/
├── pc-fix/           # One-command Ubuntu system optimization
│   ├── pc-fix        # Main script
│   ├── README.md     # pc-fix documentation
│   └── CHANGELOG.md  # Version history
├── pc-clean/         # Safe memory/cache cleanup
│   ├── pc-clean      # Main script
│   ├── README.md     # pc-clean documentation
│   └── CHANGELOG.md  # Version history
├── <future-util-1>/  # Future utilities
└── <future-util-2>/
```

## Installation

### Option 1: Symlink to ~/.local/bin (recommended)
```bash
# Add to ~/.bashrc or ~/.zshrc
export PATH="/mnt/Storage/Career/utilities/pc-fix:/mnt/Storage/Career/utilities/pc-clean:$PATH"

# Or symlink individually
ln -sf /mnt/Storage/Career/utilities/pc-fix/pc-fix ~/.local/bin/pc-fix
ln -sf /mnt/Storage/Career/utilities/pc-clean/pc-clean ~/.local/bin/pc-clean
```

### Option 2: Source directly
```bash
source /mnt/Storage/Career/utilities/pc-fix/pc-fix
source /mnt/Storage/Career/utilities/pc-clean/pc-clean
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

### pc-clean
Safe memory/cache cleanup — no snake oil, just kernel interfaces.

```bash
pc-clean              # Safe: sync + drop clean caches
pc-clean --aggressive # Also compact memory + drop slab (sudo)
pc-clean --dry-run    # Preview
```

**What it does (default):**
1. `sync` — flush dirty pages to disk
2. Drop clean pagecache (reclaimable)
3. Drop clean dentries/inodes (reclaimable)
4. Shows memory before/after with **available** focus

**Aggressive mode (`--aggressive`):**
5. Compact memory (defragment)
6. Drop slab reclaimable

**When to use:** Before heavy workload (VM, compile, game), after closing memory-hungry apps, when `available` memory is low.
**When NOT:** Routine "speed up" — Linux caches for performance; dropping caches temporarily slows the system.

## Adding New Utilities

1. Create directory: `mkdir -p utilities/<name>`
2. Add executable script: `utilities/<name>/<name>`
3. Add `README.md` with usage docs
4. Add `CHANGELOG.md` for version history
5. Update this README's "Utilities" section

## Integration with Workspace

This `utilities/` directory is designed to be merged into the main workspace repo (`Hardik-Sankhla/hermes-workspace-backup-dell-7390`) as a `utilities/` subdirectory, preserving git history via `git subtree` or manual copy.