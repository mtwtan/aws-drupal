# CloudFormation Template for Drupal CMS
## Introduction
This is a CloudFormation template that creates a fault-tolerant, highly-available Drupal stack. This stack will create:
- New VPC in a region in 2 to 3 Available Zones (AZ), each with its own subnets for DMZ, Web, and data layer
- Appropriate security groups in each subnet and all the necessary internet gateways and NAT gateways
- An Elastic File Share across the relevant AZs
- An Aurora MySQL cluster that span across the AZs with 1 write instance and another read replica
- A Memcache elastic cache
- A Bastion host for SSH connectivity to instances and RDS
- An Autoscaling group with Launch configuration
- Pre-written scripts that will run when instances are launched
- Elastic load balance on an ALB with target groups
- Creates a CloudFront CDN cache
- Create the appropriate DNS records from a zone

## Caveats
- Assumes that user will create an appropriate S3 bucket to store the CloudFormation template
- Sets the following "folders" in the S3 bucket:
  - drupal
  - templates
  - bits
- Downloads the Drupal Tar file and upload it to the S3 bucket in "folder" drupal
- Uploads the AmazonElasticCacheClusterClient-<version>-PHP<version>.tar.gz into the "bits" folder in S3
- Uploads the opcache-instance-id.php into the bits folder in S3
- Assumes the user will modify the template, replacing the <bucket-name> with the appropriate S3 bucket
- Assumes that the user running the CloudFormation has all the appropriate permissions to launch CloudFormation and the IAM policy to create IAM roles
- Assumes that the AWS account already has a working DNS zone in Route53
- Assumes that appropriate TLS certs are installed Certificate Manager and they can be used by the Drupal stack.