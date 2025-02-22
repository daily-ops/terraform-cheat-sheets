* The default file name of Terraform configuration file are `terraform.rc` and `.terraformrc` which located at `%APPDATA%` and `HOME` directory of Windows and Linux respectively. Note that `%APPDATA%` is equivalent to `$env:APPDATA` in Powershell.
* The location can be overriden by `TF_CLI_CONFIG_FILE` environment variable.
* It supports these followings:

  |Name|Description|
  |-|-|
  |credentials|configures credentials for use with HCP Terraform or Terraform Enterprise. The credentials require matching domain name and `token`.|
  |credentials_helper|configures an external helper program for the storage and retrieval of credentials for HCP Terraform or Terraform Enterprise.|
  |disable_checkpoint|when set to true, disables upgrade and security bulletin checks that require reaching out to HashiCorp-provided network services.|
  |disable_checkpoint_signature|when set to true, allows the upgrade and security bulletin checks described above but disables the use of an anonymous id used to de-duplicate warning messages.|
  |plugin_cache_dir|enables plugin caching and specifies, as a string, the location of the plugin cache directory.|
  |provider_installation|customizes the installation methods used by terraform init when installing provider plugins.|

* credentials

  ```
  credentials "app.terraform.io" {
    token = "xxxxxx.atlasv1.zzzzzzzzzzzzz"
  }
  ```

* credentials_helper

  A credentials helper called `credstore`, for example, would be implemented as an executable program named `terraform-credentials-credstore` (with an .exe extension on Windows only), and installed in one of the default plugin search locations (provider_installation).
  
  ```
  credentials_helper "credstore" {
    args = ["--host=credstore.example.com"]
  }
  ```

* plugin_cache_dir property, alternatively using `TF_PLUGIN_CACHE_DIR` environment variable, to set the directory where providers if the chosen plugin is already available in the cache directory. If so, Terraform will use the previously-downloaded copy. Using cache doesn't mean that it goes offline, there will still be a check against metadata.

  ```
  plugin_cache_dir = "$HOME/.terraform.d/plugin-cache"
  ```

* provider_installation, there are two options to mirror, `filesystem_mirror` and `network_mirror`, and one `direct` option.

  ```
  provider_installation {
    filesystem_mirror {
      path    = "/usr/share/terraform/providers"
      include = ["example.com/*/*"]
    }
    direct {
      exclude = ["example.com/*/*"]
    }
    network_mirror {
      url = "https://terraform.example.com/providers/"
    }
  }
  ```

* The `dev_overrides` block. Terraform supports a special additional block dev_overrides in provider_installation blocks. The contents of this block effectively override all of the other configured installation methods, so a block of this type must always appear first in the sequence

  ```
  provider_installation {
  
    # Use /home/developer/tmp/terraform-null as an overridden package directory
    # for the hashicorp/null provider. This disables the version and checksum
    # verifications for this provider and forces Terraform to look for the
    # null provider plugin in the given directory.
    dev_overrides {
      "hashicorp/null" = "/home/developer/tmp/terraform-null"
    }
  
    # For all other providers, install them directly from their origin provider
    # registries as normal. If you omit this, Terraform will _only_ use
    # the dev_overrides block, and so no other providers will be available.
    direct {}
  }
  ```
