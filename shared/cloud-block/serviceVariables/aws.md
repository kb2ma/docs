
### SQS variables
These variables are specific to use of the AWS SQS service.

| Variable         | Notes                                                                             |
|------------------|-----------------------------------------------------------------------------------|
|AWS_SQS_QUEUE_NAME|Name of the queue. Identified as `<cloud-topic>` in the architecture diagram above.|
|AWS_SQS_REGION    |AWS region in which the queue is defined                                           |
|AWS_SQS_ACCESS_KEY|IAM access key ID                                                                  |
|AWS_SQS_SECRET_KEY|IAM secret key for access key                                                      |

See the [SQS Developer Guide](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) for help setting up the queue service. See the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/index.html) for help setting up the identity. The IAM User must at least be assigned the SQS *SendMessage* permission.
