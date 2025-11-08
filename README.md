# 🛡️ soc-alert-automation

Python-based alert pipeline for my home SOC lab.

It ingests Wazuh and Suricata alerts from the `lab-core-network`, normalizes them into a common schema, applies detection rules, enriches the alerts, and sends notifications to analysts (console and Discord/Slack).

This is the “glue” between the raw logs in the lab and the human doing the analysis.

---

## 🔍 What This Project Does

- **Ingests alerts**  
  - Reads Wazuh and Suricata alerts from JSON files, stdin, or a directory.
- **Normalizes formats**  
  - Converts different alert formats into a single internal alert schema.
- **Applies detection rules**  
  - Uses `config/rules.yaml` to flag brute force, port scans, DNS anomalies, etc.
- **Enriches alerts**  
  - Adds GeoIP, simple reputation tags, and severity scores.
- **Notifies analysts**  
  - Pretty-prints to console and/or sends messages to Discord/Slack via webhooks.

Designed to run on top of my `lab-core-network` and SOC lab, but you can point it at any Wazuh/Suricata-style JSON alerts.

---

## 🧱 Project Structure

```text
soc-alert-automation/
├── README.md
├── pyproject.toml / requirements.txt
├── config/
│   ├── settings.yaml       # paths, webhook URLs, general settings
│   └── rules.yaml          # detection rules + thresholds
├── data/
│   └── samples/
│       ├── wazuh-alerts.json
│       └── suricata-alerts.json
├── src/
│   ├── soc_alert_automation/
│   │   ├── __init__.py
│   │   ├── loader.py       # load/stream alerts from files/stdin
│   │   ├── parser.py       # normalize Wazuh/Suricata formats
│   │   ├── rules.py        # rule engine using rules.yaml
│   │   ├── enricher.py     # GeoIP, tags, severity
│   │   └── notifiers/
│   │       ├── __init__.py
│   │       ├── console.py  # console output
│   │       └── discord.py  # or slack.py – webhook notifier
│   └── main.py             # CLI entrypoint
├── tests/
│   ├── test_parser.py
│   ├── test_rules.py
│   └── test_enricher.py
└── docs/
    ├── architecture.md
    ├── lab-integration.md
    └── screenshots/
        ├── console-alert.png
        └── discord-alert.png