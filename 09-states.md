Ways to manipulate the state of Terraform resources.

- Move the location
  
  ```
  terraform state mv <SOURCE> <DESTINATION>
  ```
  
  Don't forget the index or key when resources created with `count` or `for_each`

- The `moved` block.
  
  ```
  moved {
    from = <SOURCE ADDRESS>
    to = <DESTINATION ADDRESS>
  }
  ```

  The `moved` block supports implicit `count`, or `for_each` without need to specifiy the index or key.

  ```
  $terraform state list
  null_resource.test["A"]
  null_resource.test["B"]
  null_resource.test["C"]
  
  $cat main.tf                  
  resource "null_resource" "prod" {
    for_each = toset(["A","B","C"])
    provisioner "local-exec" {
      command = "echo ${each.key}"
    } 
  }
  
  moved {
    from = null_resource.test
    to = null_resource.prod
  }
  
  $terraform plan
  null_resource.prod["A"]: Refreshing state... [id=7184470638888354308]
  null_resource.prod["C"]: Refreshing state... [id=547213964731374889]
  null_resource.prod["B"]: Refreshing state... [id=8605255447287888062]
  
  Terraform will perform the following actions:
  
    # null_resource.test["A"] has moved to null_resource.prod["A"]
      resource "null_resource" "prod" {
          id = "7184470638888354308"
      }
  
    # null_resource.test["B"] has moved to null_resource.prod["B"]
      resource "null_resource" "prod" {
          id = "8605255447287888062"
      }
  
    # null_resource.test["C"] has moved to null_resource.prod["C"]
      resource "null_resource" "prod" {
          id = "547213964731374889"
      }
  
  Plan: 0 to add, 0 to change, 0 to destroy.
  ```
  
- Remove the resource from state.
  
  ```
  terraform state rm <RESOURE ADDRESS>
  ```

- Move resources created by old provider to new provider

  ```
  terraform state replace-provider hashicorp/aws registry.acme.corp/acme/aws
  ```
