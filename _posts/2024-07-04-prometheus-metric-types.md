---
layout: post
title: Prometheus Metric Types
short_description: Prometheus has a few metric type, which are; gauge, historgram, summary, counter...
date: 2024-07-04
---

# Prometheus Metric Types

Prometheus has a few metric type, which are;

**Gauge**: Fluctuating single numerical value that can go up and down
**Histogram**: A distribution view of bucketed observations
**Summary**: Calculated quantiles over a sliding time window
**Counter**: A cumulative count that only increases

## Gauge Metric

Gauges are similar to counters but can increase or decrease arbitrarily. They represent a snapshot value at a specific point in time. Typically, they are used to measure metrics that can fluctuate, such as CPU/memory usage, number of concurrent connections, temperature readings, and more.

### Characteristics of Gauges

1. **Dynamic Values**: Gauges can go up and down. This makes them suitable for metrics that vary over time.
2. **Instantaneous Measurement**: Gauges capture the current value at the time of collection, providing an instantaneous view of the metric.
3. **Use Cases**: Common use cases for gauges include monitoring resource usage (CPU, memory), active sessions, queue lengths, and other metrics that can both increase and decrease.

### Example Usage

Here is an example of how you might define and use a Gauge in a Prometheus-compatible application:

```go
// Define a Gauge
var cpuUsage = prometheus.NewGauge(prometheus.GaugeOpts{
    Name: "cpu_usage",
    Help: "Current CPU usage",
})

// Register the Gauge with Prometheus
prometheus.MustRegister(cpuUsage)

// Set the Gauge value
cpuUsage.Set(23.5) // setting the current CPU usage to 23.5%
