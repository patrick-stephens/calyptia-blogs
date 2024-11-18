---
title: "Handling Multiline Logs with Fluent Bit: Configuration and Best Practices"
date: "2023-03-30"
description: "Without proper handling, multiline logs can be difficult to read and interpret. Learn how to handle them properly using Fluent Bit."
image: "https://www.datocms-assets.com/97087/1681324944-featured-multiline.png?auto=format&fit=max&w=1200"
author: "Sudhanshu Prajapati"
canonicalUrl: "https://calyptia.com/blog/handling-multiline-logs-with-fluent-bit-configuration-and-best-practices"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/handling-multiline-logs-with-fluent-bit-configuration-and-best-practices). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

While it would be ideal for applications to log their messages in a single line, in reality, they often generate multiple log messages related to the same context. This complicates processing the file, especially in cases like application stack traces where multiline logs are common.

Without proper handling, multiline logs can be difficult to read and interpret. Unfortunately, many log processors do not handle multiline logs very well.

## How does Fluent Bit resolve problems with sending multiline logs?

Fluent Bit addresses this problem by providing a built-in multiline parser and a configurable multiline parser that can handle a variety of log formats. By using a multiline parser, Fluent Bit can group related log lines into a single event, making it easier to read and analyze.

In addition to multiline parsing, Fluent Bit also provides filtering capabilities that can be used to remove irrelevant logs and enrich logs with additional information. For example, you can filter logs based on their severity level, remove sensitive information, or add additional fields to provide context.

## Fluent Bit Multiline Parsing

The multiline parser engine provides two ways to utilize it.

* Built-in multiline parser
* Configurable Multiline parser.

Built-in Multiline Parsing Built-in parsers such as Java, Python, and Go are readily available in Fluent Bit without the need for additional configuration. They are designed to handle specific cases of multiline parsing.

#### Example Configuration


```yaml
[INPUT]
	Name          	tail
	Path          	test.log
	multiline.parser java
```
### Multiline Parsers

In addition to the aforementioned built-in parsers, it is possible to create custom Multiline parsers with specific rules through configuration files. To define a Multiline parser, a unique name and a type, along with other associated properties, must be specified in the [MULTILINE\_PARSER].

Prior knowledge of the conditions in the content that determine the start and continuation of subsequent lines in a multiline message is necessary to To determine the required Multiline parser type for a given use case, and you must know what content indicates the start and continuation of subsequent lines in a multiline message. Fluent Bit offers a regex-based configuration that supports states and can handle simple to complex cases.

#### Example Configuration

fluent-bit.conf


```yaml
[SERVICE]
	flush    	1
	log_level	info
	parsers_file parsers_multiline.conf

[INPUT]
	name         	tail
	path         	test.log
	read_from_head   true
	multiline.parser multiline-regex-test
```
parser.conf


```yaml
[MULTILINE_PARSER]
	name      	multiline-regex-test
	type      	regex
	flush_timeout 1000
	# Regex rules for multiline parsing
	# ---------------------------------
	#
	# configuration hints:
	#
	#  - first state always has the name: start_state
	#  - every field in the rule must be inside double quotes
	#
	# rules   |   state name   | regex pattern               	| next state name
	# --------|----------------|--------------------------------------------------
	rule     	"start_state"   "\d+\s\[.*\]"  "cont"
	rule     	"cont"      	"^\s+at.*" "cont"
```
### How to get structured data?

To get structured data from log messages using Fluent Bit, you need to define a parser plugin that can match the log messages and extract the desired data. The parser plugin uses regular expressions to match log messages and named capture groups to extract structured data.

#### Example Configuration


```yaml
[INPUT]
	Name    	tail
	Path    	/var/log/myapp.log
	Tag     	myapp.log

[PARSER]
	Name    	myapp
	Format  	regex
	Regex   	^(?<timestamp>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2})\s+(?<severity>[A-Z]+)\s+(?<message>.+)$

[FILTER]
	Name    	parser
	Match   	myapp.log
	Key_Name	message
	Parser  	myapp

[OUTPUT]
	Name    	stdout
	Match   	myapp.log
```
## Conclusion

In conclusion, configuring Fluent Bit to parse log messages correctly is crucial for ensuring accurate and complete log data is sent to the destination. By properly handling multiline log messages, Fluent Bit can avoid treating each line as a separate log entry and instead extract the desired structured data.

Fluent Bit is a powerful tool and easy to configure manually, however, it becomes difficult to manage as your infrastructure scales. Since it’s maintained by the community, it’s regularly updated which can be a challenge if you’re operating at scale and have to manually perform upgrades and patches.

You can boost your enterprise observability with Calyptia for Fluent Bit which is a long-term service solution that provides support for 1 year along with managing upgrades and patches. It also provides you with direct customer support and SLA to ensure a reliable monitoring infrastructure.

