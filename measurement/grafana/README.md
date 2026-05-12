# Grafana - Energy Measurement Visualisation

Host-level Grafana instance for visualising energy metrics.
Runs on port **3001** to avoid conflict with experiment backends (port 3000).

## Installation

```bash
# Add Grafana apt repository (recommended - keeps it upgradeable)
sudo apt-get install -y apt-transport-https software-properties-common
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
sudo apt-get update
sudo apt-get install grafana -y

# Change default port to avoid conflicts
echo -e "[server]\nhttp_port = 3001" | sudo tee -a /etc/grafana/grafana.ini

sudo systemctl enable grafana-server
sudo systemctl start grafana-server
```

## Datasources
- **Prometheus-Measurement** — connects to host Prometheus on port 9099

## Dashboards

| File | Description |
|------|-------------|
| `provisioning/dashboards/host-scaphandre.json` | Host power consumption and measurement tool overhead |
| `provisioning/dashboards/otel-demo-scaphandre.json` | Per-service energy of the OTel demo stack |

Both dashboards use `prometheus-measurement` datasource.

### Installation

Copy or symlink the provisioning files:

```bash
sudo ln -s $(pwd)/provisioning/dashboards/dashboard-provider.yaml \
    /etc/grafana/provisioning/dashboards/dashboard-provider.yaml

sudo ln -s $(pwd)/provisioning/dashboards/host-scaphandre.json \
    /etc/grafana/provisioning/dashboards/host-scaphandre.json

sudo ln -s $(pwd)/provisioning/dashboards/otel-demo-scaphandre.json \
    /etc/grafana/provisioning/dashboards/otel-demo-scaphandre.json

sudo systemctl restart grafana-server
```

### Dashboard notes

See the dashboard JSON files for exact `exe` path filters used to distinguish
host processes from Docker container processes of the same name.

## Datasources

| File | Description |
|------|-------------|
| `provisioning/datasources/prometheus.yaml` | Host Prometheus on port 9099 |

Provisioned with uid `prometheus-measurement` — referenced by all dashboards.
To apply:

```bash
sudo cp provisioning/datasources/prometheus.yaml \
    /etc/grafana/provisioning/datasources/
sudo systemctl restart grafana-server
```
