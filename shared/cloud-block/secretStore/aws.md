
A secret store itself also requires some setup and authentication values to allow access to its secrets. These values *must* be provided as balena Environment Variables. AWS Secrets Manager requires the environment variables below.


| Variable                | Notes                                                                             |
|-------------------------|-----------------------------------------------------------------------------------|
|AWS_SECRETSMGR_REGION    |AWS region in which the secret store is defined                                    |
|AWS_SECRETSMGR_ACCESS_KEY|IAM access key ID                                                                  |
|AWS_SECRETSMGR_SECRET_KEY|IAM secret key for access key                                                      |

See the AWS Secrets Manager [User Guide](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) for help with setup. See the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/index.html) for general help with users and security.

When actually adding secrets, add them simply as Plaintext -- no separate key and value. AWS Secrets Manager console defaults to a "compound" secret that contains a *list* of key:value pairs rather than just a single plaintext value.
