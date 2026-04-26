# Green Observability Benchmark

A research project comparing the energy consumption and carbon footprint of popular observability stacks using the OpenTelemetry Astronomy Shop as a standard workload.

## Motivation

Observability stacks are always-on infrastructure components. Their energy consumption is rarely measured or compared. This project applies the Green Software Foundation's SCI (Software Carbon Intensity) methodology to quantify and compare the environmental impact of different observability backends.

Prior work (GOXN, 2026) measured the energy cost of the demo application itself. This project measures the energy cost of the **observability backends monitoring it** — a gap not yet addressed in the literature.

## Research Questions

**RQ1:** Which observability backend consumes the least energy and produces the lowest carbon footprint under identical workload conditions?

**RQ2:** Do different energy measurement tools agree on energy consumption readings when measuring the same workload simultaneously?

## Workload

The [OpenTelemetry Astronomy Shop](https://github.com/open-telemetry/opentelemetry-demo) is used as the standard workload — a CNCF-maintained microservices demo application with a built-in load generator. Its built-in observability stack (Prometheus + Jaeger + OpenSearch) serves as the **baseline** for comparison.

## Backends Compared

| Backend | Source | Description |
|---|---|---|
| Baseline | OTel demo built-in | Prometheus + Jaeger + OpenSearch |
| LGTM | Grafana Labs official | Loki + Grafana + Tempo + Mimir |
| VictoriaMetrics | VM official | VictoriaMetrics + Grafana |
| Elastic | Elastic official | Elasticsearch + Kibana |

## Measurement Tools

Three independent tools measure energy consumption simultaneously from the host, external to all experiment environments:

| Tool | RAPL Interface | Metrics | Port |
|---|---|---|---|
| Scaphandre v1.0.2 | powercap sysfs | Power (W) | 8083 |
| Alumet v0.9.4 | perf-events | Energy (J) | 9091 |
| Kepler | eBPF + perf-events | Energy (J) | 8085 |

All three tools read from Intel RAPL hardware counters. RAPL is **read-only** — tools do not interfere with each other or with the experiment environment.

Running three tools simultaneously serves two purposes:
1. Cross-validation of measurements
2. Tool comparison contributing to RQ2

## Architecture
HOST LAYER (measurement):
Scaphandre  :8083  ─┐
Alumet      :9091  ─┼→ Prometheus :9099 → Grafana :3001
Kepler      :8085  ─┘
EXPERIMENT LAYER (what we measure):
OTel Astronomy Shop + Backend X (Docker Compose)

Measurement tools run as host systemd services, completely external to Docker Compose experiment environments.

## SCI Framework

This project applies the Green Software Foundation's Software Carbon Intensity formula:
SCI = (E × I) + M per R
E = Energy consumed (Scaphandre/Alumet/Kepler)
I = Carbon intensity of electricity (gCO2/kWh)
M = Embodied carbon of hardware
R = Functional unit (per request)

## Known Limitations

- Measurement tools consume energy themselves and are excluded from results
- RAPL covers CPU and DRAM but not disk I/O or network
- Container-level energy attribution uses proportional models (approximate)
- Results are specific to the test machine hardware

## Status

🚧 Work in progress

## References

- [Green Software Foundation SCI](https://sci.greensoftware.foundation)
- [GOXN Paper (2026)](https://arxiv.org/abs/2601.16635)
- [Alumet Paper (2025)](https://hal.science/hal-05246933)
- [OTel Astronomy Shop](https://github.com/open-telemetry/opentelemetry-demo)

## Author

Angel Olivares — Observability Engineer
