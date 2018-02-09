Option 1: VIP Fail over Method
===============================

-	Two nodes for haproxy in different subnets (AZ)
-	one acting as the master and the other as the slave
-	one Elastic IP is needed which is public and static.
-	Both these nodes will have keepalived and haproxy packages installed.
-	One node will always get the traffic and the other node (slave) will be idle as long as the master is healthy
-	In case of any failure of the master node, slave node will assume the role of the master and do load balancing.
-	In Verizon, this is done by using two nodes and reserving one static private ip in addition to private ips assigned to the instances.
-	In case of AWS, we need to achieve the above functionality by handling the static ip part of keepalived. 
-	The switch would be achieved using a script that is triggered by the failing node to notify the peer about its failure and provides a way to perform the action on aws resources via that trigger.

VIP failover in AWS VPC across AZ’s with Keepalived. The main problem in AWS is that this provider is blocking the multicast traffic in the VPC’s. To circumvent this we need to switch to unicast for the LVS/IPVS cluster communication. Another issue is the challenge of the virtual environment it self, more specific the VIP failover. In the virtual world it is not enough to move the VIP from one host to another but we also need to inform the physical host Hypervisor platform (Xen,KVM etc) about the change so the traffic can be correctly routed to the new destination via its SDN

Pros:
-----

-	High Availability and Fault tolerant as nodes are in different AZ

Cons:
-----

-	Need of public Elastic IP as a common 'via media' since nodes are in different subnets
-	Actions to be performed post the trigger will have to be achieved through a script and AWS CLI. So there is a chance of storing the keys which may lead to security loopholes as nodes in public subnet can access to internet as well as nodes in private subnets.
-	The above loop hole can be hardened using NACL and Security group and by having an isolated subnet and security group.

References: 

https://www.exasol.com/support/browse/SOL-522

Option 2:
=========



ENI

-	to bind multiple ENIs to a single instance so that you can connect to multiple subnets from the same machine
-	using Elastic Network Interfaces you have the ability to assign static IP addresses:
ELB

-	External Elastic Load Balancers can only route to public subnets

Bastion Host
-	set up a proper bastion host in the public subnet to be able to connect to instances in the private subnet. 
The bastion host should be the only point of entry to instances in your private subnet
