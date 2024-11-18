---
title: "The Present and Future States of Fluent Bit"
date: "2022-06-13"
description: "In the final of a three-part series, we look at the current state of Fluent Bit and ongoing efforts to expand its functionality including support for traces"
image: "https://www.datocms-assets.com/97087/1682097900-blog-post-1.jpg?auto=format&fit=max&w=1200"
author: "Eduardo Silva"
canonicalUrl: "https://calyptia.com/blog/the-present-and-future-states-of-fluent-bit"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/the-present-and-future-states-of-fluent-bit). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

*This is the third and final in a series of posts based on my presentation “Fluent Bit in 2022: Logs, Prometheus and OpenTelemetry” at FluentCon 2022. In* [*the previous post*](https://calyptia.com/blog/fluent-bit-designed-for-cloud-and-containerized-environments)*, we explored how Fluent Bit became the industry-standard log processor and forwarder for the container and microservices era. In this post, we’ll look at the current state of Fluent Bit and ongoing efforts to expand its functionality.*

The Cloud Native Computing Foundation recently announced that [Fluent Bit has surpassed one billion deployments](https://www.cncf.io/blog/2022/03/22/fluent-bit-reaches-1-billion-downloads/). It has become the industry-standard open-source engine for enterprise observability of logs and metrics, and it is included in major Kubernetes distributions, including Google Kubernetes Engine (GKE), Amazon Elastic Kubernetes Service (EKS) and Azure Kubernetes Service (AKS).

But the Fluent Bit community is working hard to improve the project, to implement new features, increase security, and optimize performance. Here’s a look at some of what’s to come.

Watch the full presentation from FluentCon

## New Fluent Bit Flavors

Just as with what happened with Linux, just as what happened with Kubernetes, we are starting to see different distributions of Fluent Bit.

AWS has long been a contributing maintainer of the Fluent Bit project. A while back they came to us and said, ”Hey, we have our own specific back-end services. And we want to provide, first-class connectors for our users by using Fluent Bit.” So we started the synergy of working together, and they created their own plugins—initially in Go and now in C. Then they said, “we need to package this for our customers.” So they are using AWS for Fluent Bit, which is just a Fluent Bit distribution that contains extra Amazon connectors

Google is also investing heavily in Fluent Bit. They are creating their own agent, which is kind of a wrapper agent that uses open telemetry for traces and uses Fluent Bit for all the logging management. So if you’re running Google Cloud, likely a Google Ops Agent will be available for you. If you deploy, for example, a Kubernetes cluster in Google Cloud, you will get Fluent Bit as a default daemon sender to collect all the logs and ship them to Stackdriver.

Cloud provider adoption is really important for the project and community base. You see that the technology has been growing and everybody is contributing back. When the major cloud providers buy into a technology, every other company—large, midsize, or small—follows the same path. And nowadays, AWS, Google Cloud, Microsoft Azure, and hundreds of companies are using Fluent technology in their own infrastructure.

![Various logos](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682097905-logos-1.png&w=3840&q=75)## Fluent Bit Security Improvements

As part of the investments from maintainers and other companies, we have been working on how to make the Fluent Bit project stronger. How do we avoid regressions? How do we increase our coverage of issues? We’ve implemented all our CICD pipeline on GitHub. So every pull request, everything that has been merged, goes through a very strict sanitization process to ensure that there are no problems and also there are no problems running on different architectures. We aim to support the four basic architectures—x86 in two flavors and the same with ARM.

As for security, Fluent Bit is written in C and there’s always this fear: what about memory safety? What about crashes? From a language perspective, there’s not much that we can do besides best practices. We enforce best practices, run memory sanitization and make sure that there are no leaks.

But we have also started working with a security company that integrated Google [Open Source Fuzz testing](https://openssf.org/) into Fluent Bit. Fuzzing is a security process where you take any component of the project that receives an input, and you send similar input, or random input, or modified input with the aim of making the project crash. You just send trash data. The first day that we ran this, we found many bugs—many, many, many bugs. In the last 12 months, all of that has been fixed.

These were not the type of bugs that you face on a daily basis. But if your service is being attacked, the attackers may be trying to exploit some of these bugs. Now they can’t.

The result is a more stable agent and a more stable product.

## Support for Logs, Metrics, and Traces

Both Fluentd and Fluent Bit were created for logs. As of last year, we started extending the scope of the project and now we support metrics, and this year we’re releasing support for traces.

Some years ago at KubeCon Europe, people began coming up to us and saying, “I’m using a Prometheus node exporter for metrics and using Fluent Bit for logs. Can you replicate the same functionality inside Fluent Bit?”

So we started extending our own core engine to support native metrics payloads. But following our philosophy of being vendor agnostic and being able to connect different platforms and ecosystems, so we made it compatible with OpenMetrics, which is a Prometheus standard, and OpenTelemetry metrics.

On the input side, we implemented a Prometheus scraper so that Fluent Bit can scrape your Prometheus endpoints. If your application is running and exposes some metrics, Fluent Bit can scrape those metrics and get them into the pipeline. Also, we have another plugin that is a mimic of Node Exporter from Prometheus that runs locally and scrapes the same metrics that Node exporter does. We support coverage of around 60% of what the real exporter does, but that covers about 85% of actual use cases.

On the output side, you can expose this data with a Prometheus Exporter plugin that we have or with Remote Write. For most of the services that are supporting Prometheus Remote Write, like New Relic and Google, you can connect to them by using these connectors. So whether you have logs or metrics, Fluent Bit can receive the data and ship it to your preferred destination.

## OpenTelemetry Support

The way that the Fluent project views OpenTelemetry is that the industry wants to get a standard. For example, if you asked about the industry standard for metrics, everybody would say Prometheus. Everybody uses Prometheus—that is perfect. If you ask, for example, what is the industry standard for traces? Everybody will say that for now, it’s OpenTelemetry.

![OpenTelemetry logo](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682097909-opentelemetry-horizontal-color.png&w=3840&q=75)But standards have a way of switching or evolving. But at some point, you have overlapping features or overlapping protocols. You need something that runs in the middle that allows you to connect the dots, and that is Fluent Bit.

Our experimental OpenTelemetry input and OpenTelemetry output plugins currently only support metrics payloads. However, we are extending to traces in a few weeks. But we don’t aim to replicate, for example, the OpenTelemetry collector; we don’t aim to implement traces processing. That is OpenTelemetry’s job. Fluent Bit’s job is to move the data.

In Q4 2022 we will launch OpenTelemetry raw traces support. The current OpenTelemetry input and output plugins will be extended to provide this feature.

## Performance Improvements and Golang Input Plugins

We are also investing heavily in performance improvements. Modern environments are always generating more data—more and more data. Fluent Bit will be able to handle that load. Amazon has contributed a new mechanism for the event loop that enables priority queues. All of the events run smoothly and there’s no event loss. We’ve also done a lot of work on optimizing threading, input support, and encoding as well as memory management manage a memory.

For some time we have supported Golang on the output side. In Q3 2022 we will extend this to the input side of Fluent Bit which will allow developers to create their own Golang input plugins for Fluent Bit.

## A Bright Future

The traction that Fluent Bit has gained in the three years since its initial release is amazing. It is exciting to be part of a community with such energy and vision. I truly believe the best years for the Fluent ecosystem are yet to come.

