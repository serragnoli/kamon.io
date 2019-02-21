---
title: 'Kamon Prometheus Tutorial Wtih Concepts| Kamon Documentation'
description: 'Learn how to setup Kamon to collect metrics using Prometheus'
layout: docs
---

Installation
============
Follow the steps in [Installing Kamon] (../getting-started/). Ensure that `Kamon.addReporter(new PrometheusReporter())` 
has been added to your project.

If you're coming from Graphite
===============================

Kamon has made de conscious decision of using Prometheus Histogram to publish its metrics because:

- It allows fine-grained control of the variation on metrics during the window being analysed (more on this later)

Concept 1
=========
The first thing to be understood in Prometheus is that the numbers observed below refer to the number of times the metric
was recorded to be in the range for the given period.

Let's take the following example extract from an app using Kamon Prometheus serving these numbers from http://localhost:9095/

```
host_memory_bytes_bucket{component="system-metrics",mode="free",le="512.0"} 0.0
host_memory_bytes_bucket{component="system-metrics",mode="free",le="1024.0"} 0.0
host_memory_bytes_bucket{component="system-metrics",mode="free",le="2048.0"} 0.0
host_memory_bytes_bucket{component="system-metrics",mode="free",le="4096.0"} 0.0
host_memory_bytes_bucket{component="system-metrics",mode="free",le="16384.0"} 0.0
host_memory_bytes_bucket{component="system-metrics",mode="free",le="65536.0"} 0.0
host_memory_bytes_bucket{component="system-metrics",mode="free",le="524288.0"} 0.0
host_memory_bytes_bucket{component="system-metrics",mode="free",le="1048576.0"} 0.0
host_memory_bytes_bucket{component="system-metrics",mode="free",le="+Inf"} 67.0
host_memory_bytes_count{component="system-metrics",mode="free"} 67.0
host_memory_bytes_sum{component="system-metrics",mode="free"} 530864668672.0
```  

The first thing to notice in the above is the repetition of entries, these are ranges of the histogram, the line
`host_memory_bytes_bucket{component="system-metrics",mode="free",le="512.0"} 0.0` can be read as **Within the specified time window, 
there is no entry (`0.0`) of free (`mode="free"`) memory less (`le="512.0"`) than 512 bytes (`...ory_bytes_buc...`)**