* Set log level through `TF_LOG` environment variable with the following supported levels, TRACE, DEBUG, INFO, WARN or ERROR. It sometimes assists in troubleshooting such as incompatiblities in API parameters across versions of providers caused by a bug or changes from the API providers.
  
  ```
  2025-02-22T09:48:17.486+0700 [DEBUG] provider.terraform-provider-aws_v5.87.0_x5: HTTP Response Received: @caller=github.com/hashicorp/aws-sdk-go-base/v2@v2.0.0-beta.62/logging/tf_logger.go:47 aws.region=ap-southeast-2 http.duration=508 http.response.header.content_type=text/xml http.response.header.x_amzn_requestid=efabeb12-b805-4101-8eb8-6ad41c97dd7a tf_mux_provider=*schema.GRPCProviderServer tf_provider_addr=registry.terraform.io/hashicorp/aws @module=aws.aws-base http.response.header.date="Sat, 22 Feb 2025 02:48:17 GMT" http.status_code=200 rpc.method=GetCallerIdentity rpc.service=STS tf_aws.sdk=aws-sdk-go-v2 tf_rpc=ConfigureProvider http.response.body="<GetCallerIdentityResponse xmlns="https://sts.amazonaws.com/doc/2011-06-15/">
  <GetCallerIdentityResult>
    <Arn>arn:aws:iam::XXXXXXXX:user/terraform</Arn>
    <UserId>AIDA*************DN27</UserId>
    <Account>XXXXXXXX</Account>
  </GetCallerIdentityResult>
  <ResponseMetadata>
    <RequestId>efabeb12-b805-4101-8eb8-6ad41c97dd7a</RequestId>
  </ResponseMetadata>
  </GetCallerIdentityResponse>
  " http.response_content_length=406 rpc.system=aws-api tf_aws.signing_region= tf_req_id=f2640ebf-0003-3647-eb63-132910671d27 timestamp=2025-02-22T09:48:17.485+0700
  2025-02-22T09:48:17.487+0700 [INFO]  provider.terraform-provider-aws_v5.87.0_x5: Retrieved caller identity from STS: tf_mux_provider=*schema.GRPCProviderServer tf_rpc=ConfigureProvider @caller=github.com/hashicorp/aws-sdk-go-base/v2@v2.0.0-beta.62/logging/tf_logger.go:39 @module=aws.aws-base tf_provider_addr=registry.terraform.io/hashicorp/aws tf_req_id=f2640ebf-0003-3647-eb63-132910671d27 timestamp=2025-02-22T09:48:17.486+0700
  ```

* Separation of logging levels between core and provider can be achieved through two separate variables, `TF_LOG_CORE` and `TF_LOG_PROVIDER`.
* The location of log output file can be configured through `TF_LOG_PATH`.
