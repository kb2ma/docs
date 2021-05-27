---
dynamic:
  variables: [ $cloud ]
  ref: $original_ref/$cloud
  $switch_text: Cloud Block reference for $cloud
---
# Cloud Block Reference for {{ $cloud.name }}

The cloud block allows you to easily send data to a cloud service like a message queue. The [services page](cloud-block/aws) describes how to use available services. However, the cloud block includes generic capabilities across all services that are worth a separate description here.

## What's a block?
A block is a smaller, modular chunk of functionality for some useful task on a balena device. See the introductory [blog post](https://www.balena.io/blog/introducing-balenablocks-jumpstart-your-iot-app-development/). The cloud block accepts a collection of configuration variables to set up a common cloud service. Then you can simply push data to the cloud block, and the block forwards it to the configured services. See the diagram below in the configuration section.

## Autoconfiguration
Each cloud service requires the value of certain configuration and authentication variables to function. For example, suppose you wish to use Azure Event Hubs. In this case you must define a connection string (AZURE_EH_CONNECTIONSTRING), a storage account (AZURE_EH_STORAGE_ACCOUNT), and so on. However, you do not need to explicitly activate Event Hubs as a whole. Simply defining all the required environment variables is sufficient to activate use of the service.

This autoconfiguration capability also means you may send data to *multiple* cloud services or cloud providers for each message received on the MQTT input queue on a device. As long as the required environment variables for a cloud service are defined, the cloud block will attempt to use it.

## Configuration via Secret Store

{{ $cloud.name }} includes a secret store service, {{ $cloud.secretStoreFullName }}. Like it sounds, a secret store is used to store and access secret values, accessed by name like an environment variable. In fact, the services we wish to configure are housed with the cloud provider. So actually it is safer and more convenient to store the service configuration values as secrets with the provider in the cloud rather than specify them separately as Environment Variables in balenaCloud.

In addition a security best practice is to periodically rotate secret values just in case a value has been compromised without your knowledge. Rotation is easier to implement when the the secret values are defined with the cloud service provider.

The diagram below shows the steps to setup and run the cloud block. The cloud block includes a Configurator that initially reads *both* environment variables and secret values from a secret store. Next the configurator connects to the cloud data service with the required service and authentication values. Then the block is ready to push your application data to the cloud service.

![cloud-block-config](/img/integrations/cloud-block/cloud-block-configuration.png)

{{import "cloud-block/secretStore"}}
