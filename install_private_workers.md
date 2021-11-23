---

copyright:
  years: 2019, 2021
lastupdated: "2021-11-22"

keywords: Delivery Pipeline Private Workers, Installation, Kubernetes cluster, private worker

subcollection: ContinuousDelivery

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}


# Installing {{site.data.keyword.deliverypipeline}} Private Workers
{: #install-private-workers}

Install and register a {{site.data.keyword.deliverypipeline}} Private Worker so that {{site.data.keyword.contdelivery_full}} Development teams can use the private worker in their toolchain configuration. Developers can run workloads within the network scope of the private worker installation without any inbound network connectivity. 
{: shortdesc}

The {{site.data.keyword.deliverypipeline}} uses public and private workers to run pipeline jobs. By default, pipeline jobs are run by using public workers on IBM-managed public shared infrastructure. Pipeline jobs can access resources only on the public network (both within and outside of IBM) and are limited to 60 minutes run time per job.

In certain scenarios, your {{site.data.keyword.deliverypipeline}} might require access to internal or on-premises resources. In these situations, you can connect to and integrate a {{site.data.keyword.deliverypipeline}} Private Worker to run on your own Kubernetes infrastructure.

The private worker agents that are installed on private clusters request data only from the IBM-hosted private worker service. The data flow is one way and originates only from the agent.

## Prerequisites
{: #pw_install_prereqs}

Before you install a private worker, make sure that you have an {{site.data.keyword.cloud}} account to create authentication keys. You need the latest kubectl version that is installed on the Administrator's desktop computer. And you must also have a [Kubernetes cluster](https://cloud.ibm.com/kubernetes/clusters){: external} (version 1.15 or higher) with Administrative access to install a private worker.

* Suggested Kubernetes cluster configurations:

   * {{site.data.keyword.containerlong_notm}} version 1.15 or higher to run workloads in isolation on {{site.data.keyword.cloud_notm}} Public.
   * {{site.data.keyword.cloud_notm}} Private version 3.1.2 or higher to run non-beta version Tekton workloads on-premises.
   * [Docker Desktop community edition](https://www.docker.com/products/docker-desktop){: external} version 2.0.5.0 to run workloads locally.
  
* System requirements:

   * One 2CORE x 4 GB Kubernetes worker node, such as a Kubernetes Lite cluster.
  
* Network access:

   * Inbound: N/A
   * Outbound network access uses `(TCP:443)` where the region matches the delivery pipeline location and is either au-syd (Sydney, Australia), eu-de (Frankfurt, Germany), eu-gb (London, United Kingdom), jp-tok (Tokyo, Japan), jp-osa (Osaka, Japan), us-south (Dallas, US), us-east (Washington DC, US), br-sao (Sao Paulo), or ca-tor (Toronto, CA). For example, for the Frankfurt region specify `https://private-worker-service.eu-de.devops.cloud.ibm.com (TCP:443)`. For network access to the global endpoint for API key validation, use `https://iam.cloud.ibm.com (TCP:443)`. 
  
* Permissions to pull images from icr.io. Private workers require the tekton-pipelines infrastructure and must be able to pull tekton-releases images from icr.io to complete the private worker installation.

   To pull images from the `icr.io` container registry, you might need to define a specific Kubernetes ClusterImagePolicy.
   {: tip}
  
### Self-hosting container images for {{site.data.keyword.deliverypipeline}} Private Worker
{: #self_host_pw_} 

Security constraints might prevent you from pulling images from the icr.io/continuous-delivery/pipeline container registry. In such scenarios, complete the following steps:

1. Provision the container images on a supported container registry.
2. Install the `deployment.yaml` file to reference the container images in this container registry. 
3. For each container image that is referenced in the regular deployment yaml file, complete the following steps:

   * docker pull the image to a local Dockerfile
   * docker tag the image with the new reference on the supported container registry
   * docker push this new image
 
   You can obtain the deployment yaml file from `https://private-worker-service.$region.devops.cloud.ibm.com/install`.
   {: tip}

4. Replace the reference to each image in the installation file with the tag for the new image.
5. Run the following command to install the private worker by using the specific container registry: `kubectl apply –filename updated_deployment.yaml`.


## Installing a {{site.data.keyword.deliverypipeline}} Private Worker
{: #install_pw}  

{{site.data.keyword.deliverypipeline}} private workers depend on the Tekton and tekton-pipelines infrastructure. Private workers must pull tekton-releases images from `icr.io` (`icr.io/continuous-delivery/pipeline/`). You might need to define a specific Kubernetes ClusterImagePolicy to pull images from these container registries. To add the ClusterImagePolicy type to your Kubernetes cluster, you must [install several Helm charts](/docs/Registry?topic=Registry-security_enforce).

For help with {{site.data.keyword.deliverypipeline}} Private Workers, see [Troubleshooting for {{site.data.keyword.deliverypipeline}} Private Workers](/docs/ContinuousDelivery?topic=ContinuousDelivery-troubleshoot-pipeline-private-workers#troubleshoot-pw-images).
{: tip}

### Installing the {{site.data.keyword.deliverypipeline}} Private Worker by using the CLI
{: #install_pw_cli}
{: cli}

The following steps are intended for Administrators who are preparing environments and private workers for multiple people or teams. To install private workers for your own use, see [Setting up a Delivery Pipeline Private Worker](/docs/ContinuousDelivery?topic=ContinuousDelivery-private-workers#set_up_private_worker).
{: tip}

From the {{site.data.keyword.Bluemix_notm}} CLI, type the following command:

```text
kubectl apply --filename
https://private-worker-service.{REGION}.devops.cloud.ibm.com/install 
```
Where `{REGION}` is the location of the toolchain's pipeline. You can specify any of the following values for the region:

* au-syd (Sydney, Australia)
* eu-de (Frankfurt, Germany)
* eu-gb  (London, United Kingdom)
* jp-tok (Tokyo, Japan)
* jp-osa (Osaka, Japan)
* us-south (Dallas, US)
* us-east (Washington DC, US)
* ca-tor (Toronto, CA)
* br-sao (Sao Paulo, Brazil)

The following code snippet shows an example of a private worker installation in the Frankfurt region:

```text
$ kubectl apply --filename https://private-worker-service.eu-de.devops.cloud.ibm.com/install  

namespace/tekton-pipelines created
podsecuritypolicy.policy/tekton-pipelines created
clusterrole.rbac.authorization.k8s.io/tekton-pipelines-admin created
serviceaccount/tekton-pipelines-controller created
clusterrolebinding.rbac.authorization.k8s.io/tekton-pipelines-controller-admin created
customresourcedefinition.apiextensions.k8s.io/clustertasks.tekton.dev created
customresourcedefinition.apiextensions.k8s.io/images.caching.internal.knative.dev created
customresourcedefinition.apiextensions.k8s.io/pipelines.tekton.dev created
customresourcedefinition.apiextensions.k8s.io/pipelineruns.tekton.dev created
customresourcedefinition.apiextensions.k8s.io/pipelineresources.tekton.dev created
customresourcedefinition.apiextensions.k8s.io/tasks.tekton.dev created
customresourcedefinition.apiextensions.k8s.io/taskruns.tekton.dev created
service/tekton-pipelines-controller created
service/tekton-pipelines-webhook created
clusterrole.rbac.authorization.k8s.io/tekton-aggregate-edit created
clusterrole.rbac.authorization.k8s.io/tekton-aggregate-view created
configmap/config-artifact-bucket created
configmap/config-artifact-pvc created
configmap/config-logging created
deployment.apps/tekton-pipelines-controller created
deployment.apps/tekton-pipelines-webhook created
customresourcedefinition.apiextensions.k8s.io/workeragents.devops.cloud.ibm.com created
deployment.apps/private-worker-agent created
role.rbac.authorization.k8s.io/private-worker-agent created
rolebinding.rbac.authorization.k8s.io/private-worker-agent created
serviceaccount/private-worker-agent created
clusterrolebinding.rbac.authorization.k8s.io/private-worker-agent created

```
To set up a pool of private workers, repeat this process with more Kubernetes clusters.
{: tip}
 

## Registering a {{site.data.keyword.deliverypipeline}} Private Worker
{: #register_pw}

### Creating a service ID
{: #pw_create_service_id}

A service ID represents a pool of one or more private workers that act together. You can initially register one private worker installation, and then incrementally register more private workers into the same group by reusing the same service ID. Registering multiple private workers in the same group supports higher availability and horizontal scaling of your private worker capacity. For more information about service IDs, see [Creating and working with service IDs](/docs/account?topic=account-serviceids).

#### Creating a service ID in the console
{: #console_create_service_id}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}}](http://cloud.ibm.com){: external}.
2. Go to [https://cloud.ibm.com/iam/serviceids](https://cloud.ibm.com/iam/serviceids){: external}.
3. Click **Create**.
4. Enter a name and description for the service ID. If you are creating a service ID for a pool of private workers, specify the name of the private workers pool, for example: **Pipeline Private Workers for Acme**.
5. Click **Create**. 
6. Save your service ID for later use. The service ID is required on the Kubernetes cluster that is targeted for the {{site.data.keyword.deliverypipeline}} Private Worker installation.

#### Creating a service ID by using the CLI
{: #cli_create_service_id}
{: cli}

From the {{site.data.keyword.cloud_notm}} CLI, type the following command:

```text
$ ibmcloud iam service-id-create {worker-pool-name} -d "{worker-pool-description}"

Creating service ID {worker-pool-name} bound to current account as username@domain.com...OK
Service ID {worker-pool-name} is created successfully

Name           {worker-pool-name}
Description    {worker-pool-description}
CRN            crn:v1:bluemix:public:iam-identity::a/8d63fb1cc5e99e86dd7229dddff75fef::serviceid:ServiceId-38ffff31-3ea3-4ecc-9732-190f7a993097
Bound To       crn:v1:bluemix:public:::a/8d63fb1cc5e99e86dd7229dddff75fef:::
Version        1-6df15bde97b6e87f583a557f8731888f
Locked         false
UUID         ServiceId-38ffff31-3ea3-4ecc-9732-190f7a993097

```

### Creating an API key
{: #pw_create_api_key}

An API key is a unique code that is passed to an API to identify the application or user that is calling it. To prevent malicious use of an API, you can use API keys to track and control how that API is used. For more information about API keys, see [Understanding API keys](/docs/account?topic=account-manapikey).

#### Creating an API key in the console
{: #console_create_key}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}}](http://cloud.ibm.com){: external}.
2. Go to [https://cloud.ibm.com/iam/serviceids](https://cloud.ibm.com/iam/serviceids){: external}.
3. Select the Service ID that you want to create an API for.
4. In the **API keys** tab, click **Create**.
5. Enter a name and description for the API key to specify the private worker installation, such as **Pipeline Private Worker in IBM Cloud Private**.
6. Click **Create**.
7. Copy or download your API key. You cannot retrieve your API key again after you create it.

#### Creating an API key by using the CLI
{: #cli_create_key}
{: cli}

From the {{site.data.keyword.cloud_notm}} CLI, type the following command:

```text
$ ibmcloud iam service-api-key-create {worker-api-key-name} (SERVICE\_ID\_NAME|SERVICE\_ID\_UUID) \[-d, --description DESCRIPTION\] \[--file FILE\]

Creating API key  {worker-api-key-name} of service
SERVICE\_ID\_NAME as username@domain.com...
OK
Service API key {worker-api-key-name} is created
Successfully saved API key information to FILE
Please preserve the API key! It cannot be retrieved after it's created.

Name           {worker-api-key-name}
Description    Description
Bound To       crn:v1:bluemix:public:iam-identity::a/2cac145ae78048679b129009cfe8c7f9::serviceid:ServiceId-9a6a14e5-5811-4c2c-9131-0e1d4bb7dfe1
Created At     2019-07-04T10:51+0000
API Key        doJX9kORc4q5PRkH19H3lePDwYRAKNWk4XlIuEBrriOD
Locked         false
UUID           ApiKey-c1ee0fb5-90f2-476e-a260-a796e6d7f5f7

```

### Registering the private worker with {{site.data.keyword.cloud_notm}}
{: #pw_register_cloud}

Before you can register the private worker with {{site.data.keyword.cloud_notm}}, you must deploy the private worker framework by using the following command: `kubectl apply --filename "https://private-worker-service.{REGION}.devops.cloud.ibm.com/install`. To use the registration commands, you must be logged in to the Kubernetes cluster (with kubectl) into which you previously installed a private worker.
{: tip}

You must register a private worker with the specific {{site.data.keyword.cloud_notm}} region that corresponds to the location of the delivery pipelines that you want to enable.

1. Go to `https://private-worker-service.{REGION}.devops.cloud.ibm.com`, where `{REGION}` is the location of the toolchain's pipeline. You can specify any of the following values for the region:

   * au-syd (Sydney, Australia)
   * eu-de (Frankfurt, Germany)
   * eu-gb  (London, United Kingdom)
   * jp-tok (Tokyo, Japan)
   * jp-osa (Osaka, Japan)
   * us-south (Dallas, US)
   * us-east (Washington DC, US)
   * ca-tor (Toronto, CA)
   * br-sao (Sao Paulo, Brazil)

2. Specify a meaningful name for your private worker. This name must start and end with lowercase alphanumeric characters and can also contain `_` or `.` characters.
3. Run the following command with the service ID and API key that you created previously, and the private worker name:

```text
$ kubectl apply --filename "https://private-worker-service.{REGION}.devops.cloud.ibm.com/install/worker?serviceId={SERVICE_ID}&apikey={API_KEY}&name={WORKER_NAME}"

workeragent.devops.cloud.ibm.com/worker-name created
secret/worker-name-auth created

```


To verify that the agent is registered correctly, type the following command:

```text
$ kubectl get workeragents
NAME           SERVICEID     AGENT   REGISTERED   VERSION   AUTH   CONSTRAINED   PAUSED
<worker_name>  <ServiceId>   OK      Succeeded    OK        OK     false         false
```
{: tip}


## Using Private Worker agent attributes
{: #install_pw_agent_states}

The following attributes are available for private worker agents:

* **NAME**: The name that was specified when the agent was registered. This name appears on the Private Worker integration page.
* **SERVICEID**: The work queue ID from which this agent processes work requests.
* **AGENT**: A value of `OK` indicates that the agent can process work requests.
* **REGISTERED**: A value of `Succeeded` indicates that the agent successfully registered with the regional private worker service.
* **VERSION**: A value of `OK` indicates whether the version of the agent is current.
* **AUTH**: A value of `OK` indicates whether the agent `apikey` is valid.
* **CONSTRAINED**: A value of `false` indicates that enough cluster resources are available for the agent to run tasks. A value of `True` specifies that the cluster is resource-constrained.`
* **PAUSED**: A value of `false` indicates that the agent is operational and can run tasks. A value of `true` specifies that the agent is paused and cannot run any tasks. One reason that an agent might be paused is for cluster maintenance.


## Updating the {{site.data.keyword.deliverypipeline}} Private Worker installation
{: #install_pw_update} 

If the private worker is reported as inactive, you must update the installation.

To view the version of your private worker, type the following command: `kubectl -n tekton-pipelines describe deploy private-worker-agent`.
{: tip}

To update your private worker installation, complete the following steps:

1. Run the [installation command](#install_pw) again.
2. [Register the private worker](#register_pw) on your Kubernetes cluster again.

### Provisioning and updating the private worker installation file for IBM Cloud Private
{: #provision_pw_install}

If your pipeline worker is installed on {{site.data.keyword.cloud}} Private (ICP), you can use the following script to provision and update the private worker installation file.

```bash
\#\!/bin/bash
region=${region:-"us-south"}
target_cr="mycluster.icp:8500"
install_filename="updated-private-worker-install.yaml"
curl -o $install_filename
https://private-worker-service.$region.devops.cloud.ibm.com/install
cat $install_filename | grep -e
'gcr.io/tekton-releases/github.com/tektoncd/pipeline/cmd' -e 'image:' \\
| sed 's/- gcr.io/gcr.io/g' \\
| sed 's/- image: gcr.io/gcr.io/g' \\
| sed 's/image: gcr.io/gcr.io/g' \\
| sed 's/image://g' \\
| awk '{$1=$1;print}' \\
| while read -r image ; do

echo "Processing $image"
docker pull $image
new_image_tag=$image
# if $image only have a single slash it is coming from dockerhub
number_of_slashes=$(echo $image | tr -cd '/' | wc -c)
if [ "$number_of_slashes" == "1" ]; then
new_image_tag="$target_cr/$image"
fi

# replace the sha id reference in the tag if any
new_image_tag="${new_image_tag@sha256}"
# replace gcr.io to the target cr domain
new_image_tag="${new_image_tag/gcr.io/$target_cr}"
docker tag $image $new_image_tag
docker push $new_image_tag
# replace the image reference in the installation.yaml file
sed -i "s~$image~$new_image_tag~g" $install_filename
done

echo "*****"
echo "Provisioning of docker images to $target_cr done."
echo "Update of the install file $install_filename done"
echo "Change the scope of the images to global before"
echo "running 'kubectl apply --filename $install_filename'
echo "to install the delivery pipeline private worker"
```

This script contains the following requirements:

* The ibmcom and tekton-releases namespaces currently exist on the target ICP.
* The Docker client is connected to the ICP’s private container registry. For more information about authentication for the Docker CLI, see [Configuring authentication for the Docker CLI](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_images/configuring_docker_cli.html){: external}.   

After you provision the container images on the ICP’s private registry, update the image's scope to global to make sure that the images can be accessed from any namespaces. For more information about updating the scope of an image, see [Changing image scope](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_images/change_scope.html).

You can provide pipeline users with access to the base images (icr.io/continuous-delivery/pipeline/pipeline-base-image) that are used to run pipeline jobs, which are supplied by the global IBM Cloud Container Registry. To use these images, you must configure your pipeline jobs by using the `Custom Dockerimage`. You must also reference the expected image in the ICP’s private registry, for example: `mycluster.icp:8500/icr.io/continuous-delivery/pipeline/pipeline-base-image:latest`.
{: tip}
