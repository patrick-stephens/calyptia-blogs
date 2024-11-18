---
title: "Scaling ARM builds with Actuated"
date: "2023-08-24"
description: "Calyptia fixed failing ARM builds for open-source Fluent Bit and accelerated our commercial development by adopting Actuated and bare-metal runners."
image: "https://www.datocms-assets.com/97087/1692911945-actuated-social-media.png?auto=format&fit=max&w=1200"
author: "Patrick Stephens"
canonicalUrl: "https://calyptia.com/blog/scaling-builds-with-actuated"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/scaling-builds-with-actuated). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*


> TLDR; Calyptia fixed its failing Arm builds for open-source Fluent Bit and accelerated our commercial development by adopting Actuated and bare-metal runners.
> 
> 

## Intro

Some architecture builds can be slow using the Github Actions hosted runners due to emulating the non-native architecture for the build. This blog shows how Calyptia reduced build times and costs using [Actuated](https://actuated.dev) to create self-hosted runners for dedicated builds in a secure and easy-to-maintain fashion.

Calyptia maintains the OSS and CNCF graduated Fluent projects including [Fluent Bit](https://fluentbit.io/), an agent for collecting, processing, and routing logs, metrics, and trace data. Fluent Bit has been downloaded billions of times, and it is the default telemetry pipeline agent deployed in major Kubernetes distributions, including Google Kubernetes Engine (GKE) and AWS Elastic Kubernetes Service (EKS). We also provide Calyptia Core, our commercial telemetry pipeline management platform, which enables users to manage the collection, processing, and routing of event data to any destination.

## The problem

One of the best things about Fluent Bit is that we provide native packages (RPMs and DEBs) for a myriad of supported [targets](https://docs.fluentbit.io/manual/installation/supported-platforms) (various Linux, macOS and Windows). However, this  creates a support challenge due to the complexity of building and testing across all these targets. 

When PRs are provided we would like to ensure they function across the targets, but doing so can take a very long time (hours) and consume a lot of resources (that must be paid for). This means that these long running jobs are only done via exception (manually labelling a PR or on full builds for releases), leading to issues only being discovered when a full build and test is done, e.g. during the release process, thus blocking the release until it is fixed.

The long build time problem came to a head when we discovered we could no longer build for Amazon Linux 2023 (AL2023) because the build time exceeded the [6 hour limit for a single job on Github](https://docs.github.com/en/actions/learn-github-actions/usage-limits-billing-and-administration#usage-limits). We had to [disable the AL2023 target](https://github.com/fluent/fluent-bit/issues/6978) for releases which meant users could not update to the latest release leading to missing features or security problems.

In addition to challenges in the OSS, there are also challenges on the commercial side. Here, we are seeing issues with extended build times for [ARM64](https://en.wikipedia.org/wiki/AArch64) targets because our CI is based on Github Actions and currently only AMD64 (also called x86-64 or x64) runners are provided for builds. This slows down development and can mean bugs are not caught as early as possible.

### Problems with using self-hosted runners

One way to speed up builds is to provide self-hosted ARM64 runners.

Unfortunately, runners pose security implications, particularly for public repositories. In fact, Github [recommends against using self-hosted runners](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners#self-hosted-runner-security).

In addition to security concerns, there are also infrastructure implications for using self-hosted runners. We have to provide the infrastructure around deploying and managing the self-hosted runners, installing an agent, configuring it for jobs, etc. Particularly for our  OSS projects, we want anything we do to be simple and easy for maintenance purposes. 

Any change we make needs to be compatible with downstream forks as well. We do not want to break builds for existing users, particularly for those who are also contributors to the open source project. Therefore we need a solution that does not impact them.

There are various tools that can help with managing self-hosted runners, and Johannes Nicolai [provides a good curated list](https://jonico.github.io/awesome-runners/). I performed an evaluation of some of the recommended tools, but the solution would be non-trivial and would require additional effort to maintain.

### Requirements

We have the following high level goals in a rough priority order:

* Keep the process as secure as possible.
* Speed up the build.
* Keep costs minimal.
* Make it simple to deploy and manage.
* Minimize impact to OSS forks and users.

## The solution

At KubeCon EU 2023 I met up with [Alex Ellis](https://calyptia.commailto:alex@openfaas.com) from [Actuated](https://actuated.dev/) (and of [OpenFAAS](https://www.openfaas.com/) fame). After a conversation about his new project I wanted to put Alex and his technology to the test and see if Actuated could fix the problems we see with our build process.

To understand Actuated refer to their [documentation](https://docs.actuated.dev/), with this specific [blog post](https://actuated.dev/blog/is-the-self-hosted-runner-safe-github-actions) being a good overview of why we considered adopting it. We're not the only CNCF project that Alex's team was able to help. He [describes](https://actuated.dev/blog/native-arm64-for-github-actions) how he helped Parca and Network Service Mesh to slash their build times by using native Arm hardware. 

A quick TLDR; though would be that Actuated provides an agent you install which then automatically creates ephemeral VMs on the host for each build job. Actuated seemed to tick the various boxes (see the considerations above) we had for a solution, but you should never trust a vendor until you’ve tried it yourself!  


To use Actuated, you have to provision a machine with the Actuated agent, which is trivial and [well documented](https://docs.actuated.dev/install-agent/). 

We deployed an Ampere Altra Q80 server with 256GB of RAM and 80 cores ARM64 via Equinix (Equinix donates resources to the CNCF which we use for Fluent Bit so this satisfies the cost side of things) and installed the Actuated agent on it per the [Actuated docs](https://docs.actuated.dev/install-agent/). 

The update required to start using Actuated in OSS Fluent Bit is a one-liner. (Thanks in part to my excellent work refactoring the CI workflows — or so I like to think. You can see the [actual PR here for the change](https://github.com/fluent/fluent-bit/pull/7527). 

  
The following is the code required to start using Actuated:


```bash
-    runs-on: ubuntu-latest
+    runs-on: ${{ (contains(matrix.distro, 'arm' ) & 'actuated-arm64') || 'ubuntu-latest' }}
```
In Github Actions parlance, the code above translates to “if we are doing an ARM build, then use the Actuated runner; otherwise, use the default Github Hosted (AMD64) Ubuntu runner”. 

In the real code, I added an extra check so that we only use Actuated runners for the official source repo which means any forks will also carry on running as before on the Github Hosted runner.

## Outcomes for Fluent Bit

With this very simple change, all the ARM64 builds that used to take hours to complete now finish in minutes. In addition, we can actually build the AL2023 ARM64 target to satisfy those users too. **A simple change gave us a massive boost to performance and also provided a missing target.**

To demonstrate this is not specific to Equinix hosts or in some fashion difficult to manage in heterogeneous infrastructure (e.g. various hosts/VMs from different providers), we also replicated this for all our commercial offerings using a Hetzner host. The process was identical: install the agent and make the *runs-on* code change as above to use Actuated. Once again, we saw massive improvements in build times.

The usage of bare-metal (or cloud) hosts providers is invisible and only a choice of which provider you want to put the agent on. In our case we have a mixed set up with no difference in usage or maintenance.

## Challenges building containers for Calyptia Core

The native package (RPM/DEB) building described above was quite simple to integrate via the existing workflows we had. 

Building the native packages is done via a process that runs a target-specific container for each of the builds, e.g. we run a CentOS container to build for that target. This allows a complete build to run on any Linux-compatible machine with a container runtime either in CI or locally. For ARM builds, we were using QEMU emulation, hence the slowdown as this has to emulate instructions between architectures.

Container builds are the primary commercial area for improvement as we provide a SAAS solution running on K8S. Container builds were also a trickier proposition for OSS as we were using a single job to build all architectures using the *docker/build-push-action*. The builds were incredibly slow for ARM and also atomic, which means if you received a transient issue in one of the architecture builds, you would have to repeat the whole lot.

As an example: <https://github.com/fluent/fluent-bit/blob/master/.github/workflows/call-build-images.yaml> 


```yaml
- name: Build the production images
  id: build_push
  uses: docker/build-push-action@v4
  with:
    file: ./dockerfiles/Dockerfile
    context: .
    tags: ${{ steps.meta.outputs.tags }}
    labels: ${{ steps.meta.outputs.labels }}
    platforms: linux/amd64, linux/arm64, linux/arm/v7
    target: production
    # Must be disabled to provide legacy format images from the registry
    provenance: false
    push: true
    load: false
    build-args: |
      FLB_NIGHTLY_BUILD=${{ inputs.unstable }}
      RELEASE_VERSION=${{ inputs.version }}

```
The build step above is a bit more complex to tease out into separate components: we need to run single architecture builds for each target then provide a multi-arch manifest that links them together.

We reached out to Alex on a good way to modify this to work within a split build per architecture approach. The Actuated team has been very responsive on these types of questions along with proactive monitoring of our build queue and runners.

Within Calyptia, we have followed [the approach Docker provided](https://docs.docker.com/build/ci/github-actions/multi-platform/#distribute-build-across-multiple-runners), as suggested by the Actuated team. 

### A Process Recommendation

 Based on what we learned, we recommend the following process:

1. Build each architecture and push by digest in a set of parallel matrix jobs.
2. Capture the output digest of each build.
3. Create the multi-arch manifest made up of each digest we have pushed in step 1 using the artefact from step 2.

This approach provides two key benefits. First, it allows us to run on dedicated runners per-arch. Second, if a job fails we only need to repeat the single job, instead of having to rebuild all architectures.

The new approach reduced the time for the release process for the Calyptia Core K8s Operator from more than an hour to minutes. Additionally, because we can do this so quickly, we now build all architectures for every change rather than just on release. This helps developers who are running ARM locally for development as they have containers always available.

The example time speed up for the Calyptia Core K8S operator process was replicated across all the other components. A very good bang for your buck! 

For us, the actuated subscription fee has been of great value. Initially we tested the waters on the Basic Plan, but soon upgraded when we saw more areas where we could use it. The cost for us has been offset against a massive improvement in CI time and development time plus reducing the infrastructure costs of managing the self-hosted runners.

## Lessons learnt

The package updates were seamless really; however, we did encounter some issues with the ecosystem (not with actuated) when refactoring and updating our container builds. The issues with the container builds are covered below to help anyone else with the same problems.

### Provenance enabled by default

We were using v3 of Docker’s `*docker/build-push-action*`, but they made a breaking change which caused us a headache. They changed the default in v4 to create the various extra artefacts for provenance (e.g. SBOMs) which did have a few extra side effects both at the time and even now.

If you do not disable this then it will push manifest lists rather than images so you will subsequently get an error message when you try to create a manifest list of another manifest list.

Separately this also causes issues for older *docker* clients or organizations that need the legacy *Docker* schema format from a registry: using it means only OCI format schemas are pushed. This [impacted both OSS](https://github.com/fluent/fluent-bit/issues/7748) and our commercial offerings because users on older OS’s or with requirements for only consuming Docker schema (e.g. maybe an internal mirror registry only supports that) could not pull the images.

### Invalid timestamps for gcr.io with manifests

A funny problem found with our cloud-run deployments for Calyptia Core SAAS offering was that pushing the manifests to (Google Container Registry) gcr.io meant they ended up with a zero-epoch timestamp. This messed up some internal automation for us when we tried to get the latest version.

To resolve this we just switched back to doing a single architecture build as we do not need multi-arch manifests for cloud-run. Internally we still have multi-arch images in ghcr.io for internal use anyway; this is purely the promotion to gcr.io.

### Manifests cannot use sub-paths

This was a fun one: when specifying images to make up your manifest they must be in the same registry of course! 

Now, we tend to use sub-paths a lot to handle specific use cases for ghcr.io, but unfortunately you cannot use them when trying to construct a manifest.


```bash
OK: ghcr.io/calyptia/internal/product:tag --> ghcr.io/calyptia/internal/product:tag-amd64

NOK: ghcr.io/calyptia/internal/product:tag --> ghcr.io/calyptia/internal/amd64/product:tag
```
As with all good failures, the tooling let me make a broken manifest at build time, but unfortunately trying to pull it meant a failure at runtime.

### Actuated container registry mirror

All Github hosted runners provide default credentials to authenticate with docker.io for pulling public images. When running on a self-hosted runner you need to authenticate for this otherwise you will hit [rate limits](https://www.docker.com/increase-rate-limits/) and builds may fail as they cannot download required base images.

Actuated provide [a registry mirror and Github Action](https://docs.actuated.dev/tasks/registry-mirror/) to simplify this, so make sure you set it up

As part of this, ensure it is set up for anything that uses images (e.g. we run integration tests on KIND that failed as the cluster could not download its images) and that it is done after any [*buildx* config](https://docs.docker.com/engine/reference/commandline/buildx_build/) as it creates a dedicated `*buildx*` builder for the mirror usage.

### Actuated support

The Actuated team helped us in two ways: 

* First, we enabled Arm builds for our OSS projects and our commercial products when they timed out with hosted runners.
* Second, our costs were getting out of hand on GitHub’s larger hosted runners: Actuated not only reduced the build time, but the billing model is flat-rate, meaning our costs are now fixed, rather than growing.

As we made suggestions or collaborated with the Actuated team, they updated the documentation, including our suggestions on smoothing out the onboarding of new build servers and new features for the CLI.


> *Actuated aims to give teams the closest possible experience to managed runners, but with native arm support flat rate billing, and secure VM-level isolation. Since Calyptia adopted actuated, we’ve also shipped an SSH debug experience (like you’d find with CircleCI) and detailed reports and insights on usage across repos, users and organisations.*
> 
> *Alex Ellis*
> 
> 

The more improvements we’ve made, the more we’ve seen. Next on our list is getting the runtime of a Go release down from 26 minutes by bringing it over to Actuated.

