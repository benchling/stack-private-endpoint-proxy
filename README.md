# stack-private-endpoint-proxy
Cloudformation stack to create an NLB-based private endpoint proxy

This stack works by creating the following:
- NGINX instance, which proxies TCP traffic to the specified `DestinationIP:DestinationPort`
- AWS Network Load Balancer, which sits in front of the NGINX instance
- VPC Endpoint Service, hooked up to the NLB

Once created, we can create a VPC Endpoint on another VPC (including cross-account VPCs)
using a VPC Endpoint Service.

The other VPC can talk to the VPC Endpoint, which talks to the NLB, which talks to NGINX,
which talks to the `DestinationIP:DestinationPort`.

The destination can be any device that the NGINX instance can reach, including an
on-premises network connected to the NGINX instance VPC over VPN. Thus, this can
enable traffic from an external VPC to reach an on-premises network.

## How to Use
Only us-east-1 is supported at this time.

Download the YAML template:

[https://raw.githubusercontent.com/benchling/stack-private-endpoint-proxy/master/template.yaml](https://raw.githubusercontent.com/benchling/stack-private-endpoint-proxy/master/template.yaml)

Create an AWS CloudFormation stack:

[https://console.aws.amazon.com/cloudformation/home#/stacks/new](https://console.aws.amazon.com/cloudformation/home#/stacks/new)

After creating, in "Resources" note the `endpointService` Physical ID - this is the endpoint
for an Endpoint Service to connect to.
