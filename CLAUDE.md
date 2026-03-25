# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is `eye.dragonsecurity.io/utils` — a shared Go utility library for the DragonEye/OpenUEM platform. It is imported as a dependency by other DragonEye components (server, agent, console); it is not a standalone application.

## Build Commands

```bash
# Build (cross-platform — most files have build tags)
go build ./...

# Build for a specific platform
GOOS=linux go build ./...
GOOS=windows go build ./...
GOOS=darwin go build ./...

# Run tests
go test ./...

# Tidy dependencies
go mod tidy
```

## Architecture

**Single package (`utils`)** — all files belong to package `utils`. No sub-packages.

**Platform-specific code via build tags:** Most functionality has separate implementations per OS. The naming convention is `<feature>_<os>.go` with a `//go:build <os>` directive:

- `config_{darwin,linux,windows}.go` — INI config file paths (`GetConfigFile`, `GetAgentConfigFile`)
- `database_{linux,windows}.go` — Postgres connection URL construction from INI config; Windows variant reads password from Windows Credential Manager via `wincred`
- `jwt_{linux,windows}.go` — JWT key retrieval (INI on Linux, Windows Credential Manager on Windows)
- `logger_{darwin,linux,windows}.go` — `OpenUEMLogger` struct and `NewLogger`/`NewAuthLogger`; log paths differ per OS (`/var/log/openuem-server` on Linux, executable-relative `logs/` on Windows)
- `service.go` — Windows-only (`golang.org/x/sys/windows/svc`): Windows Service lifecycle (`OpenUEMWindowsService.Execute`) and control helpers

**Cross-platform files (no build tags):**
- `crypto.go` — PEM/PFX certificate and key I/O, SHA-256 hashing, serial number and PIN generation
- `conversion.go` — UTF-16 byte slice to Go string conversion (used by Windows credential decoding)
- `download.go` — HTTP file download with SHA-256 hash verification, releases endpoint querying
- `netbird.go` — NetBird VPN management API client (setup keys, peers)
- `wd.go` — Executable working directory helper

## Key Patterns

- **Configuration**: All components read from an INI file (`openuem.ini`) via `gopkg.in/ini.v1`. Path is OS-dependent — use `GetConfigFile()` / `GetAgentConfigFile()`.
- **Windows credentials**: Passwords and JWT keys on Windows are stored in Windows Credential Manager (`github.com/danieljoos/wincred`), decoded from UTF-16LE via `UTF16BytesToString`.
- **No test files exist yet** in this repository.
- **Renovate** is configured for automated dependency updates, extending presets from `DragonSecurity/renovate-presets`.