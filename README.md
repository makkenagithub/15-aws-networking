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



### VPC Peering:

VPC peering can be done between VPCs in different regions and they can communicate with each other with private IPs.
After creating perring connection, add the routes from subnets route tables in each vpc to destination cidr range of other vpc with peering ID.

<img width="535" height="328" alt="image" src="https://github.com/user-attachments/assets/2fe94586-ca23-4efa-9a17-f0f3da2e2f9e" />

<img width="386" height="251" alt="image" src="https://github.com/user-attachments/assets/b301d6fe-53c9-492c-b65c-8cc2804156c0" />

Only 2 VPCs can be peered at a time. 3rd VPC can't be peered. Thats the limitation.

We can use Transit gateway to connect multiple VPCs.



### Transit Gateway: 
We can give cidr range for transit gateway, but its optional , we are giving here in this demo. This demo is about VPC connections within the same region say Frankfurt.

Can be used to connect multiple VPCs. EC2 in one VPC can ping to EC2 in other VPCs

<img width="471" height="288" alt="image" src="https://github.com/user-attachments/assets/e7547ada-d686-438d-aecc-3928dd727bfe" />

<img width="571" height="207" alt="image" src="https://github.com/user-attachments/assets/3d6575e3-5fbc-4b24-92f5-c797dc0609a7" />

Then click on transit gateway and create it

<img width="557" height="191" alt="image" src="https://github.com/user-attachments/assets/a09da38d-44ee-4213-8157-1c85c589714f" />

Transit gateway attachment to 3 VPCs: Choose attachment type as VPC.

<img width="562" height="148" alt="image" src="https://github.com/user-attachments/assets/679d7757-8e6d-4975-ab4c-09df938471ef" />

<img width="548" height="172" alt="image" src="https://github.com/user-attachments/assets/9cc54121-43b8-4a07-aa14-b5c2569dcbef" />


Update Route tables with transit gateway attchemnet:

Each VPC has multiple sibnets. Update routes of each subnet route table in a VPC with the other two VPC CIDR and use transit gateway ID here while adding route. For example, 

<img width="539" height="230" alt="image" src="https://github.com/user-attachments/assets/f1e95c39-72c2-4c71-8909-de0a24801151" />

Then create an EC2 in each VPC and ping the EC2 in other VPCs. 

### Transit gateway peering - Cross region peering

Here VPCs in different regions can connect using tarnsit gateway peering connection. Here we need to transit gateways in two regions.

<img width="457" height="295" alt="image" src="https://github.com/user-attachments/assets/a8b37145-549e-495d-b3a8-8d1d3cba658c" />


Transit gateway attchemnt for peering:

In transit gateway attachment type choose peering connection instaed of VPC (as we did in previos case), then choose the region and give the transit gateway ID of other region

<img width="572" height="275" alt="image" src="https://github.com/user-attachments/assets/5bd27bd9-ea6d-4469-a12c-1f54c6d8536d" />

<img width="548" height="214" alt="image" src="https://github.com/user-attachments/assets/f919af53-bc1f-4196-843e-348f222498c3" />

<img width="404" height="170" alt="image" src="https://github.com/user-attachments/assets/478b5023-7fb5-4d0e-aded-477d26001c10" />

Then accept the transit gateway attchment request in other region. Now Both transit gateways are peered with transit gateway attchement.

Transit gateway attachment to VPC: Choose attachment type as VPC. We do in individual regions.

Frankfurt:
<img width="469" height="258" alt="image" src="https://github.com/user-attachments/assets/b0fa194d-c299-42d0-aad2-c245f843a327" />


Stockholm:
<img width="515" height="263" alt="image" src="https://github.com/user-attachments/assets/57efe2f0-a00b-4ae5-a255-e3e16d9d65dc" />


<img width="457" height="184" alt="image" src="https://github.com/user-attachments/assets/a82ec05a-5103-4ddd-940a-3e6d196265d9" />

<img width="457" height="207" alt="image" src="https://github.com/user-attachments/assets/6ad725c5-b305-4aec-8087-6415529d58e1" />

Update Route tables:














