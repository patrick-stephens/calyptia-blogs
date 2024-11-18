---
title: "Key Observability Takeaways from KubeCon"
date: "2022-11-11"
description: "Reflections on KubeCon North America 2022 and Open Observability Day and where observability is headed from Calyptia, the creators of Fluent Bit."
image: "https://www.datocms-assets.com/97087/1682097339-kubecon-takeaways-2.png?auto=format&fit=max&w=1200"
author: "Erik Bledsoe"
canonicalUrl: "https://calyptia.com/blog/key-observability-takeaways-from-kubecon"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/key-observability-takeaways-from-kubecon). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

It has been a little over a week since we returned from KubeCon + CloudNativeCon in Detroit. Since then, we’ve spent a good amount of time thinking about the presentations we saw and the conversations we had. Below are a few of our key takeaways from KubeCon regarding Observability.  


### The demand for Observability continues to grow

Every year since 2020, someone has declared that this year will be the year of observability. Perhaps that constant refrain prompted Gartner earlier this year to announce that the 2020s are “[the decade of observability](https://calyptia.com/blog/calyptia-named-a-cool-vendor-by-gartner)” (in the same report, by the way, they declared Calyptia was a Cool Vendor in the area).

At this year’s KubeCon, Open Observability Day premiered as a co-located event, sponsored by Calyptia. More than 200 people attended, and the call for presentations received more than twice the selection committee’s expectations. The presentations were uniformly engaging and generated good discussions during the Q&A periods and during the breaks.

![Tweet from @tracypholmes — Open Observability Day is officially underway! #OpenO11yDay](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682606118-open-observability-day-tweet.png&w=3840&q=75)Recordings of all the [sessions are now posted](https://www.youtube.com/playlist?list=PLj6h78yzYM2MXp3asRZmwDIBPXfJIJn4F) if you couldn’t attend or simply want to rewatch one you saw.

Plans are already underway to make Open Observability Day a regular co-located event for both KubeCon North America and KubeCon Europe.

In addition, the sponsor exhibit hall was filled with vendors and project booths tackling various aspects of the observability challenge. In addition to our own Calyptia and Flunetd/Fluent Bit booths, there were representatives from Prometheus, OpenTelemetry, Chronosphere, Splunk, Grafana Labs (who announced a new telemetry OSS project called Grafana Faro), and literally dozens of others.

## Fluent Bit is beloved

We lost track of the number of people who stopped by the booth to say how much they loved working with Fluent Bit. Of course, given that [Fluent Bit recently surpassed three billion deployments](https://www.cncf.io/blog/2022/10/13/fluent-bit-surpasses-three-billion-downloads), that shouldn’t have been a surprise. Still, three billion is just a number (although a large one) and hearing from actual users was a bit humbling. It certainly validated the efforts we have been putting into the project and our plans to continue that investment.

People also seemed really excited about [Fluent Bit v2](https://calyptia.com/blog/press-release-fluent-bit-v2-adds-full-support-for-opentelemetry-prometheus-and-webassembly-plugins), which we announced and released during Open Observability Day. People were eager to try out the new full support for OpenTelemetry, instantly recognizing the potential advantages of having a single agent capable of collecting, processing, and routing logs, metrics, and traces.

The Fluent Bit v2 t-shirt was also a huge hit. No wonder. Check out this cool design.  


![fluent-bit-v2-shirt.jpeg](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682097346-fluent-bit-v2-shirt.jpeg&w=3840&q=75)We actually ran out of shirts to give away, even though we brought more than we thought we’d ever be able to hand out.  


## Tracing is becoming mainstream

Not too long ago, being able to include distributed trace data into your observability efforts was seen as a “wouldn’t it be nice if we could” dream. Now, traces are a regular part of many organizations’ data pipelines, and [OpenTelemetry](https://opentelemetry.io/) is how users and vendors are consuming them.

The OpenTelemetry project has set the standard for traces. That’s why we have been so hard at work ensuring that Fluent Bit v2 has full support for OpenTelemetry.  


## The open source Observability communities are vibrant

One of the most extraordinary things about KubeCon, in general, is being surrounded by so many talented people that are passionate about open source. We admit that we may be biased in this, but in our opinion, many of the most talented people are working on Observability projects.

We had so many great conversations and discussions with contributors and maintainers about synergies across projects. In the coming months, look for posts that highlight these collaborative efforts.

## Looking ahead for Calyptia and Fluent Bit

Conferences like KubeCon + CloudNativeCon provide an excellent opportunity for individuals and organizations, like Calyptia, to take stock of their current paths and perhaps make some adjustments based on what they learned while attending.

As mentioned above, plans are already underway for the next Open Observability Day, and Calyptia will once again be a sponsor and one of the key drivers and promoters of the event. Open source is vital to observability, with enterprises increasingly adopting open source solutions and commercial solutions adding support for open source projects. Events such as Open Observability Day promote learning, knowledge sharing, and community, and Calyptia is proud to participate.

The rapid growth in the adoption of Fluent Bit shows that there is a strong demand for a single agent with a small footprint that provides a vendor-neutral solution to collecting, processing, and forwarding log data. People were very intrigued by Fluent Bit v2’s addition of metrics and traces with full support for OpenTelemetry, but understandably had questions about the apparent overlap with the OpenTelemetry Collector. You can count on seeing future posts from us that address those questions (spoiler alert: we complement OpenTel, we don’t compete with it).  


