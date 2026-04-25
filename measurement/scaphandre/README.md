# Scaphandre

Energy consumption measurement tool using Intel RAPL.
Runs as a host systemd service to remain external to experiment environments.

## Installation

```bash
curl -L -o /tmp/scaphandre.deb \
  https://github.com/hubblo-org/scaphandre/releases/download/v1.0.2/scaphandre_v1.0.2-deb12_amd64.deb
sudo dpkg -i /tmp/scaphandre.deb
sudo chmod -R a+r /sys/class/powercap/intel-rapl*/
sudo cp scaphandre.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable scaphandre
sudo systemctl start scaphandre
```

## Metrics endpoint
http://localhost:8083/metrics

## Key metrics
- `scaph_host_power_microwatts` — total host power
- `scaph_process_power_consumption_microwatts` — per process power
