---
title: "The Top Open Source Tools for Observability"
date: "2023-04-04"
description: "The number of tools for observability is overwhelming. We sort through the noise and discuss the major open source projects used in observability efforts."
image: "https://www.datocms-assets.com/97087/1683057969-thetopopen.png?auto=format&fit=max&w=1200"
author: "Erik Bledsoe"
canonicalUrl: "https://calyptia.com/blog/the-top-open-source-tools-for-observability"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/the-top-open-source-tools-for-observability). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

Recently we wrote about [why the evolution of observability Is naturally migrating toward open source](https://calyptia.com/blog/the-evolution-of-observability-is-open-source). In that post, we also mentioned that there has been an explosion of open source tools that solve various problems of the puzzle that is observability.

In this post, we’ll help you sort through some of the noise as we discuss the major open source projects commonly used in observability efforts.

We give particular emphasis to those projects that are part of the Cloud Native Computing Foundation (CNCF) or at least where the projects’ sponsoring company is a CNCF member. The CNCF has established itself as the standard bearer of open source and those projects that have achieved incubating or graduated status are considered stable and are being used successfully in production. Enterprises can feel confident in adopting any open source tool that has achieved graduated or incubating status from CNCF.

## Prometheus

[Prometheus](https://prometheus.io/) is a monitoring and alerting system written in Go that collects metrics data and stores it in a time series database. It includes a powerful query language called PromQL (Prometheus Query Language) that lets users select and aggregate time series data in real-time. Prometheus is commonly used in conjunction with Grafana (see below) for visualizing the data. Alerting is handled through Prometheus Alertmanager.

Prometheus was the second CNCF-hosted project after Kubernetes, so it has been battle-tested in production environments for years. It is a standalone open source project and is maintained independently of any company.

CNCF Status: Graduated  
License: Apache 2.0  
GitHub: <https://github.com/prometheus/prometheus>

## Fluentd

[Fluentd](https://www.fluentd.org/) is a data collector written in a combination of Ruby and C that is used primarily for collecting logs from sources and sending them to desired destinations. It utilizes a plugin architecture for integrations with sources and destinations and currently has over 800 plugins available. No matter how obscure your endpoint, there is probably a Fluentd plugin for it. It also offers plugins for filtering or parsing the data before delivery.

Fluentd is another longtime project, originally released in 2011 and joining CNCF shortly after Prometheus in 2016.

CNCF Status: Graduated  
License: Apache 2.0  
GitHub: <https://github.com/fluent/fluentd>

## Fluent Bit

[Fluent Bit](https://fluentbit.io/) was originally intended to be an alternative to Fluentd. Written in C with a much smaller footprint and CPU utilization, it was designed for containerized and embedded environments. However, in the last few years, its scope has expanded significantly. In addition to logs, it can also handle metrics and trace data, making it a single telemetry pipeline agent capable of handling all the traditional three pillars of observability. It also r[ecently added eBPF capability](https://calyptia.com/blog/tracee-now-natively-supports-fluent-bit-and-fluentd) through an integration with [Aquasec’s Tracee tool](https://www.aquasec.com/products/tracee/), making it compatible with the next generation of observability data to be mined for insights.

Like Fluentd, FLuent Bit utilizes a plugin architecture for integrations with sources and destinations as well as parsing and filtering. It lacks the breadth of its older sibling’s integrations, but the current plugins cover the vast majority of production use cases. It also supports creating plugins in Go and WASM, making it much easier to develop custom plugins should the need arise. Fluent Bit’s ability to filter and parse data mid-stream exceeds Fluentd’s capabilities, and custom filters can also be written in Lua. As of Fluent Bit v2.0, it provides native support for the OpenTelemetry Protocol.

Fluent Bit is sponsored and primarily maintained by Calyptia, the company founded by the project’s creator.

CNCF Status: Graduated (under the umbrella of Fluentd)  
License: Apache 2.0  
GitHub: <https://github.com/fluent/fluent-bit>

## Jaeger

[Jaeger](https://www.jaegertracing.io/) is the final CNCF graduated project on our list. It allows developers to monitor and troubleshoot transactions in distributed systems by visualizing the chain of events in these microservice interactions. Jaeger connects data from different components to create a complete end-to-end trace.

Jaeger was created in 2015 by engineers at Uber to meet their needs for tracing on Uber microservices and was donated to CNCF in 2017. It achieved graduated status in 2019, the seventh project to do so. As of May 2022, it provides native support for the OpenTelemetry Protocol.

CNCF Status: Graduated  
License: Apache 2.0  
GitHub: <https://github.com/jaegertracing/jaeger>

## OpenTelemetry

[OpenTelemetry](https://opentelemetry.io/) has taken the open source world by storm. Formed from the merger of the OpenCensus and OpenTracing projects in 2019, it is now the [second-highest velocity project in the CNCF ecosystem](https://www.cncf.io/blog/2023/01/11/a-look-at-the-2022-velocity-of-cncf-linux-foundation-and-top-30-open-source-projects/), closely trailing Kubernetes. Its popularity is understandable given its goal of unifying tracing, metrics, and logging telemetry standards. It is, essentially, bringing law and order to what has been a wild west.

More than just standards, though, OpenTelemetry has evolved to become a collection of tools, APIs, and SDKs. Although it is still considered a CNCF incubating project, it has become so widely embraced that even commercial applications that have traditionally benefited from closed architectures are now loudly proclaiming their integrations with OTel. At this juncture, it seems safe to say that regardless of what your observability stack entails—open source, commercial products, or a mixture—it should be compatible with OpenTelemetry.

CNCF Status: Incubating  
License: Apache 2.0  
GitHub: <https://github.com/open-telemetry>

## Chaos Mesh & Litmus

[Chaos Mesh](https://chaos-mesh.org/) and [Litmus](https://litmuschaos.io/) are both CNCF projects at the incubating stage that provide chaos engineering platforms. A relatively new discipline, chaos engineering tries to break systems through controlled experiments using random and unpredictable behavior in order to collect information about the failure. Both Chaos Mesh and Litmus were admitted to the CNCF in 2020.

CNCF Status: Incubating  
License: Apache 2.0  
GitHub (Chaos Mesh): <https://github.com/chaos-mesh/chaos-mesh>  
GitHub (Litmus): <https://github.com/litmuschaos/litmus>

## Thanos and Cortex

[Thanos](https://thanos.io/) and [Cortex](https://cortexmetrics.io/) both seek to make Prometheus highly available and horizontally scalable with long-term storage. The two projects clearly share many components with Prometheus, but they take a fundamentally different approach to how these pieces are joined together. Both are CNCF projects at the incubation stage.

CNCF Status: Incubating  
License: Apache 2.0  
GitHub (Thanos): [https://github.com/thanos-io/thanos](https://github.com/cortexproject/cortex)  
GitHub (Cortex): <https://github.com/cortexproject/cortex>

## OpenSearch

[OpenSearch](https://opensearch.org/) is a fork of the very popular Elasticsearch search and analytic suite. It was created in 2021 after Elastic (the parent company of Elasticsearch) changed the project’s license from Apache 2.0 to be dual licensed under the Elastic License (their own creation) and Server Side Public License (SSPL) in a move to make it difficult for cloud companies to sell managed versions of Elasticsearch. It was a move [directly targeted at AWS](https://www.elastic.co/blog/why-license-change-aws) and followed a similar move by MongoDB a few years earlier. AWS fired back that the change meant that Elasticsearch was [no longer truly open source](https://aws.amazon.com/blogs/opensource/stepping-up-for-a-truly-open-source-elasticsearch/) and announced the fork that eventually became OpenSearch.

Like Elasticsearch, enterprises often already use OpenSearch to store and analyze their business, operational, and security data, so when they adopt an observability program many of the tools needed are already in place.

OpenSearch is not a CNCF project, although AWS is a platinum member of the CNCF.

CNCF Status: N/A  
License: Apache 2.0  
GitHub: <https://github.com/opensearch-project/OpenSearch>

## Grafana

Created in 2014, Grafana is a powerful data-visualization platform. It is commonly paired with Prometheus and allows users to create dashboards from which to monitor system performance. It utilizes a plugin system to integrate with data sources such as Prometheus or dozens of other options, open source and commercial. It began as a solution specifically for visualizing metrics, but as with so many other projects that originally focused on one pillar of the observability trio, it now supports logs, metrics, and traces.

Grafana is not a CNCF project; however, its parent company Grafana Labs is also a platinum member of the CNCF, like AWS. Grafana is also the only project to make our list that is not available under the Apache 2.0 license, instead utilizing the more restrictive Affero General Public License (GPL) v3. The move came in 2021, shortly after Elastic announced its decision to abandon Apache 2.0. In [a statement announcing the move], Grafana Labs stated that while they wanted more projection than Apache 2.0 offered, they wanted to remain with an [Open Source Initiative (OSI) approved license](https://opensource.org/licenses/) and felt that AGPLv3 offered a good compromise.

CNCF Status: N/A  
License: AGPL 3.0  
GitHub: <https://github.com/grafana/grafana>

## Building Your Observability Program

Whether you are going with only open source solutions, commercial ones, or a hybrid approach, creating an observability program is a difficult task, and there is no single solution. Hopefully, this guide has helped you to identify the major open source solutions available to you.

## How Calyptia Can Help

As the creators and maintainers of Fluent Bit, Calyptia simplifies observability with Fluent Bit-based products and services. Fluent Bit provides a vendor-neutral solution for your telemetry pipeline. With Fluent Bit as your agent, your data moves seamlessly from the systems producing it to your chosen storage and analytical backends. Whether those backends are open source or proprietary commercial solutions, to Fluent Bit it doesn’t matter. And should you decide to add or change backends, with Fluent Bit as your telemetry pipeline agent it’s a simple configuration change, no need to deploy new agents.

[Calyptia Core](https://calyptia.com/products/calyptia-core), our enterprise product, provides a click-and-drag approach to telemetry pipeline management and configuration with Fluent Bit under the hood. It lets you easily apply in-stream processing filters that eliminate noisy data, redact sensitive data, and more before the data is delivered to storage and analytic solutions. This in-flight processing reduces risk and reins in out-of-control data storage and analysis costs. Calyptia Core provides all the benefits of Fluent Bit and adds ease of management.

[Talk to us today](https://calyptia.com/contact) to learn how Calyptia can simplify your observability journey.

