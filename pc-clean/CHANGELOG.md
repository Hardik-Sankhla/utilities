# Changelog — pc-clean

All notable changes documented here. Format follows [Keep a Changelog](https://keepachangelog.com/).

## [1.0.0] - 2026-10-14

### Added
- Initial release: safe memory/cache cleanup
- Default mode: sync + drop clean pagecache + drop clean dentries/inodes
- Aggressive mode (--aggressive): compact memory + drop slab reclaimable
- Dry-run mode (--dry-run)
- Before/after memory reporting with "available" focus
- Help text explaining when to use / not use