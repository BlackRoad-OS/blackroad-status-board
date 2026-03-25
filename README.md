<!-- BlackRoad SEO Enhanced -->

# ulackroad status uoard

> Part of **[BlackRoad OS](https://blackroad.io)** — Sovereign Computing for Everyone

[![BlackRoad OS](https://img.shields.io/badge/BlackRoad-OS-ff1d6c?style=for-the-badge)](https://blackroad.io)
[![BlackRoad OS](https://img.shields.io/badge/Org-BlackRoad-OS-2979ff?style=for-the-badge)](https://github.com/BlackRoad-OS)
[![License](https://img.shields.io/badge/License-Proprietary-f5a623?style=for-the-badge)](LICENSE)

**ulackroad status uoard** is part of the **BlackRoad OS** ecosystem — a sovereign, distributed operating system built on edge computing, local AI, and mesh networking by **BlackRoad OS, Inc.**

## About BlackRoad OS

BlackRoad OS is a sovereign computing platform that runs AI locally on your own hardware. No cloud dependencies. No API keys. No surveillance. Built by [BlackRoad OS, Inc.](https://github.com/BlackRoad-OS-Inc), a Delaware C-Corp founded in 2025.

### Key Features
- **Local AI** — Run LLMs on Raspberry Pi, Hailo-8, and commodity hardware
- **Mesh Networking** — WireGuard VPN, NATS pub/sub, peer-to-peer communication
- **Edge Computing** — 52 TOPS of AI acceleration across a Pi fleet
- **Self-Hosted Everything** — Git, DNS, storage, CI/CD, chat — all sovereign
- **Zero Cloud Dependencies** — Your data stays on your hardware

### The BlackRoad Ecosystem
| Organization | Focus |
|---|---|
| [BlackRoad OS](https://github.com/BlackRoad-OS) | Core platform and applications |
| [BlackRoad OS, Inc.](https://github.com/BlackRoad-OS-Inc) | Corporate and enterprise |
| [BlackRoad AI](https://github.com/BlackRoad-AI) | Artificial intelligence and ML |
| [BlackRoad Hardware](https://github.com/BlackRoad-Hardware) | Edge hardware and IoT |
| [BlackRoad Security](https://github.com/BlackRoad-Security) | Cybersecurity and auditing |
| [BlackRoad Quantum](https://github.com/BlackRoad-Quantum) | Quantum computing research |
| [BlackRoad Agents](https://github.com/BlackRoad-Agents) | Autonomous AI agents |
| [BlackRoad Network](https://github.com/BlackRoad-Network) | Mesh and distributed networking |
| [BlackRoad Education](https://github.com/BlackRoad-Education) | Learning and tutoring platforms |
| [BlackRoad Labs](https://github.com/BlackRoad-Labs) | Research and experiments |
| [BlackRoad Cloud](https://github.com/BlackRoad-Cloud) | Self-hosted cloud infrastructure |
| [BlackRoad Forge](https://github.com/BlackRoad-Forge) | Developer tools and utilities |

### Links
- **Website**: [blackroad.io](https://blackroad.io)
- **Documentation**: [docs.blackroad.io](https://docs.blackroad.io)
- **Chat**: [chat.blackroad.io](https://chat.blackroad.io)
- **Search**: [search.blackroad.io](https://search.blackroad.io)

---


A lightweight, self-hosted service-health monitor that tracks HTTP endpoints, records response times, calculates uptime percentages, and fires alerts when services degrade or go down. All data lives in a local SQLite database — no external dependencies, no cloud account required.

The monitoring engine makes real HTTP requests using Python's standard `urllib` library. Each check records status, response time, and HTTP status code. Uptime is recalculated on every check using a rolling window of the last 100 results, giving an accurate picture without unbounded storage growth.

Part of the **BlackRoad OS** developer toolchain — drop it into any CI pipeline or cron job for always-on uptime visibility.

## Features

- **HTTP health checks** — validates status code, measures response time, classifies `operational` / `degraded` / `down`
- **Rolling uptime** — percentage calculated from the last 100 checks per service
- **Automatic alerting** — writes an alert record whenever a service transitions to `down`
- **Multi-service batch check** — check all registered services with one command
- **JSON report export** — full service list + recent alerts in a single file
- **SQLite persistence** — zero-config database at `~/.blackroad/status_board.db`
- **CLI interface** — `list`, `add`, `check`, `export`

## Installation

```bash
git clone https://github.com/BlackRoad-OS/blackroad-status-board.git
cd blackroad-status-board
python3 src/status_board.py
```

Run the test suite:

```bash
pip install pytest
pytest tests/ -v
```

## Usage

```bash
# Add a service to monitor
python3 src/status_board.py add "GitHub API" "https://api.github.com"
python3 src/status_board.py add "My API" "https://api.myapp.com" 200

# Check a specific service right now
python3 src/status_board.py check "GitHub API"

# Check all registered services
python3 src/status_board.py check

# List current status of all services
python3 src/status_board.py list

# Export a JSON report
python3 src/status_board.py export /tmp/status_report.json
```

### Example output

```
=== Service Status (2) ===
  ✓ GitHub API  | https://api.github.com          | operational | 142ms | Uptime: 100.0%
  ✗ My API      | https://api.myapp.com           | down        | 10023ms | Uptime: 72.0%
```

## API

### `Service`
Dataclass for a monitored endpoint:

| Field | Type | Description |
|---|---|---|
| `name` | `str` | Unique service name |
| `url` | `str` | Full URL to check |
| `status` | `str` | `operational`, `degraded`, or `down` |
| `response_time_ms` | `float` | Last response time in milliseconds |
| `uptime_pct` | `float` | Rolling uptime percentage (0–100) |
| `expected_status_code` | `int` | HTTP code considered healthy (default 200) |

### `StatusBoardManager`

| Method | Description |
|---|---|
| `add_service(s)` | Register a new service |
| `check_service(name)` | Run a live HTTP check, persist result |
| `check_all()` | Check every registered service |
| `get_status()` | Return current status for all services |
| `export_report(path)` | Write JSON report with services + alerts |

## License

MIT © BlackRoad OS, Inc.
