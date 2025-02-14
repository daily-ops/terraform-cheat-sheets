## The `init` command.

  - Initialising terraform working directory with modules and providers

    ```
    terraform init
    ```

  - Upgrade existing modules and providers

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
