
- Common declaration of a resource.

  ```
  resource "<TYPE>" "<NAME>" {
    provider = aws.region-a
    argument1 = abc
    argument2 = def
  }
  ```
  
  The `provider` is argument optional and can be used to explicitly associate resource to the specified provider.

- Removing resource from configuration without destroy

  ```
  removed {
    from = aws_instance.example
  
    lifecycle {
      destroy = false
    }
  }
  ```

- Removing resource from configuration with destroy-time provisioner.

  ```
  removed {
    from = aws_instance.example
  
    lifecycle {
      destroy = false
    }
  }
  ```

- The Terraform language defines the following meta-arguments, which can be used with any resource type to change the behavior of resources:

  - depends_on, for specifying hidden dependencies
  - count, for creating multiple resource instances according to a count
  - for_each, to create multiple instances according to a map, or set of strings
  - provider, for selecting a non-default provider configuration
  - lifecycle, for lifecycle customizations
  - provisioner, for taking extra actions after resource creation
 
- Pre-condition check
- 
  ```
  resource "aws_instance" "example" {
    instance_type = "t2.micro"
    ami           = "ami-abc123"
  
    lifecycle {
      # The AMI ID must refer to an AMI that contains an operating system
      # for the `x86_64` architecture.
      precondition {
        condition     = data.aws_ami.example.architecture == "x86_64"
        error_message = "The selected AMI must be for the x86_64 architecture."
      }
    }
  }
  ```
- Post-condition check
  SAME AS pre-condition

- Timeout setting

  ```
  resource "aws_db_instance" "example" {
    # ...
  
    timeouts {
      create = "60m"
      delete = "2h"
      update = "30m"
    }
  }
  ```
