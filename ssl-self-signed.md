---
title: Catalyze Resources - Stratum Self-Signed SSL Certs
category: ssl
summary: Learn about self-signed SSL certificates on Stratum.
---

# Self-Signed vs CA-Signed SSL Certs

If you are doing testing or do not yet have a domain name secured, you can use a self-signed SSL certificate to setup
your environment when not in production. Internet browsers will throw up a warning for any site with a self-signed
certificate

A self-signed cert can be generated on your local computer with the Openssl library.

# Generate Your Certificate

First, generate the private key for the self-signed certificate.

`openssl genrsa -out key.pem 2048`

Second, generate the certificate signing request for the certificate.

`openssl req -new -key key.pem -out csr.pem`

Finally, generate the certificate and answer the questions.

**Please note: Do not set a challenge password.**

`openssl req -x509 -days 365 -key key.pem -in csr.pem -out certificate.pem`

**Takeaway: The key.pem and certificate.pem files are now suitable for use in the CLI to setup certs and sites. Follow the guide [here](https://resources.catalyze.io/stratum/articles/guides/self-service-SSL) and don't forget to use the `-s` flag while uploading the cert!**

# Production SSL Certificates

Self-signed SSL certificates are not acceptable for production use. Use a reputable certificate authority for your domain.

If you are not familiar with certificate authorities, we recommend [Digicert](https://www.digicert.com/).
