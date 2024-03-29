# CloudQuery on AWS:

### Note: This template is an Alpha Feature, which means there might be issues but we are interested in your feedback

For fully supported deployment methods check out the Helm Chart and associated Terraform found on our [Documentation Site](https://www.cloudquery.io/docs/deployment/overview)



## Overview
This Cloudformation Template enables users to quickly setup an environment for using CloudQuery fetch functionality. This template creates resources that are not included in the Free tier. 

![Architecture Overview](https://user-images.githubusercontent.com/30294676/186777055-aa1022e2-c80f-47c4-973f-25e44c74ff68.png)

### Components:
- **Schedule Trigger**: A CloudWatch Rule that triggers periodically based on a schedule. The event contains the url of the configuration file stored in S3 that the fetch task will use. 
- **Distributed Lock**: A [StepFunction](https://aws.amazon.com/blogs/compute/controlling-concurrency-in-distributed-systems-using-aws-step-functions/) that ensures that only a task is running for any single configuration file.
- **Configuration Storage**: S3 bucket that is configured to allow the Task Role permission to perform GetObject
- **Postgres Database**: An Aurora Serverless Database where CloudQuery stores all of the data that it fetche


## Capabilities:
- Automate deployment of AWS resources using native tooling
- Fetch resources on a recurring basis

## Running:

1. Clone repo
2. Install aws cli v2
3. Set the name of your stack:
```bash
export STACKNAME=CloudQuery-Deployment
```
4. Deploy the stack: 
```bash
make deploy
```
5. Update the config file to specify the resources and regions you want to grab (by default it will grab all resources in all enabled regions)
6. Copy `cloudquery.yml` to s3 created by cloudformation
```
make store-config
```
7. You can manually run a fetch by running the following command:
```
make run-task
``` 
Note: this assumes that the cloudquery configuration file is located in the s3 bucket that was created by the template `s3://<bucket_name>/cloudquery.yml`

### Current Limitations:
- [ ] Support user defined VPC
- [x] Allow user to specify CQ core version
  - [ ] Needs documentation
- [ ] Enable user to define custom policy
- [ ] Provide way of interacting with external database
- [ ] Support using AWS RDS IAM credentials instead of admin credentials
- [ ] Document how to support multiple parallel cq fetches


## Send us feedback
To post feedback, submit feature ideas, or report bugs, use the Issues section of the GitHub repository or reach out to us on Discord.

