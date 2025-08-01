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






