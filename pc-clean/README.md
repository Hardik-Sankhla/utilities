# pc-clean

Safe memory/cache cleanup for Ubuntu — no snake oil, just kernel interfaces.

## Quick Start

```bash
./pc-clean              # Safe cleanup (drop clean caches only)
./pc-clean --dry-run    # Preview
./pc-clean --aggressive # Also compact memory & drop slab (needs sudo)
```

## What It Does

| Mode | Action | Safe? |
|------|--------|-------|
| Default | `sync` → drop pagecache (clean) → drop dentries/inodes (clean) | ✅ Yes |
| `--aggressive` | + compact memory + drop slab (reclaimable) | ⚠️ Needs sudo |

## When to Use

- **Before** heavy workload (VM, large compile, game)
- **After** closing memory-hungry apps (browser, IDE, Docker)
- When `available` memory is low but `free` + `buff/cache` is high

## When NOT to Use

- Routine "make PC faster" — Linux caches aggressively for performance
- Dropping caches **temporarily slows** the system (re-reads from disk)
- If `available` memory is already high — no benefit

## Understanding Memory Output

```bash
$ free -h
              total  used  free shared buff/cache available
Mem:           14G   8.3G  3.3G   1.3G       4.9G      6.4G
```

- **available** = what new apps can actually use (includes reclaimable cache)
- **free** = truly unused (wasted)
- **buff/cache** = kernel cache (speeds up disk access)

**pc-clean only drops reclaimable portions** — it never touches dirty pages or active app memory.

## Aggressive Mode Details

```bash
pc-clean --aggressive
```

| Step | Kernel Interface | Effect |
|------|------------------|--------|
| compact_memory | `/proc/sys/vm/compact_memory` | Defragments memory (helps huge pages) |
| drop_caches=3 | `/proc/sys/vm/drop_caches` | Pagecache + dentries + slab (reclaimable) |

Requires `sudo`. Use sparingly — only when default mode isn't enough.

## Example Output

```
=== pc-clean: Memory/Cache Cleanup ===
=== Memory BEFORE ===
              total        used        free      shared  buff/cache   available
Mem:            14Gi       8.3Gi       3.3Gi       1.3Gi       4.9Gi       6.4Gi
=== 1/4: Sync filesystems ===
=== 2/4: Drop pagecache (clean pages) ===
=== 3/4: Drop dentries/inodes ===
=== 4/4: Skipped aggressive mode (use --aggressive) ===
=== Memory AFTER ===
              total        used        free      shared  buff/cache   available
Mem:            14Gi       7.8Gi       4.1Gi       1.3Gi       3.7Gi       6.5Gi
=== Key Metric: Available Memory ===
Available: 6.5Gi (this is what matters for new apps)
=== pc-clean complete ===
```

## License

MIT