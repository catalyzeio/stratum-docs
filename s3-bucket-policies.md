# How are Catalyze S3 Buckets managed?

Catalyze sets a variety of default policies on S3 Buckets to achieve compliant behavior!

## Bucket Policies

### Require Server Side Encryption

Catalyze requires all POST/PUT operations to S3 Buckets to specify server-side encryption.

This policy affects API and command-line interactions with S3 buckets.

You ***WILL*** receive `Access Denied` errors if you attempt a PUT/POST without a server-side-encryptoin flag or header.
