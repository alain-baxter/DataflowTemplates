Pub/Sub to Pub/Sub Template
---
Streaming pipeline. Reads from a Pub/Sub subscription and writes to a Pub/Sub topic. .

:memo: This is a Google-provided template! Please
check [Provided templates documentation](https://cloud.google.com/dataflow/docs/guides/templates/provided/pubsub-to-pubsub)
on how to use it without having to build from sources.

:bulb: This is a generated documentation based
on [Metadata Annotations](https://github.com/GoogleCloudPlatform/DataflowTemplates#metadata-annotations)
. Do not change this file directly.

## Parameters

### Mandatory Parameters

* **inputSubscription** (Pub/Sub input subscription): Pub/Sub subscription to read the input from, in the format of 'projects/your-project-id/subscriptions/your-subscription-name' (Example: projects/your-project-id/subscriptions/your-subscription-name).
* **outputTopic** (Output Pub/Sub topic): The name of the topic to which data should published, in the format of 'projects/your-project-id/topics/your-topic-name' (Example: projects/your-project-id/topics/your-topic-name).

### Optional Parameters

* **filterKey** (Event filter key): Attribute key by which events are filtered. No filters are applied if no key is specified.
* **filterValue** (Event filter value): Filter attribute value to use if an event filter key is provided. Accepts a valid Java Regex string as an event filter value. In case a regex is provided, the complete expression should match in order for the message to be filtered. Partial matches (e.g. substring) will not be filtered. A null event filter value is used by default.

## Getting Started

### Requirements

* Java 11
* Maven
* Valid resources for mandatory parameters.
* [gcloud CLI](https://cloud.google.com/sdk/gcloud), and execution of the
  following commands:
    * `gcloud auth login`
    * `gcloud auth application-default login`

The following instructions use the
[Templates Plugin](https://github.com/GoogleCloudPlatform/DataflowTemplates#templates-plugin)
. Install the plugin with the following command to proceed:

```shell
mvn clean install -pl plugins/templates-maven-plugin -am
```

### Building Template

This template is a Classic Template, meaning that the pipeline code will be
executed only once and the pipeline will be saved to Google Cloud Storage for
further reuse. Please check [Creating classic Dataflow templates](https://cloud.google.com/dataflow/docs/guides/templates/creating-templates)
and [Running classic templates](https://cloud.google.com/dataflow/docs/guides/templates/running-templates)
for more information.

#### Staging the Template

If the plan is to just stage the template (i.e., make it available to use) by
the `gcloud` command or Dataflow "Create job from template" UI,
the `-PtemplatesStage` profile should be used:

```shell
export PROJECT=<my-project>
export BUCKET_NAME=<bucket-name>

mvn clean package -PtemplatesStage  \
-DskipTests \
-DprojectId="$PROJECT" \
-DbucketName="$BUCKET_NAME" \
-DstagePrefix="templates" \
-DtemplateName="Cloud_PubSub_to_Cloud_PubSub" \
-pl v1 \
-am
```

The command should build and save the template to Google Cloud, and then print
the complete location on Cloud Storage:

```
Classic Template was staged! gs://<bucket-name>/templates/Cloud_PubSub_to_Cloud_PubSub
```

The specific path should be copied as it will be used in the following steps.

#### Running the Template

**Using the staged template**:

You can use the path above run the template (or share with others for execution).

To start a job with that template at any time using `gcloud`, you can use:

```shell
export PROJECT=<my-project>
export BUCKET_NAME=<bucket-name>
export REGION=us-central1
export TEMPLATE_SPEC_GCSPATH="gs://$BUCKET_NAME/templates/Cloud_PubSub_to_Cloud_PubSub"

### Mandatory
export INPUT_SUBSCRIPTION=<inputSubscription>
export OUTPUT_TOPIC=<outputTopic>

### Optional
export FILTER_KEY=<filterKey>
export FILTER_VALUE=<filterValue>

gcloud dataflow jobs run "cloud-pubsub-to-cloud-pubsub-job" \
  --project "$PROJECT" \
  --region "$REGION" \
  --gcs-location "$TEMPLATE_SPEC_GCSPATH" \
  --parameters "inputSubscription=$INPUT_SUBSCRIPTION" \
  --parameters "outputTopic=$OUTPUT_TOPIC" \
  --parameters "filterKey=$FILTER_KEY" \
  --parameters "filterValue=$FILTER_VALUE"
```

For more information about the command, please check:
https://cloud.google.com/sdk/gcloud/reference/dataflow/jobs/run


**Using the plugin**:

Instead of just generating the template in the folder, it is possible to stage
and run the template in a single command. This may be useful for testing when
changing the templates.

```shell
export PROJECT=<my-project>
export BUCKET_NAME=<bucket-name>
export REGION=us-central1

### Mandatory
export INPUT_SUBSCRIPTION=<inputSubscription>
export OUTPUT_TOPIC=<outputTopic>

### Optional
export FILTER_KEY=<filterKey>
export FILTER_VALUE=<filterValue>

mvn clean package -PtemplatesRun \
-DskipTests \
-DprojectId="$PROJECT" \
-DbucketName="$BUCKET_NAME" \
-Dregion="$REGION" \
-DjobName="cloud-pubsub-to-cloud-pubsub-job" \
-DtemplateName="Cloud_PubSub_to_Cloud_PubSub" \
-Dparameters="inputSubscription=$INPUT_SUBSCRIPTION,outputTopic=$OUTPUT_TOPIC,filterKey=$FILTER_KEY,filterValue=$FILTER_VALUE" \
-pl v1 \
-am
```