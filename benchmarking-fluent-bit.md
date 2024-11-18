---
title: "Benchmarking Fluent Bit"
date: "2022-11-08"
description: "Fluent Bit was designed for high performance with a super light footprint. But just how many messages a second can it handle? We put it to the test."
image: "https://www.datocms-assets.com/97087/1682097352-benchmarking-fluent-bit.png?auto=format&fit=max&w=1200"
author: "Erik Bledsoe"
canonicalUrl: "https://calyptia.com/blog/benchmarking-fluent-bit"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/benchmarking-fluent-bit). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

We are often asked about the performance of Fluent Bit. In particular, we are asked what performance looks like as we scale to Petabytes of data per day. In this blog, we cover a few simple test scenarios to showcase how Fluent Bit performs against different message rates. We’ve made everything available in a [public repo](https://github.com/calyptia/benchmarking) so that you can try them yourself (more on that below in the methodology section).

## The Results

Let’s get straight to the results data. If you would prefer to [read the methodology](https://calyptia.com#methodology) first, feel free to skip ahead and return here afterward. For each of the tests, we used an average payload size of 1000 bytes, and sent data to an HTTPS endpoint to simulate the performance of TLS and encryption for every record.



|  |  |  |  |  |
| --- | --- | --- | --- | --- |
| 
Msgs/s
 | 
MB/s
 | 
CPU Usage (ms)
 | 
Resident Memory Usage (MiB)
 | 
Dropped Records
 |
| 1,000 | 1 | 5.8 | 40.6 | 0 |
| 5,000 | 5 | 24.0 | 40.4 | 0 |
| 10,000 | 10 | 551.8 | 41.4 | 0 |
| 50,000 | 50 | 284.9 | 40.9 | 0 |
| 100,000 | 100 | 604.3 | 40.3 | 0 |

As you can see from the data, Fluent Bit successfully scaled 1000x without dropping a single record. Consistently throughout the testing, Fluent Bit resource utilization was minimal and particularly efficient in its use of resident memory, where the usage remained essentially consistent across all the tests. CPU usage is a cumulative measure, while resident memory usage is a point-in-time measurement. Memory usage would spike at the beginning of the test, but would rapidly settle down to the reported numbers for the duration of the test. 

## The Methodology

Note: All files necessary to run the benchmarking scenarios described above are available in our [public benchmarking repo](https://github.com/calyptia/benchmarking).

We created public Ubuntu (v.20.04) images in AWS and GCP and installed Calyptia Fluent Bit, a long-term support version of Fluent Bit, based on the open source 1.9.x series . For our tests, we used a GCP E2-highcpu-8 instance with an 8-core CPU and 8 GB of RAM.Once the VM is running, we [generate a sample log file to be processed using a script](https://github.com/calyptia/benchmarking/tree/main/standalone/test/scenarios/tail_null/scenario_helpers/data_generator). We use this generated log file


```
/test/data/input.log
```
rather than an actual log file from the VM so that we can easily control the throughput for the tests. In addition, we clone <https://github.com/calyptia/fluent-bit-devtools> and <https://github.com/fluent/fluent-bit-ci> so we can also reuse the monitoring stack provided there. The primary test script permits the user to define the rate at which the log files are processed, their size, and the duration of the test. For the benchmarks above, we ran scenarios at various rates:

* 1,000 messages per second
* 5,000 messages per second
* 10,000 messages per second
* 50,000 messages per second
* 100,000 messages per second

We ran each scenario for 5 minutes and maintained a constant log size setting of 1,000 bytes.

For each scenario, we also configured Fluent Bit to output to a TCP endpoint, an HTTPS endpoint, and to simply drop the data (NULL). To collect metrics we also make use of Fluent Bit’s Prometheus integrations and monitoring capabilities, which allow us to send directly to a Prometheus compatible endpoint.

Here, for example, is the Fluent Bit configuration used for outputting to the TCP endpoint.  



```yaml

[INPUT]
    name              tail
    path              /test/data/input.log
    Read_from_Head    true
    Refresh_Interval  1

[OUTPUT]
    Name  tcp
    Match *
    Host  localhost
    Port  5170
    Format json
    # multithreading
    workers      5

[SERVICE]
    Flush           1
    Daemon          off
    Log_Level       off

# Metrics
[INPUT]
    name            fluentbit_metrics
    tag             metrics.fb

[OUTPUT]
    Name            prometheus_remote_write
    Match           metrics.*
    # Ensure these match the monitoring stack
    tls             off
    Host            localhost
    Port            9090
    uri             /api/v1/write
    add_label       scenario fluent_bit_tail_tcp
```
## Next Steps

Over the coming weeks, you can expect to see more posts where we put Fluent Bit in head-to-head competitions with other open source agents. Our aim is to develop scenarios that allow us to compare results in a fair and unbiased way, which will require feedback and an open source repository for success.

We’ve published all of the [source code](https://github.com/calyptia/benchmarking) used to run the test so that those who wish can reproduce the tests themselves. The repo also contains the raw data from our most recent runs.

We are also currently building benchmarking tests for [Calyptia Core](https://calyptia.com/products/calyptia-core), our Kubernetes solution with a plug-and-play approach to configuring data sources and destinations that enables users to quickly and easily aggregate observability data at scale and ensures that all data is captured, processed, and routed as desired. We’ll be publishing those results soon as well.

