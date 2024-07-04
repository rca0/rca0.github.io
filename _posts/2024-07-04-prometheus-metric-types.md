---
layout: post
title: Prometheus Metric Types
short_description: Prometheus has a few metric type, which are; gauge, historgram, summary, counter...
date: 2024-07-04
---

# Prometheus Metric Types

Prometheus has a few metric types, which are;

**Gauge**: Fluctuating single numerical value that can go up and down.
**Histogram**: A distribution view of bucketed observations.
**Summary**: Calculated quantiles over a sliding time window.
**Counter**: A cumulative count that only increases.

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
```

## Histogram Metric

Histograms track the distribution of events over time. They are useful for measuring the statistical distribution of values, such as request durations or response sizes.

### Characteristics of Histograms

1. **Bucketed Observations**: Histograms work by categorizing observations into predefined buckets. Each bucket counts the number of observations that fall into a specific range.
2. **Cumulative Counts**: Prometheus histograms maintain cumulative counts for each bucket, making it possible to derive the count of observations that fall into any specific range by subtracting the counts of adjacent buckets.
3. **Sum of Observations**: Histograms also track the sum of all observed values, allowing you to calculate the average of the observations.

### Example Usage

Here is an example of how you might define and use a Histogram in a Prometheus-compatible application:

```go
// Define a Histogram
var requestDuration = prometheus.NewHistogram(prometheus.HistogramOpts{
    Name:    "http_request_duration_seconds",
    Help:    "A histogram of the request duration.",
    Buckets: prometheus.DefBuckets, // Default buckets provided by Prometheus
})

// Register the Histogram with Prometheus
prometheus.MustRegister(requestDuration)

// Observe a value
requestDuration.Observe(1.2) // observing a request duration of 1.2 seconds
```

## Summary Metric

Summaries are similar to histograms but also provide a total count and sum of observed values. They are useful for monitoring request latencies and other distributions where quantile estimation is needed.

### Characteristics of Summaries

1. **Quantile Calculation**: Summaries calculate configurable quantiles (e.g., 0.5, 0.9, 0.99) over a sliding time window, which makes them useful for latency measurements and other performance metrics.
2. **Total Count and Sum**: Summaries maintain a total count of observations and the sum of all observed values, allowing you to calculate the average of the observations.
3. **Sliding Window**: The quantiles are calculated over a configurable sliding time window, which helps in getting recent statistics without considering outdated data.

### Example Usage

Here is an example of how you might define and use a Summary in a Prometheus-compatible application:

```go
// Define a Summary
var requestLatency = prometheus.NewSummary(prometheus.SummaryOpts{
    Name:       "http_request_latency_seconds",
    Help:       "A summary of the request latency.",
    Objectives: map[float64]float64{0.5: 0.05, 0.9: 0.01, 0.99: 0.001},
})

// Register the Summary with Prometheus
prometheus.MustRegister(requestLatency)

// Observe a value
requestLatency.Observe(1.2) // observing a request latency of 1.2 seconds
```

