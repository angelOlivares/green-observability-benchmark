# Alumet Energy Measurement Agent

Academic-grade energy measurement tool using Intel RAPL via perf-events.
Developed by LIG (Grenoble Computer Science Lab) and Eviden (Atos HPC R&D).
Runs as a host systemd service external to all experiment environments.

## Installation

```bash
curl -L -o /tmp/alumet.deb \
  https://github.com/alumet-dev/alumet/releases/download/v0.9.4/alumet-agent_0.9.4-1_amd64_ubuntu_24.04.deb
sudo dpkg -i /tmp/alumet.deb
sudo cp alumet-config.toml /etc/alumet/
sudo cp alumet.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable alumet
sudo systemctl start alumet
```

## Metrics endpoint
http://localhost:9091/metrics

## Key metrics
- `rapl_consumed_energy_alumet` — energy consumed in Joules per RAPL domain

## RAPL domains measured
- `package` — full CPU package (cores + iGPU + L3 cache)
- `pp0` — CPU cores only

## Notes
- Uses perf-events kernel interface (lower overhead than powercap)
- See docs/methodology.md for details on running alongside Scaphandre
