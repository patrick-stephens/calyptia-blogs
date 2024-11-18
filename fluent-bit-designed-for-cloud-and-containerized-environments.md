---
title: "Fluent Bit: Designed for Cloud and Containerized Environments"
date: "2022-06-08"
description: "Emerging from the Fluentd project, Fluent Bit was built for containers and microservices with high performance and a small footprint."
image: "https://www.datocms-assets.com/97087/1682097914-blog-post.jpg?auto=format&fit=max&w=1200"
author: "Eduardo Silva"
canonicalUrl: "https://calyptia.com/blog/fluent-bit-designed-for-cloud-and-containerized-environments"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/fluent-bit-designed-for-cloud-and-containerized-environments). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

*This is the second of a series of posts based on my keynote presentation “Fluent Bit in 2022: Logs, Prometheus and OpenTelemetry” at FluentCon 2022. In* [*the first post*](https://calyptia.com/blog/the-fluent-ecosystem)*, we looked at how the original Fluentd project evolved into a full ecosystem firmly grounded in open source. In this post, we’ll examine why Fluent Bit is a better solution for containers and microservices and the challenges facing developers in this era.*

Ten years ago when Fluentd was created the problem was how to move log information to a cloud service. And of course, Fluentd solves that problem.

It didn’t take too long before Fluentd was running in production on thousands of servers. We quickly realized that we needed another solution that was lighter weight, more performant, and with a lower memory footprint. As we were creating Fluentd, our target was embedded Linux. But then containers exploded, and then came Kubernetes.

And so Fluent Bit was created as a better solution for this era of microservices and containers. Why is it better? Because it was written in C. It was optimized to consume low CPU and low memory, but also to have a very high throughput when it’s moving data from one place to the other. So naturally, users started migrating from one project to the other.

Initially, the intent was to have Fluent Bit function as a forwarder, just taking data from the source and sending it out to Fluentd which would act as the aggregator and take care of all the processing. But our users started asking for more features in Fluent Bit. For example, we didn’t have filtering at the beginning. We didn’t support Kubernetes. One of the first plugins available for Fluent Bit was built to gather kernel log messages, CPU metrics, and memory metrics—those plugins are still there.

Watch the full presentation from FluentCon

## Moving from Fluentd to Fluent Bit

The trend now is that users are switching from Fluentd to Fluent Bit because they’re following a journey where they’re moving to Kubernetes or they’re moving to a different kind of architecture.

And now the problem of moving data is larger than it was before. Five years ago people were processing a few terabytes per month, maybe. Sometimes now it is terabytes per day. Every year, every company and every architecture is generating more data.

If you’re generating more data your agent and other tools in your infrastructure must be able to scale. Fluentd was not designed for the kind of workload necessary in this new era of containers

![fluentd-to-fluent-bit.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682097919-fluentd-to-fluent-bit.png&w=3840&q=75)Fluent Bit, however, was designed for exactly that. And that’s why we’re seeing this pattern of Fluentd users moving to Fluent Bit.

## The Challenge for Maintainers

One of the strengths Fluent Bit is that in a production-grade system it is very high performing but consumes very few resources. Of course, it’s not the same to process one thousand messages per second as it is to process ten to twenty thousand. We are going to need more computing resources for that.

Our challenge as maintainers and developers is that every year we need to optimize more. We need to find how to reduce memory allocation, what kind of trading strategy we can use, how we can optimize before moving the data, and even how we optimize when it’s about to encode the data into JSON, which is a really expensive task. And as you might guess, every destination waits for a JSON payload. So no matter if in the agent we have a binary representation of the data, we need to convert that back to JSON.

Now talking as a maintainer, and as a founder of a company, we are investing in both projects. Fluentd may be around for another ten years. Fluent Bit, maybe longer, but most of the innovation and the momentum is happening more on the Fluent Bit side.

## One Billion Deployments and Still Growing

We are seeing great traction on the number of Docker Hub pulls Fluent Bit is receiving. In 2019 when we started publishing, we received a few million. In 2020, that jumped to over 200 million. In 2021, that tripled to over 600 million. We’re not even half way through 2022 yet, but we are already approaching that number again. Actually, CNCF just reported that [Fluent Bit has been deployed more than a billion times](https://www.cncf.io/blog/2022/03/22/fluent-bit-reaches-1-billion-downloads/). That is insane. It’s insane because it’s a huge adoption. And it’s still accelerating.

As adoption grows, you get more bug reports, more enhancement requests. It’s like a snowball that continues growing. And it’s really interesting because there are more challenges. You might think that an agent that solves a basic data problem wouldn’t change all that much, but actually, every year new challenges emerge, more stuff that we need to do or innovate into this space.

*In the next and final post of the series, I’ll look at recent Fluent Bit innovations and exciting changes to come.*

