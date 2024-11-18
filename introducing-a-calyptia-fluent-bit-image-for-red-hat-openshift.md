---
title: "Introducing a Calyptia Fluent Bit Image for Red Hat OpenShift"
date: "2022-01-16"
description: "Last week, Calyptia published a Red Hat certified container image based on upstream (open-source) Fluent Bit to the Red Hat container catalog. "
image: "https://www.datocms-assets.com/97087/1682098251-os.png?auto=format&fit=max&w=1200"
author: "Patrick Stephens"
canonicalUrl: "https://calyptia.com/blog/introducing-a-calyptia-fluent-bit-image-for-red-hat-openshift"
herobg: "https://www.datocms-assets.com/97087/1689182792-background-fluent-bit.png"
---
*This post was [originally published on the Calyptia blog](https://calyptia.com/blog/introducing-a-calyptia-fluent-bit-image-for-red-hat-openshift). [Calyptia](https://calyptia.com) is the primary sponsor and creator of the Fluent Bit project.*

**Now available as Red Hat Certified Software in the Red Hat Container Catalog**

Last week, Calyptia [published](https://catalog.redhat.com/software/containers/search?q=fluent-bit&p=1&vendor_name=Calyptia) a Red Hat certified container image based on upstream (open-source) Fluent Bit to the [Red Hat container catalog](https://catalog.redhat.com/software). The reason we did this was to support users who want to ensure they are using a Fluent Bit container image that is 1) Tested by Calyptia and then 2) Certified by Red Hat for their various platforms. 

This article is intended to give a short introduction and some examples of how to use this Fluent Bit image with [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift). It can also be used via standard containers by pulling the image and running it directly: *podman pull registry.connect.redhat.com/calyptia/fluent-bit:latest*

Below, we will show how you can get live logs from your OpenShift cluster into an Elasticsearch and Kibana instance using the Calyptia Fluent Bit image:

![screenshot-from-2022-01-13-10-37-36-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098256-screenshot-from-2022-01-13-10-37-36-1.png&w=1920&q=75)## What Is It?

The image is built from the open-source Calyptia for Fluent Bit here: <https://github.com/calyptia/fluent-bit/tree/master/calyptia/ubi8> 

The primary difference from upstream Fluent Bit is that instead of being based on a Debian base image we use a [Universal Base Image (UBI) provided by Red Hat](https://developers.redhat.com/products/rhel/ubi). This then allows us to be certified for the Red Hat ecosystem along with receiving security updates and the like from it. 

The image is built using the upstream source code directly from the [Fluent Bit](https://github.com/fluent/fluent-bit) repository. It mirrors the upstream build process closely just with those differences to support the Red Hat base image. There are some minor improvements to meet Red Hat certification requirements for security and licensing as well.

Certification covers a few different things including scanning for security vulnerabilities and verifying any licensing requirements. This is all Red Hat controlled with [documentation available](https://redhat-connect.gitbook.io/partner-guide-for-red-hat-openshift-and-container/program-on-boarding/certification-workflow) explaining it all. It demonstrates that the image meets a certain high level standard for any compliance requirements.

## OpenShift with Calyptia for Fluent Bit

The first phase of this work was to build the image with the Red Hat provided UBI base image. Once this was complete we then submitted it for certification to confirm it met all the requirements. This can take a while to complete depending on the complexity of the product involved but once we had it approved we then wanted to confirm the published image correctly functions within OpenShift – obviously we did similar testing prior to submission but this is a final check on the image users will actually download to use.

We wanted a confidence check that there are no issues with the image provided focussing on the primary plugins that users would likely want. Currently OpenShift provides an optional inbuilt logging stack making use of custom versions of FluentD, also supported by Calyptia,  and Elasticsearch with Kibana. We therefore want to verify those plugins function correctly.

## OpenShift Developer Sandbox Testing

The easiest way to get into OpenShift is to use the Red Hat managed developer sandbox. I used this initially as a simple validation test for the Calyptia for Fluent Bit image by just deploying it with the default configuration:

1. Set up test cluster: <https://developers.redhat.com/developer-sandbox>
2. Create a daemonset using the Calyptia Fluent Bit image:

apiVersion: apps/v1  
kind: DaemonSet  
*metadata:*  
*name: fb-test*  
*namespace: pat-calyptia-dev*  
*spec:*  
*selector:*  
*matchLabels:*  
*app: fb*  
*template:*  
*metadata:*  
*labels:*  
*app: fb*  
*spec:*  
*containers:*  
*– name: fb*  
*image: >-*  
*registry.connect.redhat.com/calyptia/fluent-bit*  
*ports:*  
*– containerPort: 2020*

![screenshot-from-2022-01-10-10-34-01-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098261-screenshot-from-2022-01-10-10-34-01-1.png&w=1920&q=75)As you can see this will run up a Kubernetes daemonset in OpenShift with the Calyptia Fluent Bit image using its default configuration. The default configuration is to use the CPU input plugin and the standard output plugin so you should see CPU info in the logs of the pods run up.

![screenshot-from-2022-01-10-10-32-00-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098265-screenshot-from-2022-01-10-10-32-00-1.png&w=1920&q=75)This is a fairly trivial example but does demonstrate that the image can be pulled and run successfully with minimal effort.

## OpenShift Partner Lab Testing

Now, the developer sandbox is a multi-tenant system intended for basic development on OpenShift. It means therefore it has additional constraints that prevent us from managing cluster level access (e.g. for logs) and mounting Secrets or ConfigMaps.

Similarly there is no option to install one of the OpenShift operators via OperatorHub. In this case we want to test it with the Red Hat cluster logging operator as documented in the [official documentation](https://docs.openshift.com/container-platform/4.9/logging/cluster-logging-deploying.html).

To do more involved testing, we therefore need a full cluster we are in direct control of. We can either spin an OpenShift cluster up ourselves on a cloud provider or, as Calyptia is a Red Hat partner, we can ask for a Red Hat managed cluster dedicated for our use via the OpenShift Partner Lab (OPL). With a quick chat with our Red Hat partner representative we got one of these allocated in a few hours which is a full cluster we have complete control over.

With this cluster I can now deploy operators and completely control (or destroy!) the cluster components. I did a quick test following those [instructions](https://docs.openshift.com/container-platform/4.9/logging/cluster-logging-deploying.html) to deploy the default cluster logging stack to confirm it was all functional then removed it. 

## Elasticsearch Output

Initially, I deployed the Elastic Cloud on Kubernetes (ECK) operator and configured it with default values to provide me with a simple Elasticsearch and Kibana stack. One change I did make to the defaults was to disable TLS just to simplify any deployment or testing I needed to do, obviously do not do this in production!

*apiVersion: elasticsearch.k8s.elastic.co/v1*  
*kind: Elasticsearch*  
*metadata:*  
*name: elasticsearch-sample*  
*namespace: openshift-operators*  
*spec:*  
***http:***  
***tls:***  
***selfSignedCertificate:***  
***disabled: true***

This should spin up Elasticsearch and Kibana pods. Make sure to set up a route to view Kibana externally, as this is testing again I just set it up with TLS pass through which will trigger warnings in your browser too.

![screenshot-from-2022-01-12-19-50-40-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098269-screenshot-from-2022-01-12-19-50-40-1.png&w=1920&q=75)You should now be able to view the Kibana UI via the route.

![screenshot-from-2022-01-12-19-55-43-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098274-screenshot-from-2022-01-12-19-55-43-1.png&w=1920&q=75)To access you will need the generated credentials which are detained in a Secret in the namespace.

![screenshot-from-2022-01-12-19-57-54-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098278-screenshot-from-2022-01-12-19-57-54-1.png&w=1920&q=75)These credentials will also be required to set up Fluent Bit output.

We now have a running Elasticsearch cluster to use along with its associated Kibana instance. Next step is to enable this output in the Calyptia for Fluent Bit image to verify it. To do this we first create a ConfigMap for the Fluent Bit configuration making sure to include the credentials used for Kibana as well (again, don’t do this in production!):

*apiVersion: v1*  
*kind: ConfigMap*  
*metadata:*  
*name: fluent-bit-config*  
*namespace: openshift-operators*  
*data:*  
*fluent-bit.conf: |-*  
*[SERVICE]*  
*Daemon Off*  
*Log\_Level debug*  
*HTTP\_Server On*  
*HTTP\_Listen 0.0.0.0*  
*HTTP\_Port 2020*

*[INPUT]*  
*Name dummy*  
*Tag dummy.log*  
*Dummy {“message”: “testing”}*

*[OUTPUT]*  
*Name        es*  
*Match       \**  
*Host        elasticsearch-sample-es-default.openshift-operators.svc.cluster.local*  
*Port        9200*  
*Index       fluentbit*  
*tls Off*  
*tls.verify Off*  
       *Logstash\_Format On*  
*HTTP\_User elastic*  
*HTTP\_Passwd XXXXXXXXXXXXXX*

To keep things simple we just deploy in the same namespace but really it could be any but just make sure to set up any routing or network policies to enable it. We’re also still using dummy input here as we want to test one step at a time: previously we verified dummy input and standard output worked, now we are verifying that Elasticsearch output is working and next we will verify it with forward input but keeping the Elasticsearch output unchanged.

The service details in the Elasticsearch output configuration can be found in OpenShift:

![screenshot-from-2022-01-12-20-04-31-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098283-screenshot-from-2022-01-12-20-04-31-1.png&w=1920&q=75)Once we have this configuration set up we run up a pod with it. 

*apiVersion: v1*  
*kind: Pod*  
*metadata:*  
*name: fb-es-example*  
*labels:*  
*app: fb-es-example*  
*namespace: openshift-operators*  
*spec:*  
*containers:*  
*– name: fb*  
*image: registry.connect.redhat.com/calyptia/fluent-bit*  
*ports:*  
*– containerPort: 2020*  
*volumeMounts:*  
*– name: config-volume*  
*mountPath: /fluent-bit/etc/fluent-bit.conf*  
*subPath: fluent-bit.conf*  
*volumes:*  
*– name: config-volume*  
*configMap:*  
*name: fluent-bit-config*

The only difference from the previous deployment is we’re using the ConfigMap as a volume and specifying the default configuration file to simplify things.

Looks at the logs for the pod then to confirm it is all ok: 

If you see a dropped connection then review the TLS and authentication configuration is correct. You can shell into the container to also investigate things (there is no need for a debug container on OpenShift):

![screenshot-from-2022-01-12-20-34-52-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098287-screenshot-from-2022-01-12-20-34-52-1.png&w=1920&q=75)Now we should start seeing indexes appear in Kibana. Set up one appropriately, as we have *logstash\_format on* we can see it with the *logstash-\** prefix. 

As we’re only sending a test message it’s not wildly interesting yet:

![screenshot-from-2022-01-13-08-56-13-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098292-screenshot-from-2022-01-13-08-56-13-1.png&w=1920&q=75)## Red Hat Fluentd Forwarding

To quickly verify the *forward* plugin and receive actual logs we can deploy the Red Hat cluster logging operator configured to only deploy the Fluentd log collector sending to our Calyptia for Fluent Bit image which then sends it to that Elasticsearch cluster we deployed previously. This may seem a little convoluted but is really quite simple to deploy:

1. A ConfigMap update to add the *forward* input.
2. Set up a *Service* for Calyptia Fluent Bit deployment, this is the service for the input above in step 1.
3. Install the Red Hat cluster logging operator with a custom configuration to get fluentd to send to Calyptia Fluent Bit

For step 1, we update the configuration we previously had in the ConfigMap to add the *forward* input and also a simple standard output plugin just so it shows the logs being sent. The changes should look like this:

*fluent-bit.conf: |-*  
*[SERVICE]*  
*Daemon Off*  
*Log\_Level debug*  
*HTTP\_Server On*  
*HTTP\_Listen 0.0.0.0*  
*HTTP\_Port 2020*

*[INPUT]*  
*Name forward*  
*Port 24224*

*[OUTPUT]*  
*Name stdout*  
*Match \**

*[OUTPUT]*  
*Name        es*  
*Match       \**  
*Host        elasticsearch-sample-es-default.openshift-operators.svc.cluster.local*  
*Port        9200*  
*Index       fluentbit*  
*Tls         Off*  
*Tls.verify  Off*  
*Logstash\_Format On*  
*HTTP\_User elastic*  
*HTTP\_Passwd XXXXXXXXXXXXXX*

To demonstrate it is definitely getting new logs I also removed all indexes in Kibana – or you could re-deploy the ECK operator. Now we have our configuration we then create a Calyptia Fluent Bit Deployment:

*apiVersion: apps/v1*  
*kind: Deployment*  
*metadata:*  
*name: fluent-bit*  
*namespace: openshift-operators*  
*spec:*  
*selector:*  
*matchLabels:*  
*app: fluent-bit*  
*replicas: 1*  
*template:*  
*metadata:*  
*labels:*  
*app: fluent-bit*  
*spec:*  
*containers:*  
*– name: fluent-bit*  
*image: registry.connect.redhat.com/calyptia/fluent-bit*  
*ports:*  
*– containerPort: 2020*  
*– containerPort: 24224*  
*volumeMounts:*  
*– name: config-volume*  
*mountPath: /fluent-bit/etc/fluent-bit.conf*  
*subPath: fluent-bit.conf*  
*volumes:*  
*– name: config-volume*  
*configMap:*  
*name: fluent-bit-config*

Note the extra 24224 port for the forward input, the 2020 port is for the HTTP server which can provide metrics and healthcheck support. Once this deployment is created just ensure the pod comes up and logs are showing it running correctly:

![screenshot-from-2022-01-13-09-31-57-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098296-screenshot-from-2022-01-13-09-31-57-1.png&w=1920&q=75)The key messages are:

*[2022/01/13 09:31:27] [debug] [in\_fw] Listen=’0.0.0.0′ TCP\_Port=24224*  
*[2022/01/13 09:31:27] [ info] [input:forward:forward.0] listening on 0.0.0.0:24224*  
*[2022/01/13 09:31:27] [debug] [stdout:stdout.0] created event channels: read=20 write=21*  
*[2022/01/13 09:31:27] [debug] [es:es.1] created event channels: read=22 write=23*  
*[2022/01/13 09:31:27] [debug] [output:es:es.1] host=elasticsearch-sample-es-default.openshift-operators.svc.cluster.local port=9200 uri=/\_bulk index=fluentbit type=\_doc*  
*[2022/01/13 09:31:27] [debug] [router] match rule forward.0:stdout.0*  
*[2022/01/13 09:31:27] [debug] [router] match rule forward.0:es.1*  
*[2022/01/13 09:31:28] [ info] [http\_server] listen iface=0.0.0.0 tcp\_port=2020*

  
This shows the forward input port being set up and the output to Elasticsearch as well as the HTTP server (2020).

Now create a Service for the forward input:

*apiVersion: v1*  
*kind: Service*  
*metadata:*  
*name: fluent-bit*  
*namespace: openshift-operators*  
*spec:*  
*selector:*  
*app: fluent-bit*  
*ports:*  
*– protocol: TCP*  
*port: 24224*  
*targetPort: 24224*

This should look similar to the following and you can see the URL to use in the Fluentd configuration:

![screenshot-from-2022-01-13-09-41-53-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098301-screenshot-from-2022-01-13-09-41-53-1.png&w=1920&q=75)We now have all the extra “sauce” we need to use Calyptia for Fluent Bit. The rest of the setup is just following the official Red Hat [documentation to set up cluster logging](https://access.redhat.com/documentation/en-us/openshift_container_platform/4.9/html-single/logging/index#cluster-logging-deploy-console_cluster-logging-deploying).

The main thing to do is turn off the default Elasticsearch & Kibana instances as we’ll use the ones we had previously:

*apiVersion: “logging.openshift.io/v1”*  
*kind: “ClusterLogging”*  
*metadata:*  
*name: “instance”*  
*namespace: “openshift-logging”*  
*spec:*  
*managementState: “Managed”*  
*collection:*  
*logs:*  
*type: “fluentd”*  
*fluentd: {}*

Now set up forwarding of all logs to our Calyptia Fluent Bit service: 

<https://access.redhat.com/documentation/en-us/openshift_container_platform/4.9/html-single/logging/index#cluster-logging-collector-log-forward-fluentd_cluster-logging-external> 

*apiVersion: logging.openshift.io/v1*  
*kind: ClusterLogForwarder*  
*metadata:*  
*name: instance*  
*namespace: openshift-logging*  
*spec:*  
*outputs:*  
*– name: fluent-bit-tcp*  
*type: fluentdForward*  
*url: ‘tcp://fluent-bit.openshift-operators.svc.cluster.local:24224′*  
*pipelines:*  
*– name: forward-to-fluent-bit*  
*inputRefs:*  
*– application*  
*– audit*  
*– infrastructure*  
*outputRefs:*  
*– fluent-bit-tcp*  
*parse: json*  
*labels:*  
*clusterId: “calyptia-test”*

In this case we want TCP (not TLS) forwarding of all types of log (application, audit, infrastructure) and we do not want to send to the default ES instance.

It may take a little while but we should start receiving logs in the Fluent Bit instance, you can see this in the logs directly for the Fluent Bit pod as it uses the *stdout* plugin.

![screenshot-from-2022-01-13-10-34-56-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098305-screenshot-from-2022-01-13-10-34-56-1.png&w=1920&q=75)We can remove that output plugin and rollout the deployment again to reconfigure it (Fluent Bit does not automatically reload its configuration by default). 

Kibana should show you the logs now:

![screenshot-from-2022-01-13-10-37-36-1.png](https://calyptia.com/_next/image?url=https://www.datocms-assets.com/97087/1682098256-screenshot-from-2022-01-13-10-37-36-1.png&w=1920&q=75)These are actual logs via Calyptia Fluent Bit into Elasticsearch to display in Kibana.

## Further work

Now, this is enough to have confidence the image is functioning correctly. It unfortunately does not show how to integrate with the actual container logs equivalent to Red Hat Cluster Logging. This requires a bit more work and security set up so will be the subject of a follow up post showing how to integrate Calyptia for Fluent Bit into the default monitoring stack provided by OpenShift in a secure fashion.

