---

copyright:
  years: 2015, 2022
lastupdated: "2022-02-07"

keywords: Delivery Pipeline, tool integration, toolchains, yaml, pipeline jobs

subcollection: ContinuousDelivery

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# FAQs for {{site.data.keyword.deliverypipeline}}
{: #faq_delivery_pipeline}

Get answers to frequently asked questions about using {{site.data.keyword.deliverypipeline}}.
{: shortdesc} 


## How do I pass artifacts between pipeline jobs?
{: #artifacts_pipeline_jobs}
{: faq}
{: support}

When a stage runs, the stage's input is passed to each of the jobs in the stage. Each job is given a clean container to run in. As a result, jobs within a stage cannot pass artifacts to each other. To pass artifacts between jobs, separate the jobs into two stages, and use the output from the job in the first stage as input to the second stage.

For more information about pipeline jobs, see [Jobs](/docs/ContinuousDelivery?topic=ContinuousDelivery-deliverypipeline_about#deliverypipeline_jobs).


## Is there a maximum time limit that my pipeline jobs can run?
{: #pipeline_jobs_time_limit}
{: faq}
{: support}

The lengths of Classic pipeline jobs and Tekton pipeline runs are determined by the private worker that the pipeline run occurs on. On IBM Managed workers, this value is 6 hours. On self-managed [{{site.data.keyword.deliverypipeline}} Private Workers](/docs/ContinuousDelivery?topic=ContinuousDelivery-install-private-workers), the default length of time for a pipeline run is 24 hours.

## How secure are the pipeline secure properties?
{: #pipeline_secure_properties}
{: faq}

Pipeline secure properties are encrypted by using AES-128, and decrypted immediately before they are passed to your pipeline script. These properties are also masked by using asterisks in the properties user interface and in your pipeline log files. Before data is written to the log file for your pipeline job, it is scanned for exact matches to all of the values in the pipeline secure properties. If a match is found, it is masked by using asterisks. Be careful when you are working with secure properties and log files since only exact matches are masked. 


## How do I find information about the environment variables that are used in pipeline jobs?
{: #pipeline_environment_variables}
{: faq}
{: support}

For information about the environment properties and resources that are available by default in pipeline environments, see [Environment properties and resources](/docs/ContinuousDelivery?topic=ContinuousDelivery-deliverypipeline_environment#deliverypipeline_environment).


## Can I use the {{site.data.keyword.cloud_notm}} CLI to run a pipeline stage?
{: #pipeline_stage_cli}
{: faq}
{: support}

You can use the {{site.data.keyword.cloud_notm}} developer tools CLI plug-in to run a pipeline stage.

1. [Install the {{site.data.keyword.cloud_notm}} developer tools CLI plug-in](/docs/cli?topic=cli-install-devtools-manually).
1. From the command line, run the following command to manually start your pipeline:

   ```text
   ibmcloud dev pipeline-run pipelineID --stage-id stageID
   ```
   
For more information about the `pipeline-run` command, see [pipeline-run](/docs/cli?topic=cli-idt-cli#pipeline-run).


## Can I download the yaml file for a delivery pipeline?
{: #pipeline_export_yaml}
{: faq}
{: support}

You can export the definition for an entire pipeline by appending `/yaml` to the pipeline URL. For more information about exporting the definition for an entire pipeline, see [Modifying, exporting, and deleting {{site.data.keyword.contdelivery_short}} pipeline data](/docs/ContinuousDelivery?topic=ContinuousDelivery-cd_personal_data#managing_pipeline_data).
