---

copyright:
  years: 2016, 2021
lastupdated: "2021-09-13"

keywords: environment properties, environment resources, IBM Java, pipeline environments

subcollection: ContinuousDelivery

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Environment properties and resources
{: #deliverypipeline_environment}

The following properties and resources are available by default in {{site.data.keyword.contdelivery_full}} {{site.data.keyword.deliverypipeline}} environments.

## Environment properties
{: #deliverypipeline_envprop}

### General-purpose properties
The following table lists and describes each of the general-purpose environment properties that are available by default in pipeline environments.

| Environment Property | Description |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| ARCHIVE_DIR | The directory to archive or to download archives into. |
| BUILD_ID | The unique ID for the current job execution.  |
| BUILD_DISPLAY_NAME | The BUILD_ID value, prefixed with "#". |
| BUILD_NUMBER | The incremental stage ID that is shown in the pipeline UI.  |
| GIT_BRANCH | The Git branch that the job uses as input. This property is only available in jobs that use a Git repository as input. |
| GIT_COMMIT | The Git commit that the job uses as input. This property is only available in jobs that use a Git repository as input. |
| GIT_PREVIOUS_COMMIT | The Git commit value of the job's last successful run. This property is only available in jobs that use a Git repository as input. |
| GIT_URL | The Git repository URL that the job uses as input. This property is only available in jobs that use a Git repository as input. |
| IDS_JOB_ID | The unique ID of the job's configuration. |
| IDS_JOB_NAME | The name of the job's configuration. |
| IDS_OUTPUT_PROPS | Comma-separated names of your stage environment properties. |
| IDS_PROJECT_NAME | The name of the project, for example, <code>Owner - Project Name</code>. |
| IDS_STAGE_NAME | The name of the current stage. |
| IDS_URL | The URL for the current pipeline. |
| IDS_VERSION | The number for the build that is being deployed or the SCM identifier. This property is available only in deploy jobs.
| JOB_NAME | The unique job ID in the context of the current pipeline. |
| PIPELINE_ARTIFACT_URL | The URL that you can use to download the artifacts of the current Build job after the job completes. You must use a valid Bearer token to access the artifacts. |
| PIPELINE_INITIAL_STAGE_EXECUTION_ID | The unique ID for the run of the pipeline. |
| PIPELINE_IMAGE_URL | The URL of the container image that is built by a Container Registry build job. This property is present only in a job whose stage input is a Container Registry build job. This input Container Registry build job must explicitly export `PIPELINE_IMAGE_URL`, for example: `export PIPELINE_IMAGE_URL=$REGISTRY_URL/$REGISTRY_NAMESPACE/$IMAGE_NAME:$BUILD_NUMBER`. |
| PIPELINE_KUBERNETES_CLUSTER_NAME | The name of the Kubernetes cluster that is selected in the current job. |
| PIPELINE_LOG_URL | The URL that you can use to download the log file of the current job after the job completes. You must use a valid Bearer token to access the log files. |
| PIPELINE_STAGE_INPUT_JOB_ID | The ID of the job that is input for the current stage. |
| PIPELINE_STAGE_INPUT_REV | The revision of the input for the current stage. |
| PIPELINE_TRIGGERING_USER | The current user for the pipeline job|
| TASK_ID | The unique ID of the job's current run. |
| TMPDIR | A directory location where temporary files are stored. |
| WORKSPACE | The path for the current working directory. |
{: caption="Table 1. General-purpose environment properties" caption-side="top"}

### Runtime and tool properties
The following table lists and describes each of the runtime and tool environment properties that are available by default in pipeline environments.

| Environment Property | Description |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| ANT_HOME | The path to Apache Ant 1.9.2. |
| ANT_JAVA8_HOME | The path to a 1.10+ version of Apache Ant that requires Java 8. |
| GRADLE_HOME | The path to Gradle 1.11. |
| JAVA_HOME | The path to IBM&reg; Java&trade; 7. |
| JAVA7_HOME | The path to IBM Java 7. |
| JAVA8_HOME | The path to IBM Java 8. |
| MAVEN_HOME | The path to Apache Maven 3.2.1. |
| NODE_HOME | The path to Node.js 0.10.29. |
{: caption="Table 2. Runtime and tool environment properties" caption-side="top"}

To use Apache Ant 1.10+ in your pipeline's scripts, set `ANT_HOME` to `$ANT_JAVA8_HOME` and `JAVA_HOME` to `$JAVA8_HOME`.
{: tip}

### Deployment properties
The following table lists and describes each of the deployment environment properties that are available by default in pipeline environments.

| Environment Property | Description |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| CF_APP | For deployments, the name of the app to deploy. This property is required for deployment and can be specified in the script itself, the deploy job configuration interface, or the project's `manifest.yml` file. |
| CF_ORG | For deployments, the name of the organization (org) to deploy to. |
| CF_ORGANIZATION_ID | For deployments, the ID of the org to deploy to. |
| CF_SPACE | For deployments, the name of the space to deploy to. |
| CF_SPACE_ID | For deployments, the ID of the space to deploy to.  |
| CF_TARGET_URL | For deployments, the URL of {{site.data.keyword.Bluemix_short}} or Cloud Foundry. |
| IDS_VERSION | For deployments, the version of the app that is being deployed or the source identifier. |
{: caption="Table 3. Deployment environment properties" caption-side="top"}

## Preinstalled resources
{: #deliverypipeline_resources}

Several runtimes, tools, and Node modules are preinstalled in every pipeline.

### Runtimes and tools
The following table lists each of the preinstalled runtimes and tools that are available in every pipeline.

All links are in the home directory.
{: tip}

| Resource | Link Name | Path |
|----------|-----------|-----------|
|Apache Ant 1.9.2|ant |/opt/IBM/ant |
|Cloud Foundry CLI 6.14 |cf | /opt/IBM/cf |
|Gradle 1.12|gradle |/opt/IBM/gradle |
|Gradle 2.9 |gradle2 |/opt/IBM/gradle2 |
|IBM Cloud CLI (bx) 0.13.0 |ibmcloud |/usr/local/ibmcloud/bin/ibmcloud |
|IBM Java (default)|java |/opt/IBM/java |
|IBM Java 7 x86_64-71 |java7 |/opt/IBM/java7 |
|IBM Java 8 x86_64-80|java8 |/opt/IBM/java8 |
|Apache Maven 3.2.1 |maven |/opt/IBM/maven |
|IBM Node |node |/opt/IBM/node |
|IBM Rational Team Concert&trade; SCM Tools |RTC-SCM-Tools |/opt/IBM/RTC-SCM-Tools |
{: caption="Table 4. Link names and paths for runtimes and tools" caption-side="top"}

The pipeline environment offers 64-bit versions of IBM Node 0.10, 0.10.48, 0.12, 0.12.17, 4.2, 4.4.5, 4.6.0, 6.2.2, and 6.7.0. To choose a version, use the export command.

For example, to use Node 0.12.7, type this command: `export PATH=/opt/IBM/node-v0.12/bin:$PATH`

To use Node 4.2.2, type this command: `export PATH=/opt/IBM/node-v4.2/bin:$PATH`

### Node modules

The following Node modules are globally installed in the pipeline environment:

* grunt@0.4.5
* grunt-cli@0.1.13
* grunt-contrib-concat@0.4.0
* grunt-contrib-jshint@0.10.0
* grunt-contrib-nodeunit@0.4.1
* grunt-contrib-qunit@0.5.1
* grunt-contrib-uglify@0.5.0
* grunt-contrib-watch@0.6.1
* karma@0.12.23
* karma-cli@0.0.4
* karma-jasmine@0.1.5
* karma-phantomjs-launcher@0.1.4
* phantomjs@1.9.10
