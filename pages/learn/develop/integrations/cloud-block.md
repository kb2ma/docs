# Cloud integrations via cloud block

Cloud service providers like AWS offer valuable tools for collection and management of data, like message queues and data storage, but setup and use of these services is adapted to the cloud provider. At balena our mission is to reduce friction for our fleet owners. So we provide a *cloud block* container, whose aim is to provide an interface as consistent as possible for a fleet owner to push data to the cloud.

The diagram below shows how the cloud block forwards data to a cloud messaging service. On the left is a balena device with three containers, where a data source publishes data to MQTT on some *<source-topic>* topic, to which the cloud block also subscribes. The cloud block then uses a user supplied configuration to forward the data to the provider's message queue service.

![cloud-block-app](/img/integrations/cloud-block/cloud-block-app.png)

The documentation below shows you how to implement this data flow.

 1. Integrate the data source with the cloud block
 2. Cloud service setup
 3. Create required environment variables
 4. Provision device

To illustrate how the cloud block works, we will use a data source that takes temperature readings from the device's CPU. These readings are readily available and do not require hardware or software setup.

## Integrate data source

Our goal is to push data to a source topic on an MQTT broker on the balena device. We then adapt the cloud block to subscribe to the source topic. We have created a [test application](https://github.com/kb2ma/cloudBlock-test/tree/main/cputemp) for the CPU temperature reader that does just that.

| File                                                                                                 | Discussion                                                                                                                                                                                                        |
|------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [data_source/main.py](https://github.com/kb2ma/cloudBlock-test/blob/main/cputemp/data_source/main.py)| Takes a reading every 30 seconds, and publishes to the *cpu_temp* topic                                                                                                                                           |
| [docker-compose.yml](https://github.com/kb2ma/cloudBlock-test/blob/main/cputemp/docker-compose.yml)  | Includes three containers -- *cloud* for the cloud block, *mqtt*, and *data_source*. Adapts the cloud block to subscribe to the *cpu_temp* topic used by *data_source* with the `MQTT_INPUT` environment variable.|





