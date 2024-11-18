---
title: "Logging Deep Dive and Best Practices"
date: "2023-11-30"
description: "In this video from KubeCon, the Calyptia team discusses the challenges of logging and then demonstrates best practices and monitoring and operations with Fluent Bit"
image: "https://www.datocms-assets.com/97087/1701370253-logging-deep-dive-social.png?auto=format&fit=max&w=1200"
author: "Eduardo Silva, Anurag Gupta, José Lecaros"
canonicalUrl: "https://calyptia.com/blog/logging-deep-dive-and-best-practices"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/logging-deep-dive-and-best-practices). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

The following video was recorded at KubeCon + CloudNativeCon North America in November 2023. We are grateful to the Cloud Native Computing Foundation for allowing us to post it here.

## Synopsis

### The Challenges of Logging

Log collection and analysis is an essential part of understanding what is going on with our systems. However, logging brings challenges:

* Log formats differ across systems and applications
* Logs can be collected in different ways, including from file systems, APIs, and network endpoints
* The sheer volume of logs produced can be difficult to manage

This is where Fluent Bit comes in. Fluent Bit is a unified solution for collecting, processing, and routing telemetry data. In addition to logs, Fluent Bit can also handle metrics and traces. 

### Logging Best Practices

In this section, Anurag Gupta demonstrates how Fluent Bit can be used to solve various problems, including

* Enriching logs with additional information, such as hostname
* Keeping debug and trace logs out of expensive storage and analytical backends
* Redacting or removing sensitive data such as credit card numbers
* Converting logs to metrics for easier monitoring on dashboards and reporting

### Monitoring and Operations

In the final section of the presentation, Jose Lecaros demonstrates various ways to monitor your Fluent Bit data pipelines and troubleshoot potential issues including:

* Using Grafana to visualize the metrics that Fluent Bit generates about its own health and status
* Identifying issues where your destination endpoint cannot handle the current volume and how Fluent Bit provides both in-memory and file system buffering to reduce back pressure and ensure that no data is lost.

## Learn more about Fluent Bit

* **Join the Fluent Slack Community:** To learn more about Fluent Bit, we recommend joining the [Fluent Community Slack channel](https://launchpass.com/fluent-all) where you will find thousands of other Fluent Bit users.
* **Check Out Fluent Bit On-Demand Webinars:** Our free [on-demand webinars](https://calyptia.com/events) that range from introductory sessions to deep dives into advanced features.

  


