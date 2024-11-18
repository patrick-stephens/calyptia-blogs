---
title: "Calyptia Core adds support for Redpanda"
date: "2024-01-02"
description: "Calyptia Core now supports Redpanda as a destination for high-volume streaming data pipelines. Learn how easy it is to create data pipelines with Core."
image: "https://www.datocms-assets.com/97087/1704227337-core-redpanda-social.png?auto=format&fit=max&w=1200"
author: "Erik Bledsoe"
canonicalUrl: "https://calyptia.com/blog/calyptia-core-adds-support-for-redpanda"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/calyptia-core-adds-support-for-redpanda). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

Calyptia Core now supports Redpanda as a destination for high-volume streaming data pipelines. Calyptia Core enables users to seamlessly manage their telemetry data from source to destination.

The new support for Redpanda adds to the [dozens of other integrations](https://calyptia.com/integrations) already available as either sources or destinations available in Calyptia Core.

[Redpanda](https://redpanda.com/) is a simple, powerful, and cost-efficient streaming data platform that is compatible with Kafka APIs. Sometimes referred to as a Kafka clone written in C++, Redpanda offers performance improvements and eliminates much of the complexity of Kafka. 

In addition to support for Redpanda, Core also provides integrations for Kafka as well as for Confluent, a commercial version of the open-source Kafka from the project’s creators. 

Here’s a quick overview of how to integrate Redpanda into your data pipeline with Calyptia Core.

## Getting Started

This assumes that you already have a Redpanda account and a running cluster and familiarity with the  [Redpanda Console](https://redpanda.com/redpanda-console-kafka-ui). If not, Redpanda offers [a free trial](https://redpanda.com/try-redpanda) or you can [spin up a Redpanda cluster in Docker](https://docs.redpanda.com/current/get-started/quick-start/).

Likewise, we assume you already have a Calyptia Core account. If not, we also [offer a free trial](https://calyptia.com/free-trial) as well as a [Docker option for quick evaluation](https://docs.calyptia.com/docs/v/core-for-docker-desktop/overview/readme). 

Gather the following from your Redpanda account:

* Your Redpanda broker(s) address(es) and port number(s)
* Account username and password
* Topic(s) to which you want to send data

## Creating Your Pipeline in Core

From within your Calyptia Core interface, navigate to your running Core instance, select the option to create a new pipeline, and give it a name

![Screen capture showing the steps for creating a new pipeline](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1704219146-redpanda-blog-1.png&w=3840&q=75)Select your data source. For the purposes of this exercise, we’ll be using the Mock Data plugin, but you, of course, could also use any of the dozens of other [data sources that Calyptia supports](https://calyptia.com/integrations).

![screen capture showing how to select a data source](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1704219227-redpanda-blog-2.png&w=3840&q=75)Here is a sample of the mock data that we will use:


```json
{"registertime":1500599586519,"userid":"User_9","regionid":"Region_5","gender":"MALE"} 
{"registertime":1493882456812,"userid":"User_9","regionid":"Region_3","gender":"OTHER"} 
{"registertime":1514584804940,"userid":"User_9","regionid":"Region_8","gender":"FEMALE"} 
{"registertime":1498613454415,"userid":"User_7","regionid":"Region_9","gender":"FEMALE"} 
{"registertime":1510970841590,"userid":"User_8","regionid":"Region_8","gender":"OTHER"}
```
With your data source now defined, select Redpanda as your destination. 

You’ll need to provide your Redpanda broker address and port number. Core supports adding multiple brokers separated by commas, but for this exercise, we’ll be using a single instance. 

Provide the topic or topics to which you want to send your data. **Note:** the topic must already exist in your Redpanda cluster. Core does not create topics. 

Select the format of your data (either json or msgpack). We are using json. 

The security protocol and SASL mechanism are set to the Redpanda defaults, but if your cluster differs you’ll need to adjust accordingly. 

Finally, provide the username and password that Core will use to connect to your Redpanda cluster. 

![Setting up Redpanda as the data destination](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1704219473-redpanda-blog-3.png&w=3840&q=75)After you save and deploy your pipeline in Calyptia Core, switch to your Redpanda console and open the message topic you created and you’ll see messages flowing in. 

![Screen capture of Redpanda console shoing data flowing in](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1704219524-redpanda-blog-4.png&w=3840&q=75)## Next Steps: Discover How Core can Transform your Data Pipelines

Of course, Calyptia Core can do much more than just collect and route data (although, as you‘ve seen Core makes it really easy to do so). You could also apply any of [dozens of processing rules](https://docs.calyptia.com/calyptia-core/custom-pipelines/processing-rules) that allow you to act upon and transform your data in flight before they are sent to their destination. 

Calyptia’s built-in processing rules include the ability to

* Identify and deduplicate data
* [Redact sensitive information](https://calyptia.com/data-obfuscation-with-calyptia-core-and-new-relic)
* [Route data to different destinations based on its value](https://calyptia.com/blog/getting-started-with-calyptia-core-and-new-relic)
* Enrich data with new metadata
* And much more

To learn more about how Calyptia enables turnkey data collection, aggregation, transformation, and forwarding from any source to any destination, [schedule a demonstration](https://calyptia.com/demo).

  
  


