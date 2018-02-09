Check the following
====================

For the instances in public subnet

1	Check if EC2 instance is in public subnet and has public ip or elastic ip
2	Check the security group's inbound and outbound rules
3	Check Network Access Control List and its inbound and outbound rules
4	Check subnet's route tables to confirm
	-	if IGW is attached to VPC
	-	if RT has route to IGW
	-	
5	if peering is enabled, ensure that you have updated route table on both ends of the peering by including the allowed CIDR block range.

For the instances in private subnet

1	if peering is enabled, ensure that you have updated route table on both ends of the peering by including the allowed CIDR block range.
2	Ensure Bastion host/jump box is provisioned in public subnet so that user can ssh from jump box into the required node in private subnet
3	Ensure NAT instance/NAT Gateway is provisioned for instances in private subnets so that they can update their packages by connecting to the internet
4	Check security group of source and destination instances
5	Check NACL of source and destination subnets
