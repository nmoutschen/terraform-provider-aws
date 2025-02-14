---
subcategory: "VPC (Virtual Private Cloud)"
layout: "aws"
page_title: "AWS: aws_vpc_endpoint"
description: |-
  Provides a VPC Endpoint resource.
---


<!-- Please do not edit this file, it is generated. -->
# Resource: aws_vpc_endpoint

Provides a VPC Endpoint resource.

~> **NOTE on VPC Endpoints and VPC Endpoint Associations:** Terraform provides both standalone VPC Endpoint Associations for
[Route Tables](vpc_endpoint_route_table_association.html) - (an association between a VPC endpoint and a single `routeTableId`),
[Security Groups](vpc_endpoint_security_group_association.html) - (an association between a VPC endpoint and a single `securityGroupId`),
and [Subnets](vpc_endpoint_subnet_association.html) - (an association between a VPC endpoint and a single `subnetId`) and
a VPC Endpoint resource with `routeTableIds` and `subnetIds` attributes.
Do not use the same resource ID in both a VPC Endpoint resource and a VPC Endpoint Association resource.
Doing so will cause a conflict of associations and will overwrite the association.

## Example Usage

### Basic

```typescript
// Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { VpcEndpoint } from "./.gen/providers/aws/vpc-endpoint";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    new VpcEndpoint(this, "s3", {
      serviceName: "com.amazonaws.us-west-2.s3",
      vpcId: main.id,
    });
  }
}

```

### Basic w/ Tags

```typescript
// Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { VpcEndpoint } from "./.gen/providers/aws/vpc-endpoint";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    new VpcEndpoint(this, "s3", {
      serviceName: "com.amazonaws.us-west-2.s3",
      tags: {
        Environment: "test",
      },
      vpcId: main.id,
    });
  }
}

```

### Interface Endpoint Type

```typescript
// Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { VpcEndpoint } from "./.gen/providers/aws/vpc-endpoint";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    new VpcEndpoint(this, "ec2", {
      privateDnsEnabled: true,
      securityGroupIds: [sg1.id],
      serviceName: "com.amazonaws.us-west-2.ec2",
      vpcEndpointType: "Interface",
      vpcId: main.id,
    });
  }
}

```

### Gateway Load Balancer Endpoint Type

```typescript
// Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { Token, TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { DataAwsCallerIdentity } from "./.gen/providers/aws/data-aws-caller-identity";
import { VpcEndpoint } from "./.gen/providers/aws/vpc-endpoint";
import { VpcEndpointService } from "./.gen/providers/aws/vpc-endpoint-service";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    const current = new DataAwsCallerIdentity(this, "current", {});
    const example = new VpcEndpointService(this, "example", {
      acceptanceRequired: false,
      allowedPrincipals: [Token.asString(current.arn)],
      gatewayLoadBalancerArns: [Token.asString(awsLbExample.arn)],
    });
    const awsVpcEndpointExample = new VpcEndpoint(this, "example_2", {
      serviceName: example.serviceName,
      subnetIds: [Token.asString(awsSubnetExample.id)],
      vpcEndpointType: example.serviceType,
      vpcId: Token.asString(awsVpcExample.id),
    });
    /*This allows the Terraform resource name to match the original name. You can remove the call if you don't need them to match.*/
    awsVpcEndpointExample.overrideLogicalId("example");
  }
}

```

### Non-AWS Service

```typescript
// Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { Token, propertyAccess, TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { DataAwsRoute53Zone } from "./.gen/providers/aws/data-aws-route53-zone";
import { Route53Record } from "./.gen/providers/aws/route53-record";
import { VpcEndpoint } from "./.gen/providers/aws/vpc-endpoint";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    const ptfeService = new VpcEndpoint(this, "ptfe_service", {
      privateDnsEnabled: false,
      securityGroupIds: [Token.asString(awsSecurityGroupPtfeService.id)],
      serviceName: Token.asString(varPtfeService.value),
      subnetIds: [subnetIds],
      vpcEndpointType: "Interface",
      vpcId: vpcId.stringValue,
    });
    const internal = new DataAwsRoute53Zone(this, "internal", {
      name: "vpc.internal.",
      privateZone: true,
      vpcId: vpcId.stringValue,
    });
    const awsRoute53RecordPtfeService = new Route53Record(
      this,
      "ptfe_service_2",
      {
        name: "ptfe.${" + internal.name + "}",
        records: [
          Token.asString(
            propertyAccess(ptfeService.dnsEntry, ["0", '"dns_name"'])
          ),
        ],
        ttl: Token.asNumber("300"),
        type: "CNAME",
        zoneId: Token.asString(internal.zoneId),
      }
    );
    /*This allows the Terraform resource name to match the original name. You can remove the call if you don't need them to match.*/
    awsRoute53RecordPtfeService.overrideLogicalId("ptfe_service");
  }
}

```

~> **NOTE The `dnsEntry` output is a list of maps:** Terraform interpolation support for lists of maps requires the `lookup` and `[]` until full support of lists of maps is available

## Argument Reference

The following arguments are supported:

* `serviceName` - (Required) The service name. For AWS services the service name is usually in the form `comAmazonaws.<region>.<service>` (the SageMaker Notebook service is an exception to this rule, the service name is in the form `awsSagemaker.<region>Notebook`).
* `vpcId` - (Required) The ID of the VPC in which the endpoint will be used.
* `autoAccept` - (Optional) Accept the VPC endpoint (the VPC endpoint and service need to be in the same AWS account).
* `policy` - (Optional) A policy to attach to the endpoint that controls access to the service. This is a JSON formatted string. Defaults to full access. All `gateway` and some `interface` endpoints support policies - see the [relevant AWS documentation](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) for more details. For more information about building AWS IAM policy documents with Terraform, see the [AWS IAM Policy Document Guide](https://learn.hashicorp.com/terraform/aws/iam-policy).
* `privateDnsEnabled` - (Optional; AWS services and AWS Marketplace partner services only) Whether or not to associate a private hosted zone with the specified VPC. Applicable for endpoints of type `interface`.
Defaults to `false`.
* `dnsOptions` - (Optional) The DNS options for the endpoint. See dns_options below.
* `ipAddressType` - (Optional) The IP address type for the endpoint. Valid values are `ipv4`, `dualstack`, and `ipv6`.
* `routeTableIds` - (Optional) One or more route table IDs. Applicable for endpoints of type `gateway`.
* `subnetIds` - (Optional) The ID of one or more subnets in which to create a network interface for the endpoint. Applicable for endpoints of type `gatewayLoadBalancer` and `interface`.
* `securityGroupIds` - (Optional) The ID of one or more security groups to associate with the network interface. Applicable for endpoints of type `interface`.
If no security groups are specified, the VPC's [default security group](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html#DefaultSecurityGroup) is associated with the endpoint.
* `tags` - (Optional) A map of tags to assign to the resource. If configured with a provider [`defaultTags` configuration block](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#default_tags-configuration-block) present, tags with matching keys will overwrite those defined at the provider-level.
* `vpcEndpointType` - (Optional) The VPC endpoint type, `gateway`, `gatewayLoadBalancer`, or `interface`. Defaults to `gateway`.

### dns_options

* `dnsRecordIpType` - (Optional) The DNS records created for the endpoint. Valid values are `ipv4`, `dualstack`, `serviceDefined`, and `ipv6`.
* `privateDnsOnlyForInboundResolverEndpoint` - (Optional) Indicates whether to enable private DNS only for inbound endpoints. This option is available only for services that support both gateway and interface endpoints. It routes traffic that originates from the VPC to the gateway endpoint and traffic that originates from on-premises to the interface endpoint. Can only be specified if `privateDnsEnabled` is `true`.

## Timeouts

[Configuration options](https://developer.hashicorp.com/terraform/language/resources/syntax#operation-timeouts):

- `create` - (Default `10M`)
- `update` - (Default `10M`)
- `delete` - (Default `10M`)

## Attributes Reference

In addition to all arguments above, the following attributes are exported:

* `id` - The ID of the VPC endpoint.
* `arn` - The Amazon Resource Name (ARN) of the VPC endpoint.
* `cidrBlocks` - The list of CIDR blocks for the exposed AWS service. Applicable for endpoints of type `gateway`.
* `dnsEntry` - The DNS entries for the VPC Endpoint. Applicable for endpoints of type `interface`. DNS blocks are documented below.
* `networkInterfaceIds` - One or more network interfaces for the VPC Endpoint. Applicable for endpoints of type `interface`.
* `ownerId` - The ID of the AWS account that owns the VPC endpoint.
* `prefixListId` - The prefix list ID of the exposed AWS service. Applicable for endpoints of type `gateway`.
* `requesterManaged` -  Whether or not the VPC Endpoint is being managed by its service - `true` or `false`.
* `state` - The state of the VPC endpoint.
* `tagsAll` - A map of tags assigned to the resource, including those inherited from the provider [`defaultTags` configuration block](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#default_tags-configuration-block).

DNS blocks (for `dnsEntry`) support the following attributes:

* `dnsName` - The DNS name.
* `hostedZoneId` - The ID of the private hosted zone.

## Import

VPC Endpoints can be imported using the `vpc endpoint id`, e.g.,

```
$ terraform import aws_vpc_endpoint.endpoint1 vpce-3ecf2a57
```

<!-- cache-key: cdktf-0.17.1 input-881682753c7f4bba53d750ab515083f05a7ebaa77a8ca5024446a629a9b83d3e -->