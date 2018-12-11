Terraform AWS module for EKS master node bootstrap
==================================================

Generic repository for a terraform module for AWS EKS control plane (managed master nodes) bootstrap

![Image of Terraform](https://i.imgur.com/Jj2T26b.jpg)

# Table of content

- [Introduction](#intro)
- [Usage](#usage)
- [Release log](#release-log)
- [Module versioning & git](#module-versioning-&-git)
- [Local terraform setup](#local-terraform-setup)
- [Authors/Contributors](#authorscontributors)


# Intro

Module to bootstrap control plan of a AWS EKS cluster with the following details:
- AWS

# Usage

Example usage:

```hcl

module "eks_masters" {
  source                      = "github.com/diogoaurelio/terraform-module-aws-compute-eks-cluster-control-plane"
  version                     = "v0.0.1"

  vpc_id                      = "vpc-123"
  aws_region                  = "eu-west-1"
  cluster_name                = "eks"
  environment                 = "dev"
  master_subnet_ids           = ["subnet-12345678"]
  master_ingress_cidr         = ["10.10.0.1/24"]
  project                     = "analytics"
  account_id                  = "${data.aws_caller_identity.current.account_id}"
}

module "dev_s3_encrypted" {
  source                            = "github.com/diogoaurelio/terraform-module-aws-storage-s3-logging-encrypted"
  version                           = "v0.0.1"

  environment                       = "dev"
  project                           = "analytics"
  region                            = "eu-west-1"

  s3_bucket_name                    = "mybucket"
  s3_bucket_acl                     = "private"
  kms_key_alias                     = "${var.science_dev_env}-${var.project}-mybucket-key"
  versioning_enabled                = true
  transition_lifecycle_rule_enabled = false
  expiration_lifecycle_rule_enabled = false
}

```


# Release log

Whenever you bump this module's version, please add a summary description of the changes performed, so that collaboration across developers becomes easier.

* version v0.0.1 - first module release

# Module versioning & git

To update this module please follow the following proceedure:

1) make your changes following the normal git workflow
2) after merging the your changes to master, comes the most important part, namely versioning using tags:

```bash
git tag v0.0.2
```

3) push the tag to the remote git repository:
```bash
git push origin master tag v0.0.2
```

# Local terraform setup

* [Install Terraform](https://www.terraform.io/)

```bash
brew install terraform
```

* In order to automatic format terraform code (and have it cleaner), we use pre-commit hook. To [install pre-commit](https://pre-commit.com/#install).
* Run [pre-commit install](https://pre-commit.com/#usage) to setup locally hook for terraform code cleanup.

```bash
pre-commit install
```


# Authors/Contributors

See the list of [contributors](https://github.com/diogoaurelio/terraform-module-aws-storage-s3-logging-encrypted/graphs/contributors) who participated in this project.