# stack-private-endpoint-proxy
Cloudformation stack to create an NLB-based private endpoint proxy

This stack works by creating the following:
- NGINX instance, which proxies TCP traffic to the specified `DestinationIP:DestinationPort`
- AWS Network Load Balancer, which sits in front of the NGINX instance
- VPC Endpoint Service, hooked up to the NLB

Once created, an external AWS account (e.g. Benchling) can create a VPC Endpoint on
an external VPC using the VPC Endpoint Service.

The external VPC can talk to the VPC Endpoint, which talks to the NLB, which talks to
NGINX, which talks to the `DestinationIP:DestinationPort`.

The destination can be any device that the NGINX instance can reach, including an
on-premises network connected to the NGINX instance VPC over VPN. Thus, this can
enable traffic from an external VPC to reach an on-premises network.