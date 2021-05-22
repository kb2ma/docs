# Cloud integrations via cloud block

Cloud service providers like AWS offer valuable tools for collection and management of data, like message queues and data storage, but often setup and use of these services is specific to the cloud provider. At balena our mission is to reduce friction for our fleet owners. So we provide a *cloud block* container, whose aim is to provide a simple, consistent interface to these services via environment variables.

The diagram below shows how the cloud block forwards data to a cloud messaging service. On the left is a balena device with three containers, where a data source publishes data to MQTT on some source topic, to which the cloud block also subscribes. The cloud block then applies the user supplied configuration to forward the data to the provider's message queue service -- whether it's AWS SQS, Azure Event Hubs, or Google Pub/Sub.

![cloud-block-app](/img/integrations/cloud-block/cloud-block-app.png)

The documentation below shows you how to implement this data flow.

 1. Define data source and cloud block services for balena device
 1. Create balena application and environment variables
 1. Push service definitions to balenaCloud
 1. Provision device

To illustrate how the cloud block works, we will use a data source that takes temperature readings from the device's CPU. These readings are readily available and do not require hardware or software setup. We will send these readings to AWS Simple Queue Service (SQS) as JSON data messages.

## Define device services

As shown in the architecture diagram above, our goal is to push data to a source topic on an MQTT broker on the balena device. We then wire the cloud block to subscribe to the source topic. See the [example application](https://github.com/kb2ma/cloudBlock-test/tree/main/cputemp) for the CPU temperature reader that does just that.

As shown in the [docker-compose.yml](https://github.com/kb2ma/cloudBlock-test/blob/main/cputemp/docker-compose.yml) for our example application, you must define three containers as shown in the table below.

| Service    | Notes                                                                                                                                                       |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| data_source|Your service to generate data and publish to an MQTT topic                                                                                                   |
| mqtt       |Generic broker image, like [eclipse-mosquitto](https://hub.docker.com/_/eclipse-mosquitto)                                                                   |
| cloud      |                                                                                         balena block image that subscribes to `data_source` service messages|

Later, you will push these service definitions to balena cloud as described below. Of course you may wish to push the services locally during development, as described in the balena Develop Locally instructions.

### Data source
For our example application, [data_source/main.py](https://github.com/kb2ma/cloudBlock-test/blob/main/cputemp/data_source/main.py) takes a temperature reading every 30 seconds, and publishes the reading to the *cpu_temp* topic. We must adapt the cloud block to subscribe to the *cpu_temp* topic used by the data source. By default the cloud block subscribes to the topic *cloud-block-input*. The default may be overridden by defining an environment variable, as shown in the next section.

## Create application
From your balenaCloud account, create a Microservices or Starter application as described in the balena Getting Started instructions. Next, you must define environment variables for the application that configure the cloud block, as described here. The configuration for the cloud block must identify the AWS SQS queue to use as well as the AWS Identity and Access Management (IAM) identity that is sending messages.

### SQS variables
These variables are specific to use of the AWS SQS service.

| Variable         | Notes                                                                             |
|------------------|-----------------------------------------------------------------------------------|
|AWS_SQS_QUEUE_NAME|Name of the queue. Identified as `<cloud-topic>` in the architecture diagram above.|
|AWS_SQS_REGION    |AWS region in which the queue is defined                                           |
|AWS_SQS_ACCESS_KEY|IAM access key ID                                                                  |
|AWS_SQS_SECRET_KEY|IAM secret key for access key                                                      |

See the [SQS Developer Guide](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) for help setting up the queue service. See the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/index.html) for help setting up the identity. The IAM User must at least be assigned the SQS SendMessage permission.

### Cloud block variables
These variables configure the cloud block on the balena device.

| Variable              | Notes                                                                                                                                             |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
|CLOUD_BLOCK_INPUT_TOPIC|MQTT topic to which the cloud block subscribes for messages from the data source. Identified as `<source-topic>` in the architecture diagram above.|
|CLOUD_BLOCK_DEBUG      |Prints debug messages to the *cloud* service log                                                                                                   |

## Push service definitions to balenaCloud
Once you have defined the application, you may push the service definitions to balenaCloud with the `balena push` CLI command. See the balena Getting Started instructions for details.

## Provision device
Finally, provision a device with the application you creatd!