# Changelog

All notable changes to this project will be documented in this file.

## [1.1.0] - 2025-01-18

### Added
- Home Assistant CLI integration for direct HA control from Claude Terminal
- Claude instructions (CLAUDE.md) for Home Assistant-specific commands and workflows
- Fix for missing credentials script to improve error handling during initialization

### Changed
- Enhanced Docker configuration with HA CLI support
- Added elevated privileges (hassio_api, hassio_role: manager, docker_api) for HA CLI access

## [1.0.2] - Previous Release
- Initial stable release with core functionality