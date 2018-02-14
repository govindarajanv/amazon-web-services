https://github.com/scarolan/aws_ha_chef
https://docs.chef.io/install_server_ha.html
There is no official HA support in open source chef. This is a feature of private chef, opscode's commercial offering.

Personally I question the need for HA. If you chef server goes off-line the servers it manages will continue to run unaffected. Instead I would recommend focusing on DR, the ability to recover my chef server in the event of an outage.

"Cookbooks", "environments", "roles" and "databags" are normally kept under revision control. What's missing for DR is the transactional data namely "nodes" and "clients". For these I'd recommend investigating the new download and upload knife commands that came with Chef 11.

Option #1:
==========

Having one standalone chef server, high availability is achieved by making use of Auto scaling group and snapshots. Here minimum one node should always be running, if that node goes down, Auto scaling group will spin up the instance when the health check fails. It uses the latest AMI and snapshot to spin up the instance.
