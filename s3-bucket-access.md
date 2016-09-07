---
title: S3 Bucket Access
category: storage
summary: How can I access my Catalyze S3 Bucket?
---

# How can I access my Catalyze S3 Bucket?

When Catalyze creates S3 Buckets for Stratum environments, we place three environment variables in your environment:

- `S3_BUCKET`
- `S3_ACCESS_KEY_ID`
- `S3_SECRET_ACCESS_KEY`

Catalyze also sets a number of [bucket policies](/stratum/articles/s3-bucket-policies) that secure the bucket contents.

## Using the AWS CLI

For moving large amounts of content into your bucket, the [AWS CLI](https://aws.amazon.com/cli/) can be a handy way for interacting with the environment.

### Setup your S3 Credentials

Use the below command and add in your `S3_ACCESS_KEY_ID` and `S3_SECRET_ACCESS_KEY`

`aws configure`

### Use the AWS S3 Commands

After setting up credentials, you can use the `aws s3` sub command to interact with the container.

Example Commands:

- List Bucket
  - `aws s3 ls s3://your_bucket_name/`
- Copy Single File to Bucket
  - `aws s3 cp local_file s3://your_bucket_name/ --sse`
- Copy Folder to Bucket
  - `aws s3 cp local_folder s3://your_bucket_name/ --recursive --sse`

Note that copying data to the bucket ***requires*** the [server side encryption flag (--sse)](/stratum/articles/s3-bucket-policies#require-server-side-encryption) due to bucket policies.

See [here](http://docs.aws.amazon.com/cli/latest/reference/s3/index.html) for more details.

For using the S3 API routes, see [here](http://docs.aws.amazon.com/cli/latest/reference/s3api/index.html).

## Using Your Application

Most development frameworks contain libraries for interacting with the S3 API. Those libraries can make the task of interacting with S3 much simpler.

[Amazon SDKs](https://aws.amazon.com/tools/)

## Bucket Region

All S3 buckets setup by Catalyze are put in the `US Standard` region. When specification of the region is required for an SDK that you are using, you should specify the `us-east-1` region.
