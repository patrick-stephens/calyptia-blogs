---
title: "Save 30% or More on your Splunk Bill with Calyptia Fluent Bit"
date: "2022-07-25"
description: "Splunk and Calyptia are great Companions. Calyptia can help optimize your Splunk instances while also cutting costs significantly."
image: "https://www.datocms-assets.com/97087/1682097409-calyptia3-1.jpg?auto=format&fit=max&w=1200"
author: "Timothy Stephan"
canonicalUrl: "https://calyptia.com/blog/save-30-or-more-on-your-splunk-bill-with-calyptia-fluent-bit"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/save-30-or-more-on-your-splunk-bill-with-calyptia-fluent-bit). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

## tl;dr

**As organizations move to cloud-native computing, container density and data volume keep on growing.**

* Data and cost are linearly correlated
* Data and value are **not**

**New architecture paradigms from Calyptia can reduce the costs associated with data growth.**

* Remove **noisy** data
* Route data to **cheaper storage** locations
* Redact or **remove redundant** data

[**Contact Calyptia**](https://info.calyptia.com/save-on-splunk) **to see how much you can save and what improvements we can help you make.**

## Splunk Can Become Very Expensive

The high cost of running Splunk is a common punchline in the tech community with several memes floating around the socials:

![afordable-splunk.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682097414-afordable-splunk.png&w=3840&q=75)![first-splunk-bill-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682097422-first-splunk-bill-1.png&w=3840&q=75)And my personal favorite:

![cisco-splunk-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682097428-cisco-splunk-1.png&w=3840&q=75)## Calyptia Can Help

Calyptia can help. Not only will our solution significantly reduce your Splunk costs, but it will also greatly improve your enterprise observability practice.

![meme-splunk-matrix.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682097433-meme-splunk-matrix.png&w=3840&q=75)Calyptia Fluent Bit can quickly and easily cut 30% off your Splunk bill.

And – as Fluent Bit,

1. Is vendor-agnostic, enabling you to send observability data from anywhere to anywhere, and
2. Provides you with immediate, actionable insight into your ecosystem,

it will improve your observability practice at the same time.

## Some Context to Start

Before we dive into the specifics, there are a couple things we should state up front:

1. We’re not anti-Splunk in any way. Splunk is a powerful tool that has provided a lot of value to a lot of organizations. But Splunk becomes cost prohibitive, especially as distributed, dynamic, cloud-native IT explodes the amount of data one needs to analyze to obtain a comprehensive, timely view of their ecosystem.
2. Calyptia doesn’t replace Splunk—it integrates with it (and with hundreds of other applications) to optimize your observability pipelines. It truly is one of those 1+1=3 synergy things people always talk about.

At a high level, Calyptia creates savings and introduces optimizations by implementing a strategy of [First Mile Observability](https://info.calyptia.com/first-mile-observability-whitepaper) that makes your observability pipeline more efficient and reduces Splunk costs. Let us show you how some of Calyptia’s customers have shaved 30% or more off their Splunk bill.

## Keep the Junk Out of Splunk

The most common pricing model for Splunk is based on the amount of data ingested. However, with the proliferation of cloud-native IT—with containers and IoT everywhere—the amount of data is increasing exponentially…as are the costs of storing and analyzing that data.

So it follows then that the easiest way to reduce Splunk costs is to reduce the amount of data you send to Splunk. Unfortunately, Splunk alone cannot effectively help with this issue. As Splunk requires that data be transferred into Splunk before it can be analyzed effectively, if one were using Splunk to try to understand which data was useful and which was unneeded, they would have already paid Splunk to ingest that data before they found out. So, no cost savings following that approach.

However, As Calyptia Fluent Bit enables you to understand your observability data more quickly and more comprehensively, you can immediately identify what data is essential and what is not, when and where that data is created. Non-essential or duplicative data can be routed away from Splunk to a lower cost back-end like S3 for long term storage and immediate cost savings. And there’s no risk, as If needed, that rerouted data can be replayed to Splunk at a later time.

Specifically, with Calyptia, you can pre-process your data before it is sent to Splunk and filter out irrelevant data. You could, for example, filter out messages containing “DEBUG” or those with NULL content in required fields and reroute them to lower-cost storage such as S3, or even delete them if your retention policies permit.


```
[FILTER]
    name grep
    Match *
    Exclude log *[DEBUG]*
```
## Calyptia Data Replay: Redirected <> Lost

If you later need to investigate the data that was redirected to S3 or some other storage, the data replay feature in Calyptia allows you to reproduce a data stream precisely as it would have been recorded initially.

![meme-splunk-but-wait.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682097437-meme-splunk-but-wait.png&w=3840&q=75)## Improve Your Observability Practice

Calyptia Fluent Bit can also improve your observability practice by making your data more valuable for Splunk (and for all your other observability backends).

With Calyptia you can parse, transform, and augment your observability data “in the first mile,” before sending it to Splunk, thereby reducing the volume of data ingressed as well as increasing the efficiency of your Splunk queries — and making it more valuable for your business.

Here are some ways that Calyptia Fluent Bit can help.

## Remove Irrelevant fields

Just as not all messages may be relevant for your observability efforts, even relevant messages may contain fields that are of no interest to you. Calyptia Fluent Bit can parse your messages, remove irrelevant fields, and forward the slimmed down messages to Splunk (and also send the unaltered, original messages to other storage for auditing or replaying). Having fewer fields means less ingestion, as well as less Splunk compute time indexing.  


## Turn Unstructured Data Into Structured Data

Splunk is great at bringing order to unstructured data. However, structured data is always faster and easier to search and analyze. Calyptia can parse your unstructured data, apply transformational logic, and deliver it to Splunk as fully structured data. By “shifting left” the workload of making sense of your unstructured data to Calyptia, your Splunk costs decrease.

## Convert Data Formats

Some data formats are not as efficient as others. Yes, XML, we are looking at you. And truthfully it is not your fault. You were designed as a full markup language and too often engineers treat you as only a data format. Calyptia can reformat XML data into leaner, more efficient JSON, for example, and save Splunk the effort.

## Keep Messages in Context

In an ideal world, applications would log their messages within a single line, but, in reality, applications (like stack traces) often generate multiple log messages that sometimes belong to the same context. The further down the observability pipeline you go, the more difficult it can be to recover the full context for these messages. Calyptia offers ​​multiline parsing where it identifies and concatenates multiple messages so that the context is not lost or made more difficult to recover further down the pipeline.

## Bottom Line

**Splunk and Calyptia are great companions. Calyptia can help optimize your Splunk instances while also cutting costs significantly.**

![splunk-benefits-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682097442-splunk-benefits-1.png&w=3840&q=75)## About Calyptia and Fluent Bit

Calyptia was founded by the creator of Fluent Bit, and we are the project’s primary maintainers.

With over a billion downloads, Fluent Bit is the industry-standard open-source engine for enterprise observability of logs and metrics, and it is included in major Kubernetes distributions, including Google Kubernetes Engine (GKE), Amazon Elastic Kubernetes Service (EKS), and Azure Kubernetes Service (AKS). It collects and processes event data from any source and routes that data to any destination–even multiple destinations–for storage and analysis. It is the preferred choice for cloud and containerized environments and recently surpassed one billion deployments.

Calyptia offers Fluent Bit-based services and products for the enterprise including:

* [Calyptia Core](https://calyptia.com/products/calyptia-core) — a Kubernetes solution that simplifies data collection, aggregation, and routing at scale
* Calyptia for Fluent Bit — a Long Term Support edition of Fluent Bit for enterprises that require predictable upgrades and an SLA

 

**Here’s the Call to Action**

[Contact Calyptia](https://info.calyptia.com/save-on-splunk) for a quick analysis of your environment – you’ll see how much you can save and what improvements we can help you make.

### Download the whitepaper

![splunk-whitepaper-600-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682097447-splunk-whitepaper-600-1.png&w=1200&q=75)