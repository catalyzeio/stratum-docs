---
title: S3 Bucket Policies
---

# How are Catalyze S3 Buckets managed?

Catalyze sets a variety of default policies on S3 Buckets to achieve compliant behavior!

## Bucket Policies

### Require Server Side Encryption

Catalyze requires all POST/PUT operations to S3 Buckets to specify server-side encryption.

This policy affects API and command-line interactions with S3 buckets.

You ***WILL*** receive `Access Denied` errors if you attempt a PUT/POST without a server-side-encryptoin flag or header.

When using the AWS CLI, you'll need to add the `--sse` flag to your command, such as

```
aws s3 cp myfile.txt s3://your_bucket_name/ --sse
```

For some SDKs, you may have to set the `x-amz-server-side-encryption` header on the request instead. The value of this header should be set to `AES256`. For example, the header on the upload request should look like

```
x-amz-server-side-encryption: AES256
```

## CORS Policies

Catalyze enables the default CORS policy on S3 buckets. You can manipulate the CORS policies yourself with the AWS CLI `s3api` routes.

Read more about manipulating those [here](http://docs.aws.amazon.com/cli/latest/reference/s3api/put-bucket-cors.html)

Alternatively you can contact [Support](/contact.md/) and we can assist.
