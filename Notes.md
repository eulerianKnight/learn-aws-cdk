# AWS Cloud Development Kit

## Commands

```
- cdk init app --language typescript: Initialize a CDK application with default template and preferred language.
- cdk bootstrap: Make a starter environment for all our application.
- cdk synth: Generates a Cloud Formation template for each stack.
- cdk deploy: Deploy the stacks.
- cdk list: List the stacks.
- cdk diff: Shows the difference between locally and remotely deployed CloudFormation environment.
- cdk doctor: Checks settings, versions, libraries and shows if there is any problem.
- cdk destroy <name-of-stack>: Delete stack 
```

## AWS CDK Constructs

- Basic building blocks of CDK application.
- Three levels of CDK constructs:
    - Low level constructs(L1): Cloud Formation resources. When used, we must configure all parameters. Most AWS resources are migrated to L2. Use for new services that are still not migrated.
    - AWS resources with higher level(L2): CDK provides additional functionality like defaults, boiler plate and type safety for many parameters. Used most of the time.
    - Patterns(L3): Combine multiple types of resources and help with common tasks in AWS. Example - LambdaRestApi. It is used as per matter of preference of company policy.
- In case of Cloud Formation, each stack has only L1 Construct. In case of CDK, each stack can have L1, L2 or L3 construct. 
- `cdk synth` generates only L1 construct.
- Creating a S3 bucket in 3 different level of constructs:
    ```
    ##### L1 Construct(Add in Constructor) #####

    new CfnBucket(this, 'MyL1Bucket', {
      lifecycleConfiguration: {
        rules: [{
          expirationInDays: 2, 
          status: 'Enabled'
        }]
      }
    });

    ##### L2 Construct #####

    new Bucket(this, 'MyL2Bucket', {
      lifecycleRules: [{
        expiration: cdk.Duration.days(2)
      }]
    });


    ##### L3 Construct #####
    # Create a class 
    class L3Bucket extends Construct {
        constructor(scope: Construct, id: string, expiration: number) {
            super(scope, id);

            new Bucket(this, 'L3Bucket', {
                lifecycleRules: [{
                    expiration: cdk.Duration.days(expiration)
                }]
            })
        }
    }
    new L3Bucket(this, 'MyL3Bucket', 2);
    ```
