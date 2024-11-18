---
title: "The Fluent Ecosystem"
date: "2022-06-02"
description: "More than just Fluentd and Fluent Bit, the Fluent Ecosystem is a philosophy that the enterprise pipeline should be open source and vendor neutral."
image: "https://www.datocms-assets.com/97087/1682097924-1blog-post.jpg?auto=format&fit=max&w=1200"
author: "Eduardo Silva"
canonicalUrl: "https://calyptia.com/blog/the-fluent-ecosystem"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/the-fluent-ecosystem). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

*This is the first of a series of posts based upon my keynote presentation “Fluent Bit in 2022: Logs, Prometheus and OpenTelemetry” at FluentCon 2022. In this first post, we look at how the original Fluentd project evolved into a full ecosystem of observability solutions, still firmly grounded in open source.*

The Fluentd project started more than 10 years ago to solve a very specifc problem: how to collect and move a large amount of data. We never intended Fluentd to be a drop-in replacement for other data collectors. Instead, we envisioned it as something that you could plug into your existing architecture and solve your problems related to moving data from one point to another.

Now, ten years later Fluentd is still moving data in thousands of systems, But the Fluent ecosystem has grown and evolved. The Fluent ecosystem is more than a single tool—more than just Fluentd. It’s more than Fluentd plus Fluent Bit. It’s a concept that you should be able to integrate with different systems, different architectures, and other projects. Pretty much from the beginning, we identified the importance of *connecting with* competitors’ products, *not replacing* them.


> **The Fluent ecosystem is sort of like the Swiss Army Knife for observability.**
> 
> 

We try to connect with everything that is around us because the problem now is not just moving logs from one place to another. There are traces. There are metrics. But also you don’t want to remove your infrastructure agents. That is a major and expensive undertaking. We have users and customers who deploy 100,000 servers, So when you are talking about switching technology at that scale, it’s really expensive. It takes a lot of time, a lot of planning, and a lot of maintenance.

![the-fluent-ecosystem2.jpg](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682097930-the-fluent-ecosystem2.jpg&w=3840&q=75)So for us Fluent is a full ecosystem that allows you to get into this observability journey and start connecting the dots between different sources of information, different destinations, and different types and formats of data. We try to be data agnostic. Today it’s logs, metrics, and traces. Maybe tomorrow another kind of critical information might exist, and you can be sure that the Fluent ecosystem will support it.

Watch the full presentation from FluentCon

## Open Source is Key to Observability

I firmly believe that everything in observability must be open source. I say this not just because we are an open-source company. I say this because of the value of contributions from different vendors, companies, and individuals.

But it’s also really important that you don’t get into vendor lock-in. When you first start down the observability path, from a management perspective, you think that what you need to accomplish is data analysis, that you need to centralize all your information. But that is only part of the solution.

Our value lies in providing the tool that allows you to choose your own backend, or multiple backends, or cloud services in a smooth way. We try to be agnostic in all ways possible in terms of projects, products, and platforms.

*In my* [*next post*](https://calyptia.com/blog/fluent-bit-designed-for-cloud-and-containerized-environments)*, I’ll explore how Fluent Bit was created as a response to the era of containers and microservices.*

