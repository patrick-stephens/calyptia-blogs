---
title: "First Mile data collection"
date: "2021-04-29"
description: "In order to gain useful insights from your telemetry data, your first interactions with it are vital. "
image: "https://www.datocms-assets.com/97087/1682098401-data-collection-featured-image.jpeg?auto=format&fit=max&w=1200"
author: "Anurag Gupta"
canonicalUrl: "https://calyptia.com/blog/first-mile-data-collection"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/first-mile-data-collection). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

When we look at data in an enterprise most users concentrate on the data backend, such as databases, data lakes, index stores, etc. This focus is well warranted as these backends are where users gain insights on data that is generated. Recently, we’ve begun to see that focus broaden to also include intermediate layers such as Apache Kafka, pub/sub, event buses, and message queues. These middle layers serve to give users more control over how data is consumed within the enterprise as well as starting to bring their own level of insights, such as Confluent’s KSQL.

The part before all the data unification and backends is how the data is collected and sent over. Data needs to be reliably collected, parsed, enriched, and sometimes transformed from a multitude of sources such as an application, infrastructure, or network devices – This is first-mile data collection.

## Why is first-mile data collection important?

Similar to logistics this is the first interaction with the data that will hopefully and eventually lead to useful insights. Messing up in this stage of the process will render all other stages, no matter what technology is used, absolutely useless. Additionally, when we look at first-mile data collection there are pieces of information present that will no longer be available further down the data pipeline. For example, if I am collecting information from an application I can enrich that data with relevant contextual information – What OS am I running on? What is my current state of CPU usage? What is the user’s current state?

## Benefits of first-mile data collection

As this is the stage where data is generated, adding more context could allow users to gain insights faster, or help pinpoint problems with higher accuracy. Data might also be useless when generated and if you remove/reduce that data in this layer you don’t need to spend the money to store, archive, or index. Another benefit is that similar to all the benefits of distributed computing you can perform certain transformations at the first-mile data collection level that spreads processing power over thousands of endpoints vs. single clusters that may already be hammered with requests. Ensuring that data is collected reliably, able to handle errors, and has the ability to enrich will make all downstream processes that much more efficient in retrieving insights.

### Summary of benefits:

* Reduce costs by eliminating useless data
* Reduce debugging time / Gain faster insights by enriching data only available at this stage in the data pipeline
* Reduce processing power by spreading small transformations to thousands of endpoints vs. single clusters

## First-mile data collection and vendor lock-in

While this might be the first time hearing about first mile data collection, odds are you already have systems in place to collect data – generally vendor-provided agents. These agents handle much of the automatic collection, parsing, enrichment, and forwarding to provided backends. However, they come with a tradeoff in that all your data is locked into specific pipelines and backends.

You may not be able to utilize an intermediate event messaging system, as the agents do not support sending data to that system. If features or changes are needed for those agents, many times you will find proprietary solutions that don’t allow you to modify code and continue to receive support.

## How do I start on First Mile Data collection?

One of the easiest paths on starting to understanding First Mile data collection is doing an inventory of all the agents you have installed on your servers. How many of them are performing proprietary actions that lock you into a data backend? How many of them are collecting non-tunable data? Are there opportunities to replace some of them with vendor-neutral open source options?

At Calyptia our focus has been the first-mile data collection layer for over 10 years and allowing you freedom of choice on your data backends or intermediate event layers. we are happy to discuss how we can use our open-source and vendor-neutral technology to help you gain an advantage in this important data pipeline layer.

