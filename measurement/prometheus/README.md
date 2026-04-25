# Prometheus - Energy Measurement Platform

Host-level Prometheus instance that scrapes all energy measurement tools.
Runs on port **9099** to avoid conflict with experiment backends (port 9090).

## Installation

```bash
# Download latest binary (apt version is outdated)
# Current version: 3.6.0
sudo cp prometheus /usr/local/bin/
sudo cp promtool /usr/local/bin/
sudo chmod 755 /usr/local/bin/prometheus
sudo chmod 755 /usr/local/bin/promtool

# Create system user and directories
sudo useradd --no-create-home --shell /bin/false prometheus
sudo mkdir -p /etc/prometheus
sudo mkdir -p /var/lib/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus

# Copy config and service
sudo cp prometheus.yml /etc/prometheus/
sudo cp prometheus.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable prometheus
sudo systemctl start prometheus
```

## Known issue — blank Content-Type error

Prometheus 3.x requires scrape targets to send a Content-Type header.
Scaphandre 1.0.2 does not send this header, causing:
Error scraping target: non-compliant scrape target sending blank
Content-Type and no fallback_scrape_protocol specified

**Fix:** Add `fallback_scrape_protocol: PrometheusText0.0.4` to the scrape config.
See `prometheus.yml` for the implementation.

## Ports
- **9099** — Prometheus UI and API
