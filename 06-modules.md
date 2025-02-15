Modules are reusable terraform codes. They can take variables to adjust the behaviours dynamically and return outputs.

- Syntax

  ```
  module "<NAME>" {
    source = "<SOURCE>"
    version = "<VERSION>"
    providers = {
      aws = aws.usw2
    }
    ...
    ...
    ...
  }
  ```

  - `NAME`: name of module which can be use as identifier/reference.
  - `SOURCE`: path to the module which can be in various type
    - local path with `./` or `../` prefix.
    - registry which is defined in the following format `<NAMESPACE>/<NAME>/<PROVIDER>`.
    - github, `github.com/hashicorp/example`, `git::https://github.com/dummy/vpc.git`,`git::ssh://username@github.com/dummy/storage.git?depth=1&ref=v1.2.0`
    - bitbucket, `bitbucket.org/hashicorp/terraform-consul-aws`
    - generic git, same as samples in `git::https://example.com/vpc.git`,`git::ssh://username@example.com/storage.git?depth=1&ref=v1.2.0`
    - mercurial, `hg::http://example.com/vpc.hg?ref=v1.2.0`
    - http, `https://example.com/vpc-module.zip`, zip, tar.bz2,tbz2,tar.gz,tgz,tar.xz,txz are supported.
    - s3, `s3::https://s3-eu-west-1.amazonaws.com/examplecorp-terraform-modules/vpc.zip` where credentials derives from environment variables, or profile, or instance profile respectively.
    - gcs, `gcs::https://www.googleapis.com/storage/v1/modules/foomodule.zip` where credentials derives from oauth token environment variables, service account key file, instance profile, `gcloud auth application-default login`.
  - `version`: version of module which is supported by modules located in a registry and supports version constraints:
      |Operator|	Description|
      |-|-|
      |=,no operator|	Allows only one exact version number. Cannot be combined with other conditions.|
      |!=|	Excludes an exact version number.|
      |>,>=,<,<=|	Compares to a specified version. Terraform allows versions that resolve to true. The > and >= operators request newer versions. The < and <= operators request older versions.|
      |~>|Allows only the right-most version component to increment.<br> Examples: <br>  `~> 1.0.4`: Allows Terraform to install `1.0.5` and `1.0.10` but not `1.1.0.`<br> `1.1`: Allows Terraform to install 1.2 and 1.10 but not 2.0|
  - `...`: other input variables.
- Module itself may be located in a sub-directory relative to the root of the package. A special **double-slash** syntax is interpreted by Terraform to indicate that the remaining path after that point is a sub-directory within the package
- The default inheriting providers can be overriden by using `providers` meta-argument with an assigned alias. 
- Additional meta-arguments such as `depends_on`, `count`, `for_each` are supported.
- Public module registry `registry.terraform.io`. Private ones can be setup.
