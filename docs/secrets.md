# Serverless Framework Enterprise - AWS Secrets

Serverless Framework Enterprise AWS Secrets help you secure your service deployments by enabling Serverless Framework Enterprise to issue temporary AWS Access Keys to deploy your services to AWS.

A Serverless Framework service without the Enterprise Plugin uses the AWS Access Keys stored in [environment variables](https://serverless.com/framework/docs/providers/aws/guide/credentials/) or [AWS Profiles](https://serverless.com/framework/docs/providers/aws/guide/credentials/). With Serverless Framework Enterprise Secrets the AWS Access Keys are generated by Serverless Enterprise on every command and the credentials expire after one hour.

## Minimum Version and Enterprise Plugin Requirements

In order to enable Serverless Secrets for a Service you must update your Service configuration to use Serverless Framework open-source CLI version 1.37.1 or later, and Enterprise Plugin 0.2.0 or later.

## Link your AWS Account

1. Open https://dashboard.serverless.com/
2. Once logged in, click "**secure**" near the top of the page.
3. Click the "**+ add**" button on the right, and choose "**aws**".
4. In the modal that appears, click "**Click here to add a role**".
5. Login to **your AWS account** if prompted, and you should arrive at AWS's role creation wizard.
6. Click "**Next: Permissions**" to proceed to the permissions page.
7. Tick the box next to the **AdministratorAccess** policy.
8. Click "**Next**" two more times to proceed to the Review page.
9. On the review page, enter any distinctive name (e.g. "ServerlessEnterprise") in the "**Role name**" field.
10. Click the "**Create role**" button and you will be brought back to the list of IAM roles.
11. Locate the role you just created in the list, and click its name.
12. On the summary page, next to the "**Role ARN**", click the copy icon.
13. Switch back to the Serverless Enterprise tab, and paste the copied ARN into the ARN blank for step 3 of the modal.
14. For step 4 of the modal, provide a descriptive name for your key (e.g. `sls-api-services`).
15. Click "**create secret**".
16. Under the "**Add Applications**" select the individual apps or keep “_Allow any application in this tenant to deploy to this account_.” selected. Only selected applications will be able to generate the AWS Access Keys.
17. Once you've made your selection, click "**save changes**".

## Set up the service

In the `serverless.yml` file add the `credentials` setting to the `aws` provider and use the new `secrets` variable.  Replace `sls-api-services` with the name provided in step 14 in the previous section. 

**serverless.yml**
```yaml
provider:
  name: aws
  credentials: ${secrets:sls-api-services}
```

## Test your configuration

Before you run `serverless deploy` ensure that the Serverless Framework open-source CLI is resolving the new AWS Access Keys from Enterprise.

```
serverless print
```

You should expect to see output similar to this one, which includes values for the `accessKeyId`, `secretAccessKey` and `sessionToken` under `credentials` for the `aws` provider.

```yaml
provider:
  name: aws
  credentials:
    accessKeyId: ASOAQ5RKZXALPDISYS34
    secretAccessKey: D3DRPdC9m6vrKOHIPTHw/+8xyl/c62h4gw0xrzlV
    sessionToken: >-
      FQoGZXIvYXdzEHIaDJNjSW4JAsUNWcnpzCKNAqEPjj75DyALiUg5yonFhE9o6rLV3VgH+dg4tZ9WuZBvS1V6Cf8/Tk8cpf7vE3cDrpEDXpNm1Q51bwJnQk7L1S+E5hFK9CFIE/ICyv5HLmxyWqtDHgyyExYljwnovlQz5azmvKJLCjeMF
```

## Deploy using Secrets

That’s it! You are now ready to deploy using the new Serverless Framework Enterprise Secrets.

```
serverless deploy
```
