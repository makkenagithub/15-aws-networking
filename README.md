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

IMPORTANT THING is WE NEED TO UPDATE ROUTE TABLE OF PRIVATE SUBNET in course VPC with the VPC END POINT. Then the flow will be 

ec2 in private subnet course VPC -> route table -> VPC end point -> VPC endpint service -> glb -> target group -> ec2

Here we are updating route table with all traffic 0.0.0.0/0 wih vpc endpoint. vpc endpoint option is coming with gateway load balancer endpoint option , observe

<img width="539" height="230" alt="image" src="https://github.com/user-attachments/assets/9059cad9-2833-4323-8b42-770c7b77bc89" />

Now the all the flow are enables.

Verify:

login to ec2 in security VPC and run TCP dump commad and see IP of ec2 in course VPC.

```
sudo tcpdump -nvv 'port 6081'
```
<img width="488" height="159" alt="image" src="https://github.com/user-attachments/assets/15c99a5f-f50b-411c-8e12-5f31115b83ff" />



### AWS WAF and Shield

<img width="531" height="300" alt="image" src="https://github.com/user-attachments/assets/9a8d9342-67f3-4c62-9bd9-96fc4996d7dc" />


We create a VPC and its compoenents subnets route tables etc. Adn then create an ALB with target group of an ec2 instance in the subnet. 


<img width="545" height="275" alt="image" src="https://github.com/user-attachments/assets/1ece923e-e895-4ceb-9723-ad11eacb231d" />

Initially create IP sets and provide IPs which needs to be blocked. We will give IP sets while creating WAF.

<img width="307" height="259" alt="image" src="https://github.com/user-attachments/assets/f2e00de3-1a0e-4218-9ba8-1591ad48a515" />

Here we gave our laptop IP
<img width="129" height="62" alt="image" src="https://github.com/user-attachments/assets/63c51d80-3964-435f-a570-2f56c3496582" />



click on web ACL and see region comes as global. Choose region here, create web ACL 


<img width="535" height="261" alt="image" src="https://github.com/user-attachments/assets/52e87e8a-bda1-4419-aee7-642b2b32d845" />


<img width="509" height="256" alt="image" src="https://github.com/user-attachments/assets/3d61c68e-c519-4b38-bd76-0d7ace0da1e6" />

click on Add AWS resources and add ALB

<img width="519" height="251" alt="image" src="https://github.com/user-attachments/assets/749364c3-14e8-40f1-b829-2cebf2e37213" />

<img width="371" height="229" alt="image" src="https://github.com/user-attachments/assets/b8405d43-2e9d-412f-87f7-c5ffcdca245e" />

<img width="530" height="224" alt="image" src="https://github.com/user-attachments/assets/0bcb7d88-1508-4cca-8183-93ea8cb1c5c6" />

<img width="416" height="199" alt="image" src="https://github.com/user-attachments/assets/75591794-a186-4f4d-bac7-134b4bd9e143" />

<img width="283" height="240" alt="image" src="https://github.com/user-attachments/assets/b65c4456-aeb4-414f-9de8-853dd80d400f" />

<img width="300" height="185" alt="image" src="https://github.com/user-attachments/assets/3bd6fe94-135f-4d53-b4e8-d3ed5cec909d" />

click next
<img width="303" height="210" alt="image" src="https://github.com/user-attachments/assets/12218f9c-ccee-4a96-bc2c-7878849f6e81" />

click next and then go with default things 
<img width="436" height="210" alt="image" src="https://github.com/user-attachments/assets/b1f720d5-b6c9-45c7-ad09-33c254946505" />

<img width="433" height="246" alt="image" src="https://github.com/user-attachments/assets/8b65adfe-8c69-4547-b46a-92ef1c76cb54" />

Now open the ALB url and access the application. It wont open as we blocked our laptop IP.

<img width="420" height="113" alt="image" src="https://github.com/user-attachments/assets/da6330f9-32ab-40c8-abed-e2f25ce3e800" />



### AWS Global accelerator:
For eg: application is running in mumbai and frankfurt regoins. Now if the user request comes from Europe, then the user request is served from nearest location to the user using AWS global accelerator. AWS global accelerator redirects the user requst to nearest location and helps in reducing latency and serves the user request in quick time.

<img width="548" height="298" alt="image" src="https://github.com/user-attachments/assets/ed999eac-b96e-4629-9495-207f981b59ac" />

Here we are creating a VPC in mumbai and another VPC in Frankfurt. And all the VPC internal componenets such as as subnets, routes, internet gateway, ALBs, target groups are created.

<img width="499" height="298" alt="image" src="https://github.com/user-attachments/assets/853e99a3-9763-4e63-904c-4a82c7221447" />

Global accelerator will route the user request to concerned ALB based on geo location. 

Create Global accelerator:

Global accelarator is a global resource. It does not matter in which region you are in.

<img width="551" height="277" alt="image" src="https://github.com/user-attachments/assets/680eb76d-2cf8-4f8e-9f18-034a2e66f8d1" />

Application Aapache is installed on target group ec2 instances. It runs on port 80. So we enabled port 80 in target group, ec2 security group, ALB security group. 

So in global accelerator also we are giving same port as below

<img width="542" height="213" alt="image" src="https://github.com/user-attachments/assets/bc32b5f6-4ead-4a0a-8422-b8a15fd5e05c" />

Add end point gorups as the two regions where the ALB is added i.e. mumbai and frankfurt

<img width="566" height="267" alt="image" src="https://github.com/user-attachments/assets/ac9c1e8d-7caa-4922-84a9-703ffeb4aa18" />

Then add endpoints as ALB

<img width="569" height="256" alt="image" src="https://github.com/user-attachments/assets/a8ab914e-cba9-402c-824d-b0ed3656cf39" />

endpoint 1 from mumbai

<img width="419" height="186" alt="image" src="https://github.com/user-attachments/assets/1b7f3e11-e548-4126-9e62-fc1a902ae977" />

end point 2 from frankfurt

<img width="445" height="215" alt="image" src="https://github.com/user-attachments/assets/e964df93-9a12-4bc4-8dea-8cdada5b5bc8" />

<img width="566" height="225" alt="image" src="https://github.com/user-attachments/assets/781df271-2cdf-4816-91b9-52d73c976a04" />

We see DNS name for global accelerator

<img width="541" height="256" alt="image" src="https://github.com/user-attachments/assets/e900c1ed-2b80-48ab-9de6-a4ce17aa28eb" />

So when we open the DNS of global accelerator, then the request goes to ALB which nearer to you. 

On the above we get few IPs , these are associated with global accelerator.


### Route53

NS - Name servers , which will help in resolving the DNS.
A record - we provide the IP here, in which application is running
CNAME - another domain redirect

First we need to purchase a domain from domain registrar and then we need to create a hosted zone in aws.

hosted zone:

<img width="543" height="278" alt="image" src="https://github.com/user-attachments/assets/81bbd636-875e-449e-98da-9aa5c9e75829" />

As soon as we create a hosted zone with our own domain name, then it provides name servers and SOA as below

<img width="542" height="277" alt="image" src="https://github.com/user-attachments/assets/31a34292-7cf3-47ef-b6ad-d0698c1c0233" />

These name servers needs to be updated in the settings of where we purchased the domain. So that whenever user hits the url, then request goes to domanin -> hostedzone name servers.

<img width="358" height="204" alt="image" src="https://github.com/user-attachments/assets/eaa52c2b-eae2-4c10-b0af-8886914f4096" />


A record for IP:

<img width="529" height="260" alt="image" src="https://github.com/user-attachments/assets/330b9cb9-b6d0-406f-8a98-34305385c09f" />

choose simple routin now and see we have different options of routing policy, weighted, geo location, failover etc.

<img width="495" height="229" alt="image" src="https://github.com/user-attachments/assets/8b0db213-a1c1-4137-aefb-707b5ed6effb" />

enter public IP of ec2.

<img width="283" height="293" alt="image" src="https://github.com/user-attachments/assets/c67e4d90-4358-4429-a662-a6537cb402a2" />

<img width="495" height="188" alt="image" src="https://github.com/user-attachments/assets/d6f85675-af0f-407b-bdc0-339ad6f334ad" />

<img width="524" height="226" alt="image" src="https://github.com/user-attachments/assets/44e7a99c-5ea3-48ce-9c4f-d9b757939c99" />


But in reality, we do not use A record, as IP changes frequently. So we use LB.

A record for ALB:

AWS route 53 -> hosted zone -> create record -> A record -> choose alias for APP or classic LB and its region, as below and select ALB url

<img width="264" height="284" alt="image" src="https://github.com/user-attachments/assets/9e7b3228-fd6d-479e-95b5-8ffe8bc118e1" />

<img width="521" height="196" alt="image" src="https://github.com/user-attachments/assets/b4183a34-4409-4a92-b4c3-a8d1bf71f2cc" />


Weighted Routing:

<img width="352" height="181" alt="image" src="https://github.com/user-attachments/assets/cc612b07-6010-492a-8b3a-04986f956182" />

Here we created 2 VPCs and its internal components. And 2 ALBs. One ALB can point to one VPC , similarly second one.

Choose weighted routing

<img width="500" height="241" alt="image" src="https://github.com/user-attachments/assets/8e4d804e-c420-4b1f-afb5-966d2f2689f4" />

<img width="356" height="267" alt="image" src="https://github.com/user-attachments/assets/9c632622-09b6-497d-aa38-4c17fd1edc4c" />

<img width="373" height="227" alt="image" src="https://github.com/user-attachments/assets/d9f5f070-59f5-4f73-9e6e-f7ca70d9b70a" />

weight is allowed between 0 to 255. Create 2 weighted records with weight of 128 each. Choose A record, ALB, its region and alb url

<img width="268" height="287" alt="image" src="https://github.com/user-attachments/assets/9ffe5964-5fc6-41ac-9705-73b5ef4c14b5" />

<img width="401" height="228" alt="image" src="https://github.com/user-attachments/assets/4285a5bc-de4f-4158-a2dc-cb32ec8d5ab2" />








