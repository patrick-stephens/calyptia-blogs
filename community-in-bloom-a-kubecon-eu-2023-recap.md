---
title: "Community in Bloom: A KubeCon EU 2023 Recap"
date: "2023-04-28"
description: "Highlights from Observability Day and KubeCon EU in Amsterdam, including information on the release of Calyptia Core 2.0 featuring Fleet Management."
image: "https://www.datocms-assets.com/97087/1683057941-kubeconrecap.png?auto=format&fit=max&w=1200"
author: "McKinley Culbert"
canonicalUrl: "https://calyptia.com/blog/community-in-bloom-a-kubecon-eu-2023-recap"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/community-in-bloom-a-kubecon-eu-2023-recap). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

This past week, the Calyptia team was able to join over 10,000 attendees at Observability Day and KubeCon EU in Amsterdam. The event was filled with major players in the observability, cloud native, and computing spaces. The Calyptia co-founders, Anurag Gupta and Eduardo Silva Pereira, shared [Fluentd and Fluent Bit project updates](https://www.youtube.com/watch?v=AYQlTnGFxnQ) as well as information on [getting started with telemetry pipelines](https://www.youtube.com/watch?v=Aw_mXWkM8GI) within the Calyptia toolkit.

## Cloud Native Community Updates

Cloud nativity refers to the approach to software modernization where applications are built and run in cloud environments. Across the cloud native community it can be argued that there are three major reasons why organizations turn to cloud native approaches.  


1. Building an application in the cloud [cuts cost](https://calyptia.com/blog/reducing-the-high-cost-of-observability) that may be accrued with on-prem environments.
2. Engineers and developers alike are able to significantly reduce time spent managing and maintaining hardware associated with on-prem infrastructure.
3. The cloud native approach to software ensures high availability and uptime.

A common theme that was discussed at both Observability Day and KubeCon was the idea of cloud native maturation. What does the process of an organization transitioning to a cloud native model look like?

The [Cloud Native Computing Foundation (CNCF)](https://www.cncf.io/) put it best; key business outcomes are directly reflective of identifying an organization’s stage of cloud native maturity. Here are the [5 stages](https://github.com/cncf/cartografos/blob/main/reference/prologue.md) of the Cloud Maturity Model:

![the 5 stages of the Cloud Maturity Model: Build, Operate, Scale, Improve, Optimize](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682707956-5-stages-cloud-maturity.png&w=3840&q=75)* **Level 1: Build** You have a baseline cloud native implementation in place and are in pre-production.
* **Level 2: Operate** The cloud native foundation is established and you are moving to production.
* **Level 3: Scale** Your competency is growing and you are defining processes for scale.
* **Level 4: Improve** You are improving security, policy and governance across your environment.
* **Level 5: Optimize** You are revisiting decisions made earlier and monitoring applications and infrastructure for optimization.

The next generation of the cloud native community and its counterparts are just getting started. With the evolution of cloud native and edge computing, there has been a greater emphasis on, not only serverless computing, but also on increased use of Kubernetes and other container orchestration tools.

## Calyptia Product Updates

Amidst all of the excitement of KubeCon, Calyptia had some exciting news of its own. We introduced the [release of Calyptia Core 2.0](https://calyptia.com/blog/calyptia-core-2-0-introduces-fleet-management-and-more) featuring Fleet Management and data processing rules.

Fleet Management allows for more efficient, scalable management of Fluent Bit agents and their associated pipelines. Within this feature, users are able to apply the same rules that they may use for a single instance to a group, or fleet, of instances. Calyptia Core’s new Fleet Management tool gives users the ability to deploy the Fluent Bit agent to all of their servers consistently and reliably—no matter how large the fleet—ensuring telemetry data flows smoothly throughout your pipeline. 

![screenshot of Fleet Management screen in Calyptia Core](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682708017-fleet-management-screen-cap.png&w=3840&q=75)But wait, there’s more!

The latest iteration of Calyptia Core also introduces an extended list of processing rules, enabling users to apply custom filters to their data midstream. Whether you’re looking to enrich your data or redact personal identifying information (PII) within your datasets, it’s all available within the Calyptia dashboard.

### The Future of Observability

The [open source observability space](https://calyptia.com/blog/the-evolution-of-observability-is-open-source) is only growing. As software systems become increasingly complex, the need for effective observability tools and techniques becomes more critical. Here are a few trends and developments that will shape the future of open source observability:

* **Increased adoption of cloud-native observability tools**

As more organizations migrate their workloads to the cloud, there will likely be an increasing demand for observability tools that are specifically designed for cloud-native architectures.

* **Greater emphasis on machine learning and AI**

Machine learning and AI techniques are already being used to improve observability, and this trend is likely to continue. Machine learning algorithms can help identify patterns and anomalies in large data sets, making it easier to detect and diagnose issues.

* **Expansion of observability beyond traditional infrastructure**

With the rise of edge computing, observability will need to expand beyond traditional IT infrastructure to include compatibility with a wide range of devices and systems. This will require new tools and techniques for collecting, analyzing, and visualizing data from a diverse array of sources.

* **Increased collaboration and standardization**

As observability becomes more important, there may be a greater emphasis on collaboration and standardization across different tools and platforms. The shift in open source standards could play a key role in enabling interoperability between different observability tools.

All in all, the future of open source observability will be shaped by ongoing developments in cloud computing and machine learning. As organizations seek to gain greater visibility into their environments, they will continue to demand more sophisticated observability tools that can help them resolve issues quickly and effectively.

Want to get ahead of the game? Reach out to us at hello@calyptia.com to learn more about streamlining your observability efforts.

