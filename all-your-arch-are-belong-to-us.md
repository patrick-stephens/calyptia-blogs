---
title: "All your arch are belong to us!"
date: "2022-01-26"
description: "All Fluent Bit container images have been updated to use Debian 11 along with various other improvements. Later we have an example showing how to build multi-arch images on Ubuntu."
image: "https://www.datocms-assets.com/97087/1682098212-arch-blog.png?auto=format&fit=max&w=1200"
author: "Patrick Stephens"
canonicalUrl: "https://calyptia.com/blog/all-your-arch-are-belong-to-us"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/all-your-arch-are-belong-to-us). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

**Calyptia is providing true multi-arch containers images with debug variants**

![Meme showing all your arch are belong to us](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098220-arch.png&w=1920&q=75)## TLDR;

All Fluent Bit container images have been updated to use Debian 11 along with various other improvements. Later we have an example showing how to build multi-arch images on Ubuntu.

## The problem

There is a good overview of how to do multi-arch container builds [here](https://www.docker.com/blog/multi-arch-build-and-images-the-simple-way/): the gist is there is a hard way and an easy way. Fluent Bit used the hard way, namely using multiple images built from different definitions then linked together with a multi-arch manifest.

There are obviously reasons why it was done this way, including but not limited to:

* Legacy as it was the only option when first attempted.
* Attempting to optimise the size for each target.

Using a manifest with discrete image definitions does introduce many other issues though:

* Updates to one image (e.g. new libraries for AMD 64 architecture) can be missed on the other images.
* Effort multiplied to update each image to a new version, e.g. for Debian 11.
* Single architecture only (AMD 64) debug image.
* Brittle dependencies using direct copying of libraries rather than package installation.
* CI effort and time to build each individual image and then link it into a manifest.

## The (possible) solution

A single definition has the benefits of simplifying the overall process and preventing changes being missed or made incorrectly for every architecture. Potentially we lose the benefits of optimisation though for a specific target as well although this needs to be confirmed.

We go from building three separate Dockerfiles, one for each architecture, then linking them together with a manifest to building all three from a single Dockerfile all in one go.

![flowchart](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098226-multiarch-container-blog-post-1.png&w=1920&q=75)Hard vs easy way of multi-arch Dockerfile supportWe decided therefore to provide true multi-arch and single definition container images as a new addition to evaluate this and get some feedback compared to the originals. The single [Dockerfile](https://github.com/fluent/fluent-bit/tree/master/dockerfiles) replaces the use of three separate files plus a manifest. It follows the original images fairly closely initially so it may be updated over the next few months to further improve things.

## How to build multi-arch images

This section uses Fluent Bit as an example but hopefully is of use to anyone wanting to do a similar thing, i.e. build a single Dockerfile for multiple architectures all in one go.

With [QEMU](https://www.qemu.org/) set up and [buildkit](https://docs.docker.com/buildx/working-with-buildx) support, you can build all targets in one simple call quite easily and quickly (relatively obviously to your hardware!).

Using my local Ubuntu 20.04 development PC as an example, I set up QEMU based on this [StackOverflow post](https://askubuntu.com/a/1369504):


```bash
sudo add-apt-repository ppa:jacob/virtualisation
sudo apt-get update && sudo apt-get install qemu qemu-user qemu-user-static
```
I then deployed buildkit support for Docker using the [official instructions](https://docs.docker.com/buildx/working-with-buildx/#install):


```bash
wget https://github.com/docker/buildx/releases/download/v0.7.1/buildx-v0.7.1.linux-amd64
mv buildx-v0.7.1.linux-amd64 ~/.docker/cli-plugins/docker-buildx
chmod a+x ~/.docker/cli-plugins/docker-buildx
```
Now, we need to configure QEMU and buildx integration, again using a [StackOverflow post](https://stackoverflow.com/a/60667468) for guidance:


```bash
docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
docker buildx rm builder
docker buildx create --name builder --use
docker buildx inspect --bootstrap
```
Once we have that we can now build all in one go from the root of the Fluent Bit git repository:


```bash
docker buildx build --platform "linux/amd64,linux/arm64,linux/arm/v7" -f ./dockerfiles/Dockerfile.multiarch --build-arg FLB_TARBALL=https://github.com/fluent/fluent-bit/archive/v1.8.11.tar.gz ./dockerfiles/
```
An unrelated benefit of using *buildx* is that multi-stage builds skip targets not required and it has other benefits, e.g. extra mount types (SSH, cache, etc.) plus for me generally seems to build faster than without.

This approach simplifies the CI set up to do all this in Github actions. Instead of multiple matrix jobs with a separate job for the manifest we can do it all in one go.

![Flowchart](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098230-hard-way-multi-arch-build.png&w=1920&q=75)Original “hard” way for multiple architecture buildThe above shows the Fluent Bit staging workflow introduced in a previous [article](https://calyptia.com/blog/automating-for-the-next-million-fluent-bit-downloads-the-packaging-problem) with the longer highlighted pipeline showing the creation of the original containers and below the single job highlighted as the new multi-arch build and push.

![Flowchart](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098235-easy-way-multi-arch-build.png&w=1920&q=75)New “easy” way for multiple architecture build## Package installation

The original images try to optimise their size by copying libraries from a builder image to the production image, e.g. a small [snippet](https://github.com/fluent/fluent-bit/blob/1c836f83f83f77e230e760fc292459d661482350/dockerfiles/Dockerfile.x86_64#L75-L80):


```bash
COPY --from=builder /usr/lib/x86_64-linux-gnu/sasl /usr/lib/x86_64-linux-gnu/
COPY --from=builder /usr/lib/x86_64-linux-gnu/libz* /usr/lib/x86_64-linux-gnu/
COPY --from=builder /lib/x86_64-linux-gnu/libz* /lib/x86_64-linux-gnu/
COPY --from=builder /usr/lib/x86_64-linux-gnu/libssl.so* /usr/lib/x86_64-linux-gnu/
COPY --from=builder /usr/lib/x86_64-linux-gnu/libcrypto.so* /usr/lib/x86_64-linux-gnu/
COPY --from=builder /usr/lib/x86_64-linux-gnu/libgcrypt.so* /usr/lib/x86_64-linux-gnu/
```
The main reason for this is to improve security and image size by using the [distroless](https://github.com/GoogleContainerTools/distroless) base image. However, it is not consistent in the original images as the ARM 32 image uses package installation whereas the others do not plus the two ARM images are not using a base distroless image.

As you can imagine, this quickly gets unwieldy and is very brittle to missed or modified dependencies – issues may only be found when the container is run and possibly only run in a particular way or with a particular config. It can also inhibits some security or other CICD tooling that checks packages installed.

To simplify things for this developer preview we therefore decided to switch to standard package installation across the board:


```bash
RUN apt-get update && 
    apt-get install -y --no-install-recommends 
    libssl1.1 
    libsasl2-2 
    pkg-config 
    libpq5 
    libsystemd0 
    zlib1g 
    ca-certificates 
    libatomic1 
    libgcrypt20 
    && apt-get clean 
    && rm -rf /var/lib/apt/lists/*
```
This may have an impact on image size but does have a lot of other benefits as you can see.

## Security

The main downside to this approach is that we are using distroless images currently for the existing AMD64 containers (but not the other architectures) which has significant security benefits. It does not provide a shell or package managers and generally limits dependencies to the bare minimum required for the application.

One area we would be quite interested in feedback is the choice of package installation versus direct copying of libraries. There is the benefit to security but the downside is the possibility of missing a dependency particularly if they are dynamically loaded. Each time a dependency changes this may affect the libraries we need to copy so is a non-trivial thing to maintain and test particularly for transitive dependencies.

An option is to provide a distroless base image with the additional dependencies installed using the same Bazel build approach that Google uses. Another option is to use the distroless base image with a custom script that can copy the different libraries required for each target then run that.

Some good posts on background in this area are below that go over the pro/cons of distroless versus other options:

* Red Hat [blog](https://www.redhat.com/en/blog/why-distroless-containers-arent-security-solution-you-think-they-are) on issues plus their UBI approach
* A more open [view](https://hackernoon.com/distroless-containers-hype-or-true-value-2rfl3wat) asking what you need?

 

**What do you think? Please let me know.**

## Verification and testing

One area of significant improvement is the addition of testing across the board for all container image architectures, this was started in the previous work to provide a [staging](https://calyptia.com/blog/automating-for-the-next-million-fluent-bit-downloads-the-packaging-problem) workflow and continues here. With the original approach it is quite easy to either miss a change in a specific architecture (e.g. a new dependency is added or an existing one changes) or a change is not tested and fails on a specific target. There are recent examples of both of these unfortunately and there is also more work to improve ongoing – testing is never complete!

Now, we test all architectures and deploy the Helm chart as well as a sanity check. This includes the multi-arch containers which was pretty easy to add by just [adding another call](https://github.com/fluent/fluent-bit/blob/1c836f83f83f77e230e760fc292459d661482350/.github/workflows/staging-test.yaml#L56) of the [reusable job](https://github.com/fluent/fluent-bit/blob/master/.github/workflows/call-test-images.yaml) we created previously.

![Flowchart showing testing process](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098239-testing-process.png&w=1920&q=75)## Conclusion

Calyptia has provided a new set of container images built from a single multi-arch definition **including debug variants** for all supported (Linux) architectures: [http://ghcr.io/fluent/fluent-bit/multiarch](https://calyptia.comhttp://ghcr.io/fluent/fluent-bit/multiarch)

The existing container images (and AMD 64 debug only image) are still present. The new images will be evaluated for performance, security and other requirements so are still developer preview but we would like to hear feedback from the community on them, hence this article.

Please give the new containers a go and [let us know](https://calyptia.commailto:hello@calyptia.com) any feedback you have.

Keep an eye out for upcoming tech talks by the maintainers in this and similar areas too.

