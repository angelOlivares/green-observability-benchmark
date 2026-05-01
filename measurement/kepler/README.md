# Kepler Energy Measurement Agent

CNCF project for energy consumption measurement at node, process and container level.
Uses eBPF to probe CPU performance counters and exports metrics in Prometheus format.
Runs as a host systemd service external to all experiment environments.

## Version
v0.11.4

## Installation

```bash
# Download binary
curl -L -o /tmp/kepler.tar.gz \
  https://github.com/sustainable-computing-io/kepler/releases/download/v0.11.4/kepler-0.11.4.linux-amd64.tar.gz

tar -xzf /tmp/kepler.tar.gz -C /tmp/

# Install binary
sudo cp /tmp/bin/kepler /usr/local/bin/
sudo chmod 755 /usr/local/bin/kepler

# Create config directory
sudo mkdir -p /etc/kepler
sudo cp kepler.yml /etc/kepler/

# Install and start service
sudo cp kepler.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable kepler
sudo systemctl start kepler
```

## Metrics endpoint
http://localhost:8085/metrics

## Key metrics
- `kepler_node_platform_joules_total` — total node energy in joules
- `kepler_process_package_joules_total` — energy per process
- `kepler_container_package_joules_total` — energy per container

## Configuration
Config file: `/etc/kepler/kepler.yml`

Key settings:
- Port: **8085** (avoids conflict with Scaphandre :8083 and Alumet :9091)
- Kubernetes monitoring: **disabled** (measuring Docker Compose environments)
- Metrics level: node, process, container
- GPU/Redfish: disabled

## Methodological notes
- Kepler uses eBPF + Intel RAPL via perf-events
- Running without Kubernetes — measures at process and container level via cgroups
- Runs simultaneously with Scaphandre and Alumet for cross-validation (RQ2)
- See docs/methodology.md for full measurement rationale
