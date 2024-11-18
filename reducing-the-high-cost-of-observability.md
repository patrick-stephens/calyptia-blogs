---
title: "Reducing the High Cost of Observability"
date: "2023-02-16"
description: "Observability budgets cannot keep pace with the volume of telemetry data. Here are some tips for reducing the cost of your observability program."
image: "https://www.datocms-assets.com/97087/1681305634-featured-reducing-costs.png?auto=format&fit=max&w=1200"
author: "Erik Bledsoe"
canonicalUrl: "https://calyptia.com/blog/reducing-the-high-cost-of-observability"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/reducing-the-high-cost-of-observability). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

Implementing an observability program is not a trivial task, but organizations that do so can reap great rewards. Observability provides real-time, actionable insights into the complex, multilayered, distributed IT systems that form modern applications. It reduces MTTR, improves productivity and efficiency, and helps create a better customer experience. But observability also typically comes with a high financial cost.

The cost is a direct result of the incredibly high volume of observability data that must be processed, stored, and analyzed to produce those actionable insights. In a recent survey of senior engineering professionals responsible for observability and log data management at their companies, two-thirds of the respondents [reported a six-figure annual budget for their observability tools](https://venturebeat.com/business/report-74-of-orgs-struggle-to-achieve-true-observability/). The volume of data being generated is increasing exponentially, but observability budgets cannot keep pace.

In this post, we’ll share some tips for reducing the cost of your observability program.

## Keep the Garbage Out

Garbage in, garbage out. Not all observability data is golden. A large portion of it is useless for providing the holy grail of actionable insights. Since observability platforms often charge by the amount of data ingested, simply not sending as much data can significantly reduce your spend. Long gone are the days when it made sense to “send everything to Splunk.”

## Know your Telemetry Data

Of course, how do you know which data is garbage? If your observability program is mature, you should be able to determine what types of data have provided useful information in the past and what has not. If you are uncertain, or if your observability program is immature, there are a growing number of third-party consultants who can assist you with understanding what data you can safely omit.

## Utilize Lower Cost Storage

If you are uncomfortable simply deleting data that you “think” is garbage, you can take advantage of lower-cost storage options. Send it to S3 Glacier to cool for a while until you (and your audit policies) are comfortable disposing of it permanently. If you discover that you need to examine the data you have in cold storage you can always import it into your analytical backend at that time.

## Set Data Retention Policies

Your policies will be dependent upon various internal and external rules and regulations about how to handle data, of course. But operating within those confines, you should aggressively expire data whenever possible. With modern cloud-based applications being often updated multiple times per day, the useful lifespan of telemetry data decreases. Are you really going to find much value in application log data from six months ago when there have been thousands of pull requests merged since then?

## Compress and Aggregate

Regardless of where you are storing your data and for how long, you should compress and aggregate it whenever possible. In addition to decreasing the storage required, you may also find that many queries actually execute faster.

## Choose the Right Tools

If you are going to implement the suggestions presented so far, you must be able to process data in-stream as it flows through your telemetry pipeline. You have to be able to automate decisions about whether to send data to one location or another (or both). If your data-gathering agents are incapable of applying logic to the flow or of sending data to any destination you desire, then that is not the right tool for you.

Be especially careful about vendor lock-in. Observability is complex and it can be tempting to try to simplify it by going with a platform that promises an all-in-one solution. However, history has shown that such solutions often make it very difficult to escape their grasp as their renewal fees increase over time.

## Watch Out for Tool Sprawl

Your observability program will almost certainly consist of a stack rather than a single solution, and that is okay. However, observability is [particularly prone to tool sprawl](https://calyptia.com/blog/avoiding-tool-sprawl-in-your-observability-stack), which can increase costs and decrease efficiency. Choose the right tools, yes, but not ALL the tools.

## Support Open Standards

Ensure that whatever tools you choose support standards such as OpenTelemetry, which is rapidly emerging as the standard for handling telemetry data. Doing so ensures that your data can flow to a variety of backends smoothly and helps you avoid vendor lock-in.

## Conclusion

If the volume of data gathered and the number and value of insights generated were linearly correlated, it would perhaps be easy to justify the astronomical increase in observability costs that most organizations are experiencing. Alas, that is not the case. 

The insights still have value for the business, but without careful control of your observability spend  the ROI decreases. Observability is not easy, nor is controlling its cost. Hopefully, the tips provided here will help you reign in the costs and increase your ROI.

## How Calyptia Can Help Reduce Your Observability Costs

As creators and maintainers of the open source [Fluent Bit project](https://fluentbit.io/)–the industry standard logging and metrics processor and forwarder (with [billions of deployments](https://www.cncf.io/blog/2022/10/13/fluent-bit-surpasses-three-billion-downloads/))–Calyptia is dedicated to providing vendor-neutral products and services to support and empower your observability practice. As of v2.0, [Fluent Bit is fully compatible with OpenTelemetry](https://www.businesswire.com/news/home/20221024005033/en/Fluent-Bit-v2-Adds-Full-Support-for-OpenTelemetry-Prometheus-and-WebAssembly-Plugins).

[**Calyptia Core**](https://calyptia.com/products/calyptia-core)—our enterprise product—is powered by Fluent Bit and provides a click-and-drag interface for configuring and managing your telemetry pipelines. It enables any organization to quickly aggregate observability data at scale and to ensure that all data is captured, processed, and routed as desired using a single agent (Fluent Bit) for all backends.

From any source and to any destination, Calyptia and Fluent Bit ensure your data are where you need them to be. [Schedule a meeting with us](https://calyptia.com/contact) to scope the potential cost savings of data filtering for your organization.

