# serverless-cloudfront-distribution-certificate

This serverless plugin manages to create certificate for specified CloudFront distribution. It also handles validation trough dns and ROUTE 53.

[![serverless](http://public.serverless.com/badges/v3.svg)](http://www.serverless.com)
[![npm version](https://badge.fury.io/js/serverless-cloudfront-distribution-certificate.svg)](https://badge.fury.io/js/serverless-cloudfront-distribution-certificate)
[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/pfulop/serverless-cloudfront-distribution-certificate/master/LICENSE)

## Usage

### Installation

```bash
npm install serverless-cloudfront-distribution-certificate --save-dev
```

### Configuration

```yaml
plugins:
  - serverless-cloudfront-distribution-certificate

provider:
  name: aws
  # Region must be the global region of CloudFront for your AWS partition
  region: us-east-1

custom:
  cfdDomain:
    domainNames:
      - "serverless.example.com"
      - "server.example.com"
      - "doggo.example.com"
    cloudFront: CloudFrontDistribution
    retries: 15
    minimumProtocolVersion: TLSv1.2_2018
    enabled: true
```

Where `domainNames` are domains for which ssl certificate should be generated, `cloudFront` is the logical name of your CloudFront distribution, and `minimumProtocolVersion` is the ViewerCertificate's [MinimumProtocolVersion](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ViewerCertificate.html#cloudfront-Type-ViewerCertificate-MinimumProtocolVersion) setting (optional).
You can specify `enabled` (by default true) to add custom rules for when to use this plugin.

## Note

_To use an ACM Certificate with CloudFront, your serverless configuration must be configured to the global region that contains the CloudFront distributions. For standard AWS, it is the US East (N. Virginia) (`us-east-1`) region. For AWS China (`aws-cn` partition), it is the China (Ningxia) (`cn-northwest-1`) region. ACM Certificates in the correct global region that are associated with a CloudFront distribution are distributed to all the geographic locations configured for that distribution._

_This plugin will wait up to 15 minutes for certificate to be issued. If the state won't be issued within 15 minutes, it will fail._

_Additionaly you can specify number of retries by providing retries option. This number is used when checking if certificate is issued (1 retry == 1 minute), or when waiting for route record to be created (1 retry == 2 seconds)._

_If an AWS Profile is provided to the serverless command, it has been observed that the `--aws-profile` CLI option does not work with this plugin.  Instead, set the profile as an environment variable (e.g. `AWS_PROFILE=profile-name sls deploy ...`) or set it in the serverless configuration file (`provider.profile`)_
