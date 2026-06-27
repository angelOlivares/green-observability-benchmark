## Measurement Architecture

### RAPL Interface

All three tools read from Intel RAPL hardware counters, but via different kernel interfaces:

| Tool | Interface | Path |
|---|---|---|
| Scaphandre v1.0.2 | powercap sysfs | `/sys/class/powercap/intel-rapl` |
| Kepler v0.11.4 | powercap sysfs | `/sys/class/powercap/intel-rapl` |
| Alumet v0.9.4 | perf-events | kernel perf subsystem |

On the test machine (Intel i7-3770, single socket), two RAPL domains are available:

| Domain | sysfs path | Description |
|---|---|---|
| Package | `intel-rapl:0` | CPU cores, uncore, L3 cache, memory controller |
| PP0 (cores) | `intel-rapl:0:0` | CPU cores only |

DRAM is not exposed as a separate RAPL domain on this hardware. All three tools therefore read the same physical quantity. Disagreements observed in RQ1 reflect read-path and timing differences, not different physical measurements.

### Host-Level Metrics

The following metrics are used for host-level energy measurement and SCI calculation:

| Tool | Metric | Type | Unit | Domain |
|---|---|---|---|---|
| Scaphandre | `scaph_host_power_microwatts` | gauge | µW | Package (host total) |
| Alumet | `rapl_consumed_energy_alumet{domain="package"}` | gauge | J (5s delta) | Package |
| Kepler | `kepler_node_cpu_watts{zone="package"}` | gauge | W | Package |

### Canonical Power Representation

All metrics are normalized to **watts** for comparison and SCI input:

| Tool | PromQL |
|---|---|
| Scaphandre | `scaph_host_power_microwatts / 1e6` |
| Alumet | `rapl_consumed_energy_alumet{domain="package", resource_kind="local_machine"} / 5` |
| Kepler | `kepler_node_cpu_watts{zone="package"}` |

Energy over an experiment window of `T` seconds:
E (kWh) = avg_over_time(power_watts[T]) × T / 3_600_000

### Sampling Configuration

| Tool | Poll interval | Scrape interval | Notes |
|---|---|---|---|
| Scaphandre | — | 15s | Governed by Prometheus scrape |
| Alumet | 5s | 15s | poll = flush = 5s; perf-events interface |
| Kepler | 5s | 15s | Internal monitor interval |