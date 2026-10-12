# Changelog — pc-fix

All notable changes documented here. Format follows [Keep a Changelog](https://keepachangelog.com/).

## [1.1.0] - 2026-10-13

### Fixed
- Security updates: replaced broken `--security-only` flag with `apt list --upgradable | grep -i security | xargs apt install` filter
- Dry-run mode now shows actual commands that would execute

### Added
- `--no-update` flag to skip security updates
- `--no-snap` flag to skip snap cleanup
- Smartmontools auto-detection for disk health check

### Changed
- Improved error handling with `|| true` for non-critical steps
- VA-API check now shows helpful message if driver missing

## [1.0.0] - 2026-10-13

### Added
- Initial release: 12-step Ubuntu optimization
- Security updates, package cache, journal vacuum
- Snap revision cleanup
- fstrim.timer enable/verify
- VA-API verification
- mpv installation
- VLC snap integration fix
- Swappiness tuning (vm.swappiness=60)
- Temp file cleanup
- SMART disk health check
- Status summary (disk, journal, boot time, memory)
- Command-line flags: `--dry-run`, `--help`