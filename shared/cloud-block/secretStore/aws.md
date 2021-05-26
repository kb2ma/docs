
AWS Secrets Manager requires these environment variable to access secret values.


| Variable                | Notes                                                                             |
|-------------------------|-----------------------------------------------------------------------------------|
|AWS_SECRETSMGR_REGION    |AWS region in which the secret store is defined                                    |
|AWS_SECRETSMGR_ACCESS_KEY|IAM access key ID                                                                  |
|AWS_SECRETSMGR_SECRET_KEY|IAM secret key for access key                                                      |

See the AWS Secrets Manager [User Guide](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) for help setting up the queue service. See the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/index.html) for help with secret store setup.

When actually adding secrets, add them simply as Plaintext -- no separate key and value. AWS Secrets Manager console defaults to a "compound" secret that contains a *list* of key:value pairs rather than just a single plaintext value.
