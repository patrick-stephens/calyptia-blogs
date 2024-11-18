---
title: "Configure, Manage, and Simplify Your Observability Data Pipelines with the Calyptia Core Docker Extension"
date: "2022-12-16"
description: "The new Calyptia Core Docker Extension lets you build and manage observability pipelines within Docker Desktop. Here's how to do it."
image: "https://www.datocms-assets.com/97087/1681306875-featured-install-docker.webp?auto=format&fit=max&w=1200"
author: "Eduardo Silva"
canonicalUrl: "https://calyptia.com/blog/calyptia-core-docker-extension"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/calyptia-core-docker-extension). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

*This post was co-written with*[*Ajeet Singh Raina*](https://www.docker.com/author/ajeet-singh-raina/ "Posts by Ajeet Singh Raina")*, DevRel Manager at Docker. It also appears on the*[*Docker Blog*](https://www.docker.com/blog/manage-observability-data-pipelines-with-calyptia-core-docker-extension/)*.*

Applications produce a lot of observability data. And it can be a constant struggle to source, ingest, filter, and output that data to different systems. Managing these observability data pipelines is essential for being able to leverage your data and quickly gain actionable insights.

In cloud and containerized environments, Fluent Bit is a popular choice for marshaling data across cloud-native environments. A super fast, lightweight, and highly scalable logging and metrics processor and forwarder, it recently reached three billion downloads.

Calyptia Core, from the creators of Fluent Bit, further simplifies the data collection process with a powerful processing engine. Calyptia Core lets you create custom observability data pipelines and take control of your data.

And with the new Calyptia Core Docker Extension, you can build and manage observability pipelines within Docker Desktop. Let’s take a look at how it works!

![architectural illustration](/_next/image?url=https%3A%2F%2Fwww.datocms-assets.com%2F97087%2F1681307259-calyptia-core-diagram-png.webp&w=3840&q=75)### What is Calyptia Core?

Calyptia Core plugs into your existing observability and security infrastructure to help you process large amounts of logs, metrics, security, and event data. With Calyptia Core, you can:

* Connect common sources to the major destinations (e.g. Splunk, Datadog, Elasticsearch, etc.)
* Process 100k events per second per replicas with efficient routing.  
Automatically collect data from Kubernetes and its various flavors (GKE, EKS, AKS, OpenShift, Tanzu, etc).
* Build reliability into your data pipeline at scale to debug data issues.

### Why Calyptia Core?

Observability as a concept is common in the day-to-day life of engineers. But the different data standards, data schemas, storage backends, and dev stacks contribute to tool fatigue, resulting in lower developer productivity and increased total cost of ownership.

Calyptia Core aims to simplify the process of building an observability pipeline. You can also augment the streaming observability data to add custom markers and discard or mask unneeded fields.

### Why run Calyptia Core as a Docker Extension?

Docker Extensions help you build and integrate software applications into your daily workflows. With Calyptia Core as a Docker Extension, you now have an easier, faster way to deploy Calyptia Core.

Once the extension is installed and started, you’ll have a running Calyptia core. This allows you to easily define and manage your observability pipelines and concentrate on what matters most — discovering actionable insights from the data.

### Getting started with Calyptia Core

Calyptia Core is in [Docker Extension Marketplace](https://hub.docker.com/extensions/calyptia/core-docker-desktop). In the tutorial below, we’ll install Calyptia Core in Docker Desktop, build a data pipeline with mock data, and visualize it with Vivo.

### Initial setup

Make sure you’ve installed the latest version of Docker Desktop (or at least v4.8+). You’ll also need to enable Kubernetes under the Preferences tab. This will start a Kubernetes single-node cluster when starting Docker Desktop.

![Screen capture](/_next/image?url=https%3A%2F%2Fwww.datocms-assets.com%2F97087%2F1681307463-enable-kubernetes-png.webp&w=3840&q=75)### Installing the Calyptia Core Docker Extension

#### Step 1

Open Docker Desktop and click “Add Extensions” under Extensions to go to the Docker Extension Marketplace.

![screen capture](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1681308950-docker-desktop-all-extensions-png.png&w=3840&q=75)#### Step 2

Install the Calyptia Core Docker Extension.

![Extensions marketplace screen capture](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1681309011-docker-desktop-extensions-marketplace-png.png&w=3840&q=75)By clicking on the details, you can see what containers or binaries are pulled during installation.

![extension installation screen capture](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1681309069-install-calyptia-core-extension-png.png&w=3840&q=75)#### Step 3

Once the extension is installed, you’re ready to deploy Calyptia Core! Select “Deploy Core” and you’ll be asked to login and authenticate the token for the Docker Extension.

![welcome screen](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1681309135-calytia-core-extension-welcome-png.png&w=3840&q=75)In your browser, you’ll see a message asking to confirm the device.

![device confirmation screen](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1681309180-calyptia-core-device-confirmation-png.jpeg&w=3840&q=75)![all set screen](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1681309231-clyptia-device-confirmed-png.jpeg&w=1920&q=75)#### Step 4

After confirming, Calyptia Core will be deployed. You can now select “Manage Core” to build, configure, and manage your data pipelines.

![Core desktop extension screen](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1681309276-calyptia-core-extension-manage-core-png.png&w=3840&q=75)You’ll be taken to `core.calyptia.com`, where you can build your custom observability data pipelines from a host of source and destination connectors.

![screenshot](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1681309326-calyptia-core-manage-core-png.png&w=3840&q=75)#### Step 5

In this tutorial, let’s create a new pipeline and set `docker-extension` as the name.

![screenshot](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1681309376-calyptia-core-set-pipeline-name-png.jpeg&w=3840&q=75)Add “Mock Data” as a `source` and “Vivo” as the `destination`.

*NOTE: Vivo is a real time data viewer embedded in the Calyptia Core Docker Extension. You can make changes to the data pipelines like adding new fields or connectors and view the streaming observability data from Vivo in the Docker Extension.*

![screenshot](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1681309408-select-calyptia-core-source-png.png&w=3840&q=75)![screenshot](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1681309454-select-calyptia-core-destination-png.png&w=3840&q=75)#### Step 6

Hit “Save & Deploy” to create the pipeline in the Docker Desktop environment.

![screenshot](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1681309506-calyptia-core-pipeline-png.png&w=3840&q=75)With the Vivo Live Data Viewer, you can view the data without leaving Docker Desktop.

![screenshot](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1681309535-calyptia-core-vivo-data-viewer-png.png&w=3840&q=75)### Conclusion

The Calyptia Core Docker Extension makes it simple to manage and deploy observability pipelines without leaving the Docker Desktop developer environment. And that’s just the beginning. You can also use automated logging in Calyptia Core for automated data collection from your Kubernetes pods and use metadata  to perform processing rules before it’s delivered to the chosen destination.

Give the [Calyptia Core Docker Extension](https://hub.docker.com/extensions/calyptia/core-docker-desktop) a try, and let us know what you think at hello@calyptia.com.

