### Brief summary

Terraform makes use of `providers` as plugins components in which contains the implementation of the logics and codes that perform resource provisioning, updatings, destroying.

- The requirement block

  ```
  terraform {
    required_providers {
      aws = {
        source  = "registry.terraform.io/hashicorp/aws"
        version = "~> 5.30"
      }
    }
  }
  ```

  - The `source` can be in the form of `<HOSTNAME>/<NAMESPACE>/<TYPE>` or `<NAMESPACE>/<TYPE>` where the default `<HOSTNAME>` is `registry.terraform.io`.

  - The version constraints in summary

    |Operator|	Description|
    |-|-|
    |=,no operator|	Allows only one exact version number. Cannot be combined with other conditions.|
    |!=|	Excludes an exact version number.|
    |>,>=,<,<=|	Compares to a specified version. Terraform allows versions that resolve to true. The > and >= operators request newer versions. The < and <= operators request older versions.|
    |~>|Allows only the right-most version component to increment.<br> Examples: <br>  `~> 1.0.4`: Allows Terraform to install `1.0.5` and `1.0.10` but not `1.1.0.`<br> `1.1`: Allows Terraform to install 1.2 and 1.10 but not 2.0|

- The configuration block

  ```
  provider "aws" {
    # ...
  }
  ```

- With multiple configuration blocks of the same type, aliases are required. The provider block without alias becomes default provider of the type.

  ```
  provider "aws" {
    region = "ap-southeast-2"
  }
  
  provider "aws" {
    alias = "xxx"
    region = "ap-southeast-1"
  }
  ```

- Resources or Datasources of the provider type can choose to associate with specific provider block through `provider` argument and set with the `<TYPE>.<ALIAS NAME>` otherwise implicit default provider will be assumed.

  ```
  resource "aws_instance" "web" {
    provider = aws.xxx
  }
  ```

  ```
  data "aws_caller_identity" "current" {
    provider = aws.xxx
  }
  ```
  
