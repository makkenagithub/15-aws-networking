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


Update all Route tables with transit gateway id:

Each VPC has multiple sibnets. Update routes of each subnet route table in a VPC with the other two VPC CIDR and use transit gateway ID here while adding route. For example, 

<img width="539" height="230" alt="image" src="https://github.com/user-attachments/assets/f1e95c39-72c2-4c71-8909-de0a24801151" />

Then create an EC2 in each VPC and ping the EC2 in other VPCs. 

### Transit gateway peering - Cross region peering

Here VPCs in different regions can connect using tarnsit gateway peering connection. Here we need to create transit gateways in two regions.

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


Update all Route tables: 

update all the route tables with the transit gateway ID of that region and give the CIDR of other region VPC

<img width="554" height="253" alt="image" src="https://github.com/user-attachments/assets/ff9c3464-59ed-4fa3-8116-de37bcd6ca02" />


<img width="533" height="200" alt="image" src="https://github.com/user-attachments/assets/e965eeef-7a2b-48da-b5d3-a329d74a4f9a" />


### VPC endpoint

VPC endpoint can be used to flow the traffic internally in AWS an not through internet. For eg, assume we have an S3 bucket in our AWS account. 
And We have EC2 in private subnets. Now if we want ec2 in private subnet to access S3, then ec2 needs to access S3 through internet (may be NAT gateway - I assume). So the flow is going through externally through internet. Even an EC2 in public subnet access the S3 with internet gateway and flow is external.

TO make the flow internally i.e.e not goting through internet, then we can use VPC endpoint. VPC end point notonly to access S3 internally, any other AWS respource which can be access thorigh internet, then we can make use of VPC end point so that the traffoc flow is private.


<img width="485" height="317" alt="image" src="https://github.com/user-attachments/assets/9ba5b515-630b-4566-8274-c80834649746" />


<img width="461" height="258" alt="image" src="https://github.com/user-attachments/assets/08a8f04c-a4a9-4e5d-982d-9de03c6b6d85" />


<img width="541" height="257" alt="image" src="https://github.com/user-attachments/assets/a4b65c8d-1f8f-4ca2-80ee-92b09fb0983c" />



type S3 and choose. Then choose gateway endpoint, VPC. Then choose the subnet route tables from which we need to access the endpoint. 

<img width="548" height="234" alt="image" src="https://github.com/user-attachments/assets/ed355d14-3401-42a0-bbf3-c4973512b3b1" />

<img width="553" height="278" alt="image" src="https://github.com/user-attachments/assets/054db97d-4512-4b4a-adeb-15dc3485a66f" />

<img width="470" height="271" alt="image" src="https://github.com/user-attachments/assets/235e0a58-25a2-4918-b88b-8e56525a8813" />

<img width="554" height="278" alt="image" src="https://github.com/user-attachments/assets/640dddfc-650c-4921-be01-beaba5e21241" />


Then login to EC2 and run some aws s3 commands

<img width="469" height="185" alt="image" src="https://github.com/user-attachments/assets/299044f8-291c-4bbe-b91a-5d034e88b154" />


### VPC flow logs
Create a VPC with public, private subnets, internet gateway, route tables and association to subnets. Then create an EC2 in public subnet

<img width="499" height="299" alt="image" src="https://github.com/user-attachments/assets/7e62f126-900c-4fcb-9df9-7f3395a40144" />


Now goto cloudwatch and create log group

<img width="536" height="215" alt="image" src="https://github.com/user-attachments/assets/abdedcee-d639-41e4-ba0b-b06e8e829506" />


<img width="418" height="125" alt="image" src="https://github.com/user-attachments/assets/8a474fd2-6622-4a8e-89b2-4665382299c9" />


Now VPC needs to send logs to cloudwatch log group. FOr that we need IAM roles/policy permissions. Here they created some custom role and attached a custom policy to the role. WHich is something like create / write logs to clougwatch log group.

<img width="475" height="295" alt="image" src="https://github.com/user-attachments/assets/6cf9cc24-29b5-4a8a-a42f-45e94c080028" />


<img width="409" height="176" alt="image" src="https://github.com/user-attachments/assets/f5d74838-6812-4708-b0fe-1c131483307a" />



VPC sends the logs to log gorup through NETWORK INTERFACE.

Now goto VPC and click on Flow logs -> create flow logs

<img width="551" height="261" alt="image" src="https://github.com/user-attachments/assets/b1bd0c5e-6d1a-4712-8b53-ef3e84648fe3" />

<img width="542" height="296" alt="image" src="https://github.com/user-attachments/assets/ab368cb6-f9a5-4630-9d7f-fa4a230cb223" />

<img width="549" height="269" alt="image" src="https://github.com/user-attachments/assets/1f52ca6c-7afb-48b1-b963-923611bf8df9" />

Now goto log group we created above and click on Log streams and see eni (elastic network interface), which is responsible to write logs to the log group. Then click on eni and see some logs

<img width="552" height="287" alt="image" src="https://github.com/user-attachments/assets/b39d78a3-5e9f-488c-9004-cc9aca62f0e8" />

<img width="554" height="254" alt="image" src="https://github.com/user-attachments/assets/39339c60-270a-45f4-809a-bb61c512a5f8" />


Now goto EC2 in the VPC and click on ec2 id -> Networking  and see the Network Interfaces

<img width="548" height="239" alt="image" src="https://github.com/user-attachments/assets/b699d2c4-d241-4409-894d-dfdc88cb83bf" />


### Network Loab balancer :

nlb works on layer 4. And alb works on layer 7.

<img width="530" height="302" alt="image" src="https://github.com/user-attachments/assets/43ba4930-f145-4e70-be3a-65c3af68140f" />


nlb vs alb:

1. nlb works on TCP/UDP while alb works on http/https.
2. nlb does not support path based routing , it suppprts only a url. alb supports path based routing.
3. nlb is useful when there is for high traffic with low latency. So nlb is used especially when we need low latency and handles high traffic.

<img width="512" height="217" alt="image" src="https://github.com/user-attachments/assets/5d6b6f78-5319-4b27-804b-cf9fc8bacfef" />


While setting up the target group for NLB, choose TCP as NLB works on TCP/UDP.

<img width="551" height="259" alt="image" src="https://github.com/user-attachments/assets/b24e6d0a-783d-48a6-b46a-7201b7ee3aaa" />

create NLB: choose nlb here

<img width="552" height="280" alt="image" src="https://github.com/user-attachments/assets/4b2de947-ac6b-4e02-8159-9745d7d47779" />


<img width="548" height="278" alt="image" src="https://github.com/user-attachments/assets/c67eb5c2-d1ed-493a-a0f4-03b63af0121f" />


### Gateway load balancer glb:
glb works on GENEVE protocol and port is 6081. In the target group we need to use GENEVE and 6081 port. It does not work with http, tcp, udp etc. glb works on layer 3


<img width="512" height="302" alt="image" src="https://github.com/user-attachments/assets/1bdc8585-a506-40a9-8808-410e1372fcee" />



glb is like to inspect/validate the incoming and outgoing requests. Here we are creating 2 VPCs. Course VPC and Security VPC.

In security VPC we have only private subnet and no internet gateway. But for demo purpose, we are using internet gateway in security VPC.

Also we create a target group in security VPC and glb will target to that target group. Here in the target group we need use GENEVE 6081 port. An ec2 will be placed to the target group. Choose this target group while creating glb


<img width="569" height="276" alt="image" src="https://github.com/user-attachments/assets/837b295e-3b33-41bc-8da7-7451e430259b" />


<img width="536" height="264" alt="image" src="https://github.com/user-attachments/assets/0fe89759-98f8-453a-8eb0-b272a866d314" />


Now both the VPCs and glb are created. But both are independent , we need to enable the traffic flow between them. For that we need to enable cross VPC communication.  we need to use VPC end point and VPC end point service.

VPC endpoint service is created for glb in security VPC. Similarly VPC end point is created in course VPC. So VPC end point point to VPC end point service. FLow is as follows

EC2 in cource VPC -> route table -> vpc end point -> vpc endpoint service -> glb -> applications ec2.

<img width="521" height="304" alt="image" src="https://github.com/user-attachments/assets/db823de2-78a5-4424-8b19-f2762332eba5" />


VPC endpoint service: I observe, we mention glb here. And no option to mention security VPC.

<img width="584" height="304" alt="image" src="https://github.com/user-attachments/assets/aa701082-9e2c-4747-b471-2d589140ecd7" />


<img width="502" height="134" alt="image" src="https://github.com/user-attachments/assets/85282836-71ad-417b-83ef-0f34077086e3" />

cross region also supported.

<img width="544" height="203" alt="image" src="https://github.com/user-attachments/assets/b64594d3-02d8-42a7-97ee-03e737956998" />


VPC Endpoint :

<img width="550" height="212" alt="image" src="https://github.com/user-attachments/assets/e1bdb211-8e18-45d9-ab8d-a0a2af238ee2" />

<img width="521" height="240" alt="image" src="https://github.com/user-attachments/assets/6920c5f7-d31f-4ac0-8e99-ad9e8e6bff7e" />

Provide the vpc endpoint service name here and verify. Then select the course VPC 

<img width="474" height="130" alt="image" src="https://github.com/user-attachments/assets/2155ff3a-2186-4b36-9b86-5f48e8be6709" />

Give the Course VPC and its subnets, then create end point.

<img width="554" height="240" alt="image" src="https://github.com/user-attachments/assets/9dcf202e-a23b-4ddf-a954-bc6dafc9a9b2" />












