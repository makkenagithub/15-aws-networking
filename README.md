# 15-aws-networking

VPC:

vpc - cidr blocks, subnets(cidr blocks), internet gateway, nat gateway(elastic IP), route tables and routes, route tabel association to subnets

application load balancer ALB:

alb - vpc id, subnet ids, security gorups needed

alb listener - alb id, port

alb listener rule - listerner arn, target group arn, host/context based routing, action as forword to target group

target group - vpc id, port(application port), health check

autoscaling group - target group arn, launch template id, desired, min and max instances.

<img width="538" height="208" alt="image" src="https://github.com/user-attachments/assets/d12e1a59-98af-43b6-84ed-1ada51a31b53" />


VPC Peering:

VPC peering can be done between VPCs in different regions and they can communicate with each other with private IPs.
After creating perring connection, add the routes from subnets route tables in each vpc to destination cidr range of other vpc with peering ID.

<img width="535" height="328" alt="image" src="https://github.com/user-attachments/assets/2fe94586-ca23-4efa-9a17-f0f3da2e2f9e" />

<img width="386" height="251" alt="image" src="https://github.com/user-attachments/assets/b301d6fe-53c9-492c-b65c-8cc2804156c0" />

Only 2 VPCs can be peered at a time. 3rd VPC can't be peered. Thats the limitation.








