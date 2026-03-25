# DragonEye Utils

[![CI](https://github.com/DragonSecurity/DragonEye-utils/actions/workflows/ci.yml/badge.svg)](https://github.com/DragonSecurity/DragonEye-utils/actions/workflows/ci.yml)
[![Go Reference](https://pkg.go.dev/badge/eye.dragonsecurity.io/utils.svg)](https://pkg.go.dev/eye.dragonsecurity.io/utils)

Shared Go utility library for the DragonEye/OpenUEM platform.

## Installation

```bash
go get eye.dragonsecurity.io/utils
```

## Overview

This package provides cross-platform utilities used by DragonEye components (server, agent, console):

| Module | Description |
|--------|-------------|
| `config` | OS-specific INI configuration file path resolution |
| `crypto` | PEM/PFX certificate I/O, SHA-256 hashing, serial number and PIN generation |
| `conversion` | UTF-16 byte slice to string conversion |
| `database` | Postgres connection URL construction from INI config |
| `download` | HTTP file download with SHA-256 verification |
| `jwt` | JWT key retrieval (INI on Linux, Windows Credential Manager on Windows) |
| `logger` | Structured file logging with OS-specific log paths |
| `netbird` | NetBird VPN management API client (setup keys, peers) |
| `service` | Windows Service lifecycle management |

## Platform Support

Most utilities have platform-specific implementations via Go build tags:

- **Windows** — credentials via Windows Credential Manager (`wincred`), service management via `golang.org/x/sys/windows/svc`
- **Linux** — credentials and config from `/etc/openuem-server/openuem.ini`, logs to `/var/log/openuem-server/`
- **macOS** — config from `/Library/OpenUEMServer/etc/openuem-server/openuem.ini`

## License

[Apache License 2.0](LICENSE)
