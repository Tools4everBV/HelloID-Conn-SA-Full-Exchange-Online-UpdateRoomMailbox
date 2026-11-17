# Changelog

All notable changes to this project will be documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com),
and this project adheres to [Semantic Versioning](https://semver.org).

## [1.0.1] - 2026-11-17

### Fixed

- Boolean values (`blnSetAsPrimaryEmail`) are now checked for null or empty before `[System.Convert]::ToBoolean()`, so a switch that was not touched in the form no longer causes an error

## [1.0.0] - 2026-06-08

### Added
- Initial connector baseline, including delegated form scripts/resources and changelog registration.