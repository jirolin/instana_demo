# Instana x EDA Demo

IBM Instana APM alerts integrated with Ansible Event-Driven Automation (EDA).

## Overview

This project demonstrates how Instana monitoring alerts can trigger automated remediation through Ansible EDA.

## Structure

```
instana_demo/
├── collections/
│   └── requirements.yml          # ibm.instana collection dependency
├── rulebooks/
│   └── instana-event-routing.yml # EDA rulebook for event routing
├── playbooks/
│   ├── log-event.yml             # Log all received events
│   └── notify-event.yml          # Route events by type
└── README.md
```

## EDA Controller Setup

1. Create a **Project** pointing to this Git repository
2. Select a **Decision Environment** with Python 3.9+ and aiohttp
3. Create a **Rulebook Activation** using `instana-event-routing.yml`
4. The `collections/requirements.yml` ensures `ibm.instana` is installed at activation time

## Event Routing Rules

| Event Pattern | Action |
|---------------|--------|
| `process.*down` | Service restart |
| `disk.*usage` | Disk cleanup |
| `erroneous call rate` / `response time` / `high latency` | AI analysis |
| All other events | Log only |

## Testing with curl

```bash
# Basic event
curl -X POST http://<eda-host>:5000 \
  -H "Content-Type: application/json" \
  -d '{"message": "Test event", "host": "server-01", "severity": 5}'

# Process down
curl -X POST http://<eda-host>:5000 \
  -H "Content-Type: application/json" \
  -d '{"message": "Node failed: process httpd is down", "host": "app-01", "severity": 10}'

# Performance (AI-Driven)
curl -X POST http://<eda-host>:5000 \
  -H "Content-Type: application/json" \
  -d '{"message": "Erroneous call rate is too high", "host": "api-01", "severity": 8}'
```

## Requirements

- Ansible Automation Platform 2.4+
- EDA Controller
- `ibm.instana` collection (auto-installed via requirements.yml)

## License

Apache-2.0
