---
title: "Getting Started with Calyptia Core and New Relic: Data Obfuscation"
date: "2023-05-24"
description: "A video demonstration of using Calyptia Core to gather telemetry data, process it in mid-stream, and route it to New Relic for storage and analysis."
image: "https://www.datocms-assets.com/97087/1685039409-data-obfuscation-core-new-relic.png?auto=format&fit=max&w=1200"
author: "Calyptia"
canonicalUrl: "https://calyptia.com/blog/getting-started-with-calyptia-core-and-new-relic"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/getting-started-with-calyptia-core-and-new-relic). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

In this two-part video, we demonstrate how to use [Calyptia Core](https://calyptia.com/products/calyptia-core) to create a telemetry pipeline that gathers data from a source and routes it to New Relic for storage and analysis.

In the first video, we walk through installing Calyptia Core on an EC2 in AWS. We then create a pipeline that sends data to New Relic. In addition, we demonstrate how Core’s processing rules can be used to enrich the data in mid-stream with information that would typically not be available further downstream. Finally, we demonstrate how data can also be redacted in mid-stream to prevent sensitive data from being stored where it shouldn’t.

In this second video, we demonstrate how to setup a pipeline that routes syslog data through Core and into New Relic. We also touch upon how Core mid-stream processing rules could be used to reduce the heavy volume of syslog data that is sent to New Relic for storage and analysis, potentially resulting in significantly lower costs.

