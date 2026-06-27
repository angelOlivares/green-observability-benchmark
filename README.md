# Green Observability Benchmark

A research project comparing the energy consumption and carbon footprint of popular observability stacks using the OpenTelemetry Astronomy Shop as a standard workload.

## Motivation

Observability stacks are always-on infrastructure components. Their energy consumption is rarely measured or compared. This project applies the Green Software Foundation's SCI (Software Carbon Intensity) methodology to quantify and compare the environmental impact of different observability backends, while also evaluating the reliability of the measurement tools themselves.

Prior work (GOXN, 2026) measured the energy cost of the demo application itself. This project measures the energy cost of the **observability backends monitoring it** — a gap not yet addressed in the literature.

## Research Questions

**RQ1:** To what extent do energy consumption measurement tools agree or disagree on their readings — and consequently on carbon emissions estimates — across the OTel demo stripped baseline and all observability backend conditions?

**RQ2:** Which observability backend consumes the least energy and produces the lowest carbon footprint under identical workload conditions?

**RQ3:** What is the energy and carbon footprint of adding observability to a cloud-native application?

## Workload

The [OpenTelemetry Astronomy Shop](https://github.com/open-telemetry/opentelemetry-demo) is used as the standard workload — a CNCF-maintained microservices demo application with a built-in load generator. Its built-in observability stack (Prometheus + Jaeger + OpenSearch) serves as the **baseline** for comparison.

## Experiment Conditions

| Condition | Backend | Description |
|---|---|---|
| Stripped | None | OTel demo with all observability removed — reference baseline for RQ3 |
| Baseline | OTel demo built-in | Prometheus + Jaeger + OpenSearch |
| LGTM | Grafana Labs official | Loki + Grafana + Tempo + Mimir |
| VictoriaMetrics | VM official | VictoriaMetrics + Grafana |
| Elastic | Elastic official | Elasticsearch + Kibana |

## Measurement Tools

Three independent tools measure energy consumption simultaneously from the host, external to all experiment environments:

| Tool | RAPL Interface | Metrics | Port |
|---|---|---|---|
| Scaphandre v1.0.2 | powercap sysfs | Power (W) | 8083 |
| Alumet v0.9.4 | powercap sysfs | Energy (J) | 9091 |
| Kepler v0.11.4 | perf-events | Energy (J) | 8085 |

All three tools read from Intel RAPL hardware counters. RAPL is **read-only** — tools do not interfere with each other or with the experiment environment.

Running three tools simultaneously serves two purposes:
1. Cross-validation of measurements (RQ1)
2. Enabling per-condition energy comparison (RQ2, RQ3)

## Architecture
HOST LAYER (measurement):

Scaphandre  :8083  
Alumet      :9091
Kepler      :8085 
Prometheus  :9099
Grafana     :3001

EXPERIMENT LAYER (what we measure):
OTel Astronomy Shop + Backend X (Docker Compose)

Measurement tools run as host systemd services, completely external to Docker Compose experiment environments.

## SCI Framework

This project applies the Green Software Foundation's Software Carbon Intensity formula:
SCI = (E × I) + M per R
E = Energy consumed (Scaphandre / Alumet / Kepler)

I = Carbon intensity of electricity (gCO2/kWh) — source: Red Eléctrica de España

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
- [Alumet's Github Repo](https://github.com/alumet-dev/alumet)
- [Scaphandre's Github Repo](https://github.com/hubblo-org/scaphandre)
- [Kepler's Github Repo](https://github.com/sustainable-computing-io/kepler)
- [OTel Astronomy Shop](https://github.com/open-telemetry/opentelemetry-demo)

## Author

Angel Olivares — Observability Engineer