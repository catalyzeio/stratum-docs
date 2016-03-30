---
title: Stratum Environment Metrics
---

# Stratum Environment Metrics
The Stratum Dashboard includes comprehensive metrics for your environment's services.

## Viewing Metrics in the Stratum Dashboard

Each service in the Stratum Dashboard contains individual graphs of metrics.

Click on the "Details" button for a service to examine them.

![view](images/view_details.png)

### Duration and Refresh
By default, the Stratum Dashboard shows the last hour of metrics for the selected service. You can change the display window by selecting a different option from the dropdown located immediately above the metric graphs.

![duration](images/metrics_duration.png)

You can also refresh the existing time intervals by clicking the "Refresh" button.

## Metrics Collected
Below are brief explanations of the metrics collected.

### CPU Usage
CPU Usage is tracked as a percentage of the amount of CPU the service has been allotted.

![cpu](images/metrics_cpu.png)

### Memory
Memory usage is tracked in terms of the number of megabytes of RAM used by the container. The memory limit of the container is clearly displayed by a red line across the top of the graph should your container approach that limit.

![memory](images/metrics_memory.png)

The minimum, maximum, and average memory use is also displayed.

### Network In/Out

Network usage is tracked by total packets and total bandwidth, for both inbound and outbound traffic.

![network](images/metrics_network.png)
