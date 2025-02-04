---

copyright:
  years: 2015, 2020
lastupdated: "2020-06-24"

keywords: troubleshoot, private workers

subcollection: ContinuousDelivery

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}
{:tip: .tip}
{:important: .important}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:support: data-reuse='support'}

# Troubleshooting for Pipeline Private Workers
{: #troubleshoot-pipeline-private-workers}

General problems with using Pipeline Private Workers might include Kubernetes cluster and kubectl version issues. In many cases, you can recover from these problems by following a few easy steps.
{: shortdesc}

## Why is my {{site.data.keyword.deliverypipeline}} Private Worker inactive?
{: #troubleshoot-pw-inactive}
{: troubleshoot}
{: support}

Private workers within a pool of workers can be in one of the following states:

* Active with the current, supported version of private workers
* Inactive with an unsupported version of private workers
* Inactive

Your private worker is inactive. Inactive private workers cannot handle incoming run requests. Pipeline stages that use an inactive private worker cannot complete.
{: tsSymptoms}
   
There is an issue with your Kubernetes cluster and the worker cannot be contacted. Or, the version of the private worker that you are running is no longer supported.
{: tsCauses}

To activate your {{site.data.keyword.deliverypipeline}} private worker, [install](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-install-private-workers#install_pw) the private worker again. Then, [register](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-install-private-workers#register_pw) the {{site.data.keyword.deliverypipeline}} private worker on the Kubernetes cluster again.
{: tsResolve}

## I tried to install support for {{site.data.keyword.deliverypipeline}} Private Workers in Kubernetes. Why did the installation fail?
{: #troubleshoot-pw-install}
{: troubleshoot}
{: support}

If there is an issue with the version of kubectl that you are running on the client machine, the {{site.data.keyword.deliverypipeline}} Private Worker installation fails. 

After you try to install support for private workers in Kubernetes, an error message is displayed to indicate that there is a schema mismatch and the installation fails.
{: tsSymptoms}

`SchemaError(io.k8s.apimachinery.pkg.apis.meta.v1.APIGroup): invalid object doesn't have additional properties`
   
There is a mismatch between the versions of kubectl that you are running on the Kubernetes server and the Kubernetes client.
{: tsCauses}

Install the latest version of kubectl on the client machine.
{: tsResolve}


## Why can't I pull images for tekton-releases or pipeline-private-worker from some container registries?
{: #troubleshoot-pw-images}
{: troubleshoot}
{: support}

Cluster security prevents you from pulling down images. 

When you try to install the private worker framework, an error message is displayed.
{: tsSymptoms}

```text
Error from server (InternalError): error when creating "https://private-worker-service.us-south.devops.cloud.ibm.com/install": Internal error occurred: admission webhook "trust.hooks.securityenforcement.admission.cloud.ibm.com" denied the request: 
Deny "gcr.io/tekton-releases/github.com/tektoncd/pipeline/cmd/controller@sha256:80e040a58ce6c4d58ae893eb934777bce013ef8be079967dc3db783d76fa5aaa", no matching repositories in ClusterImagePolicy and no ImagePolicies in the "tekton-pipelines" namespace
Error from server (InternalError): error when creating "https://private-worker-service.us-south.devops.cloud.ibm.com/install": Internal error occurred: admission webhook "trust.hooks.securityenforcement.admission.cloud.ibm.com" denied the request: 
Deny "gcr.io/tekton-releases/github.com/tektoncd/pipeline/cmd/webhook@sha256:da75fbdaeb800813d85b99f7f54b665e8d0edbb2c5a7ffc6a99d66aede0291a3", no matching repositories in ClusterImagePolicy and no ImagePolicies in the "tekton-pipelines" namespace

```
   
The private worker installer pulls images from `gcr.io` and [Docker hub](https://hub.docker.com/r/ibmcom/pipeline-private-worker){: external}. Some platforms, such as IBM Cloud Private, do not allow these container registries on the default image policy.
{: tsCauses}

Make sure that the policy for pulling images in your cluster supports pulling images from `gcr.io` and `docker.io`. For example, if you are installing the private worker framework on {{site.data.keyword.cloud_notm}} Private, add those policies by using the {{site.data.keyword.cloud_notm}} Private Web console. For more information about managing image security enforcement by using the IBM Cloud Private Web console, see [Enforcing container image security](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/manage_images/image_security.html){: external}.
{: tsResolve}
