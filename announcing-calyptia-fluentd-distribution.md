---
title: "Announcing Calyptia Fluentd distribution!"
date: "2021-06-11"
description: "A new distribution of Fluentd that includes more metric-focused features, along with packages optimized for performance and integration with more services. "
image: "https://www.datocms-assets.com/97087/1682537855-calyptia.jpg?auto=format&fit=max&w=1200"
author: "Calyptia"
canonicalUrl: "https://calyptia.com/blog/announcing-calyptia-fluentd-distribution"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/announcing-calyptia-fluentd-distribution). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

Today, we’re happy to announce the initial release of Calyptia Fluentd v1.0. Fluentd is an extremely popular open-source project that is a graduated project part of the cloud native computing foundation (CNCF).

Calyptia Fluentd is a new distribution of Fluentd that includes more metric-focused features, along with packages optimized for performance and integration with more services. In the future, Calyptia Fluentd will also give users simple ways to communicate with the [Calyptia Cloud](https://config.calyptia.com/) service.

## How to install?

We’ve added documentation within the official Fluentd documents how to install on the following platforms:

* [Debian GNU/Linux or Ubuntu](https://docs.fluentd.org/v/1.0/installation/install-by-deb#using-to-install-calyptia-fluentd)
* [RHEL/CentOS](https://docs.fluentd.org/v/1.0/installation/install-by-rpm#using-to-install-calyptia-fluentd)
* [macOS](https://docs.fluentd.org/v/1.0/installation/install-by-dmg#calyptia-fluentd-v1)
* [Windows](https://docs.fluentd.org/v/1.0/installation/install-by-msi#calyptia-fluentd-v1).

Supported platforms are:

![screen_shot_2021_09_08_at_7-08-14_pm_w720.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098369-screen_shot_2021_09_08_at_7-08-14_pm_w720.png&w=1920&q=75)## Key Features

### Ruby 3

Ruby 3 is the latest version of ruby that gives improvements in performance and more memory safety features compared to older ruby versions. Calyptia Fluentd packages Ruby 3.0 in the package by default and brings all the benefits to default plugins such as tail, filtering, and others.

### Systemd (Linux)

Calyptia-Fluentd supports systemd management on Linux platforms.

#### start


```bash
$ sudo systemctl start calyptia-fluentd.service
```
#### stop


```bash
$ sudo systemctl stop calyptia-fluentd.service
```
### Launchctl (macOS)

Calyptia-Fluentd supports launchctl management on macOS.

#### start


```bash
$ sudo launchctl load /Library/LaunchDaemons/calyptia-fluentd.plist
```
#### stop


```bash
$ sudo launchctl unload /Library/LaunchDaemons/calyptia-fluentd.plist
```
### Windows Service (Windows)

Calyptia-Fluentd also integrates as a Windows Service on Windows. Note that the following commands request Administrator privileges.

#### start

##### using net.exe


```bash
> net start fluentdwinsvc
```
##### PowerShell Cmdlet


```bash
PS> Start-Service fluentdwinsvc
```
#### stop

##### using net.exe


```bash
> net stop fluentdwinsvc
```
##### PowerShell Cmdlet


```bash
PS> Stop-Service fluentdwinsvc
```
### Plugin Management

Calyptia-Fluentd provides calyptia-fluentd-gem command to manage plugins.

For example, for users who want to install fluent-plugin-mongo:


```bash
$ sudo calyptia-fluentd-gem install fluent-plugin-mongo
```
## Performance

Performance is extremely important to Fluentd users, and Calyptia Fluentd is no different. We’ve added new versions of ruby that include performance benefits and are always looking at ways we can optimize for major use cases. We’ve also included benchmarks for Calyptia Fluentd below in the following scenarios:

* tailing a flat file(in\_tail)
* consuming syslog (in\_syslog)
* consuming Windows EventLog (in\_windows\_eventlog2)

## Tailing a Flat File — Benchmark scenario for in\_tail

### Environment

* Forwarder | CentOS 8 on AWS t2.medium instance
* Aggregator | CentOS 8 on AWS t2.medium instance

### Scenario

increase generating lines rate step by step

* baseline(0 line/sec)
* 500 lines/sec
* 1000 lines/sec
* 2000 lines/sec
* 5000 lines/sec

Generate logs with [dummer](https://github.com/sonots/dummer) which is ltsv format

Dummer generates ltsv format lines:


```
configure 'sample' do
  output "message.log"
  delimiter "\t"
  labeled true
  field :id, type: :integer, countup: true, format: "%04d"
  field :time, type: :datetime, format: "[%Y-%m-%d %H:%M:%S]", random: false
  field :level, type: :string, any: %w[DEBUG INFO WARN ERROR]
  field :method, type: :string, any: %w[GET POST PUT]
  field :uri, type: :string, any: %w[/api/v1/people /api/v1/textdata]
  field :reqtime, type: :float, range: 0.1..5.0
  field :foobar, type: :string, length: 8
end
```
### Worker CPU Usages

![worker_cpu_usage_w720.jpg](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098373-worker_cpu_usage_w720.jpg&w=1920&q=75)In higher traffic environments we see Calyptia Fluentd using lower CPU time consumption compared to other distributions.

### Worker Memory Usages (RSS)

![worker_memory_usafe_w720.jpg](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098378-worker_memory_usafe_w720.jpg&w=1920&q=75)## Consuming Syslog — Benchmark scenario for in\_syslog

### Environment

* Forwarder | CentOS 8 on AWS t2.medium instance
* Aggregator | CentOS 8 on AWS t2.medium instance

### Scenario

Increase generating lines rate step by step

* baseline(0 line/sec)
* 500 messages/sec
* 1000 messages/sec
* 1500 messages/sec

Generate syslog message with loggen

### Worker CPU Usages

![worker_syslog_w720.jpg](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098382-worker_syslog_w720.jpg&w=1920&q=75)### Worker Memory Usages (RSS)

![worker_syslog_mem_w720.jpg](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098387-worker_syslog_mem_w720.jpg&w=1920&q=75)Calyptia Fluentd RSS memory usage is lower compared to other distributions with the same configuration.

## Consuming Windows EventLog — Benchmark scenario for in\_windows\_eventlog2

### Environment

* Collector | Windows Server 2019 on AWS t2.medium instance
* [Benchmark tool](https://github.com/fluent-plugins-nursery/EventLogBencher) written in C#
* Aggregator | Ubuntu Focal (20.04 LTS) on AWS t2.medium instance

### Scenario

increase generating Windows events size step by step

* 512 bytes 120000 events total
* 1024 bytes 120000 events total
* 2048 bytes 120000 events total

monitoring Ruby processes with [typeperf](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/typeperf) during about 18 minutes

### Worker CPU Usages

![worker_memory_w720.jpg](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098391-worker_memory_w720.jpg&w=1920&q=75)### Worker Memory Usages (Working Set)

![worker_windows_memory_w720.jpg](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098396-worker_windows_memory_w720.jpg&w=1920&q=75)## How do I get started?

You can start by downloading Calyptia-Fluentd from [Calyptia Fluentd download](https://calyptia-fluentd.s3.us-east-2.amazonaws.com/index.html?prefix=1/), and start building configuration in [our visualization tool (sign-up required)](https://config.calyptia.com/).

