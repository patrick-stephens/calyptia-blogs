---
title: "Automating For The Next Million Fluent Bit Downloads, The Packaging Problem!"
date: "2021-12-17"
description: "Calyptia has modernized the release process for Fluent Bit adding automated testing and other best practices."
image: "https://www.datocms-assets.com/97087/1682098312-fluentbit-evolve-calyptia.png?auto=format&fit=max&w=1200"
author: "Patrick Stephens"
canonicalUrl: "https://calyptia.com/blog/automating-for-the-next-million-fluent-bit-downloads-the-packaging-problem"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/automating-for-the-next-million-fluent-bit-downloads-the-packaging-problem). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

## Calyptia Is Evolving Fluent Bit, Adding More Automation And Security

![fluentbit-evolve-calyptia.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098317-fluentbit-evolve-calyptia.png&w=1200&q=75)The current release process for Fluent Bit relies on a custom build infrastructure and scripts to deploy things, along with the personnel able to do it. There is limited automated testing, so every release is therefore subject to whatever manual checks are carried out for packaging and those are usually limited to only a subset of the platforms. As Fluent Bit continues to grow in popularity – with two million Docker deployments per day, and increasing adoption from cloud providers and observability providers – we decided to modernize the packaging process to benefit users and the community.

The new automation Calyptia invested in for Fluent Bit allows any user to produce a qualified release, easily add additional platform support, apply security tooling to improve supply chain security, and use more analysis tooling to catch issues earlier. 

## [TLDR](https://www.dictionary.com/browse/tldr);

We wanted to improve our release process.

Our goals were:

1. Automate: No manual builds or tests
2. Test : Verification across all targets
3. Simplify: Less complexity == less bugs
4. Secure: The outputs and the process

This article covers how Calyptia accomplished this for the open source Fluent Bit project.

Feel free to look at the final workflows for [building](https://github.com/fluent/fluent-bit/blob/master/.github/workflows/staging-build.yaml) and [testing](https://github.com/fluent/fluent-bit/blob/master/.github/workflows/staging-test.yaml) with an overview shown below:

![Automated build overview flowchart](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098321-fluent-bit-release-process-1.png&w=1200&q=75)Automated build overview## Why and How?

### Automation

![Fluent Bit logo](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098326-3.png&w=384&q=75)Whilst there are various scripts already present for Fluent releases, these require manual triggers at each stage of the process on the appropriate build machine with valid credentials. The process is known to the maintainers but hard and time consuming to follow. This manual process also opens up the possibility of misconfiguration or other errors leading to problems in the final release.

Fluent Bit already makes use of [Github Actions](https://github.com/features/actions) and we decided to build on this to add the extra workflows to fully automate the complete release process. We reduced the risk of *“bus factor”*, only one maintainer able to deploy, by allowing anyone to trigger the process and get a reproducible and qualified release out the other end with minimal effort.

For Fluent Bit we build to the following locations prior to release:

* Containers → gchr.io (Github container registry)
* Binary packages → S3 bucket

These are then tested and eventually get pushed to the release repositories without having to be rebuilt.

## Testing

Another area of concern with the current process was that we did not provide robust automated tests for packaging. There were manual checks on each release but these could miss an issue with a specific target or things like upgrade vs installation.

In adding automation, we were not focusing on testing Fluent Bit functionality as there is existing testing for that, but instead making sure that when we package it up for each target we have done so correctly. We want to prevent common errors like misconfiguring permissions for a binary or breaking an init script.

![Testing process flowchart](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098239-testing-process.png&w=1920&q=75)The testing processFor containers we now verify locally that each architecture runs with a basic configuration and then we verify it deploys using the [Helm chart](https://github.com/fluent/helm-charts) on Kubernetes-in-Docker ([KIND](https://kind.sigs.k8s.io/)). This has an additional benefit of testing the Helm chart too. We found an [issue](https://github.com/fluent/fluent-bit/issues/4404) with the ARM containers when running this so it demonstrated its benefit immediately.

For binary packages we use a representative container for each OS that runs its init process. We upgrade or install Fluent Bit and confirm that the service is correctly running when the container starts.This provides a quick and simple sanity check on the package.

As part of the process we then hook in triggers for the existing integration tests plus adding auditable approvals for promoting to a release.

## Simplification

Simplification may seem like a non-critical requirement however reducing complexity has a myriad of benefits in reducing bugs, improving readability and support.

The current process makes use of multiple repositories for packages, not including container images, and we wanted to simplify this process to make it straightforward for any consumer of the OSS to follow, audit, and submit changes back to the project. This has come up a few times, for example in the Fluent Bit Slack channel, as users ask how to build for a target operating system without realising there was a separate repository for this.

We moved a large portion of the separate repositories into the main repository to both help with their use in the actions but it also makes it easier for other users to follow and reproduce themselves. This consolidation also aligns how we build containers in the main repository with how we build the packages.

A subset of the current process, packaging the binaries, is shown below to demonstrate this:

![flowchart](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098331-build-package-process.png&w=1920&q=75)The build processThis is just a subset of the process with sections that need to be repeated per target and others run on different machines. It also does not include the container build and publish which is similar. We automated and improved this so that now, the new full process for building both images and packages looks like this:

![screenshot](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098335-screenshot-from-2021-12-14-12-51-33.png&w=1920&q=75)The new [reusable workflows](https://github.blog/2021-11-29-github-actions-reusable-workflows-is-generally-available/) option in Github Actions also opened the door to reducing the complexity where we build things on master, per release, etc. by reusing the same action with different configuration. This moves us to a single source for each job to reuse so reduces maintenance effort as well as preventing simple issues like where we only update one copy of a particular workflow.

As the workflows have evolved, simpler standard actions have come along that can replace some of the bespoke setup and build process. We replaced a lot of the cruft particularly around building multi-arch containers with standard actions instead. This makes it easier to understand and maintain.

## Secure

![Trivy logo](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098340-7.png&w=256&q=75)We want to ensure we provide secure binaries as much as possible via industry best practices. We used tooling like [Trivy](https://aquasecurity.github.io/trivy) and [Dockle](https://github.com/goodwithtech/dockle#comparison), covered later, to automate these kinds of checks. The extra granularity of reusable workflows also meant we could restrict the exposure of secrets to just those steps that need them quite easily.

![Dockle logo](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098344-8.png&w=256&q=75)We also want to provide a simple record of “approving” a release so we have an approach that builds to a *staging* environment and then a *release* environment has protection rules in place to allow nominated approvers to release it once whatever checks are complete.

Finally, users will want to authenticate that their images or packages are the actual ones provided by Fluent Bit. The current process already provided GPG signing via the manual process but no support for containers so we added [Cosign](https://github.com/sigstore/cosign) support to allow people to verify their containers.

## Conclusion

Overall, [Calyptia](https://calyptia.com/) achieved the goals for the OSS Fluent Bit project set out earlier, although there are always more improvements to be done and some are underway now: for example, the provision of soak testing and true multi-arch container definitions.

For the OSS project, this work from Calyptia has improved the verification of the packaging process and the final binaries with things like automated CVE & best practices checks. As Fluent Bit is deployed at least over two million times daily in cloud and containerised environments this will directly benefit a lot of users.

Watch this space for follow up posts going into more detail in the areas covered here.

