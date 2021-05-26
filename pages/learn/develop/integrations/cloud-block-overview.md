---
dynamic:
  variables: [ $cloud ]
  ref: $original_ref/$cloud
  $switch_text: Cloud Block overview for $cloud
---
# Cloud Block Overview for {{ $cloud.name }}

The cloud block allows you to easily send data to a cloud service like a message queue. The [services page](cloud-block/aws) describes how to use available services. However, the cloud block includes generic capabilities across all services that are worth calling out separately.

## Autoconfiguration
Each cloud service requires the value of certain environment variables to function. For example, suppose you wish to use Azure Event Hubs. In this case you must define include a connection string (AZURE_EH_CONNECTIONSTRING), a storage account (AZURE_EH_STORAGE_ACCOUNT), and so on. However, you do not need to explicitly activate Event Hubs as a whole. Simply defining all the required environment variables is sufficient to activate use of the service.

This autoconfiguration capability also means you may send data to multiple cloud services or cloud providers for each message received on the MQTT input queue on a device. As long as the required environment variables for a cloud service are defined, the cloud block will attempt to use it.

## Configuration via Secret Store

{{ $cloud.name }} includes a secret store service, {{ $cloud.secretStoreFullName }}. Like it sounds, a secret store is used to store and access secret values, accessed by name like an environment variable. However, since the services we wish to configure are housed with the cloud provider, it is safer and more convenient actually to store the secrets with the provider in the cloud rather than specify them explicitly as Environment Variables in balenaCloud. In addition it is easier to periodically rotate the secret values as a security best practice just in case a secret value has been compromised without your knowledge.

![cloud-block-config](/img/integrations/cloud-block/cloud-block-configuration.png)

{{import "cloud-block/secretStore"}}
