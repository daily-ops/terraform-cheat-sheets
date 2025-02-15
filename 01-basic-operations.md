## The `init` command.

  - Initialising terraform working directory with modules and providers

    ```
    terraform init
    ```

  - Upgrade/Downgrade existing modules and providers

    ```
    terraform init -upgrade
    ```

  - Migrate state to new backend

    ```
    terrafrom init -migrate-state
    ```

## The `plan` command.
  
  - Process terraform configuration files and generate plan output

    ```
    terraform plan
    ```

  - Supply variables to the plan operation

    ```
    terraform plan -var "name=value"
    ```

  - Render output in json format which can be useful for external tools.
 
    ```
    terraform plan -json
    ```

  - Suppress ASCII color code from the plan output which makes the plan output readable in automation tools
 
    ```
    terraform plan -no-color
    ```

  - Enforce the operation to return detailed exit code for automated process
 
    ```
    terraform plan -detailed-exitcode
    ```

  - Generate plan output file which can be used by the `apply` operation
 
    ```
    terraform plan -out=<OUTPUT FILENAME>
    ```

  - Generate plan for the `destroy` operation via `apply`, combine with the `-out` in the sample below
    
    ```
    terraform plan -destroy -out=<OUTPUT FILENAME>
    ```

  - Refresh and update terraform state from the updates of the remote objects
 
    ```
    terraform plan -refresh-only
    ```

  - Adjust concurrent processing in plan operation, by default 10 processes can be run concurrently. 
  
    ```
    terraform plan -parallelism=5
    ```

## The `apply` command.
  
  - Process terraform configuration files and generate apply output

    ```
    terraform apply
    ```

  - Using generated plan file, it also works with the destroy plan.

    ```
    terraform apply xxxxxxx.tfplan
    ```

  - Supply variables to the apply operation

    ```
    terraform apply -var "name=value"
    ```

  - Automatically approve the apply operaition, usually terraform will ask to confirm the apply operation with a prompt to say yes or no

    ```
    terraform apply -auto-approve
    ```

  - Destroying resources

    ```
    terraform apply -destroy
    ```

  - Suppress ASCII color code from the plan output which makes the plan output readable in automation tools
 
    ```
    terraform apply -no-color
    ```
    
  - Adjust concurrent processing in plan operation, by default 10 processes can be run concurrently. For example if there are 20 resources in the terrafomr configuration files without any dependency between each other, 10 of them will be processed concurrently. It may overwhelm the remote server by having too many intensive resource provisioning requests, or hitting API rate limit, or causing too many operations on client itself.
  
    ```
    terraform apply -parallelism=5
    ```
    
