- The `check` block allows operations to be executed out of normal lifecycle.
- Check blocks allow you to define custom conditions that execute on every Terraform plan or apply operation without affecting the overall status of an operation.
- Check blocks execute as the last step of a plan or apply after Terraform has planned or provisioned your infrastructure

```
check "health_check" {
  data "http" "terraform_io" {
    url = "https://www.terraform.io"
  }

  assert {
    condition = data.http.terraform_io.status_code == 200
    error_message = "${data.http.terraform_io.url} returned an unhealthy status code"
  }
}
```

The failure from the check block will not block terraform operations. 
We recommend using check blocks to validate the status of infrastructure as a whole. 
We only recommend using postconditions when you want a guarantee on a single resource based on that resource's configuration.
