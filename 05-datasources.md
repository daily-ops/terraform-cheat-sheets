An excerpt from [HashiCorp document](https://developer.hashicorp.com/terraform/language/data-sources). A data block requests that Terraform read from a given data source ("aws_ami") and export the result under the given local name ("example"). The name is used to refer to this resource from elsewhere in the same Terraform module, but has no significance outside of the scope of a module.

- Common way of datasource declaration

  ```
  data "aws_ami" "example" {
    most_recent = true
  
    owners = ["self"]
    tags = {
      Name   = "app-server"
      Tested = "true"
    }
  }
  ```

- Supports meta-argument, such as `provider`, `count`, `for_each`, `lifecycle` argument are supported.
- Datasources are executed during **plan** operation, actual API calls are made to retrieve data.
