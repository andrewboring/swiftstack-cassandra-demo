# swiftstack-cassandra-demo

This Vagrantfile (and supporting files) creates a three-node Cassandra cluster, a "mgmt" node, and a SwiftStack node.
The goal is to demonstrate a simple backup script to create a snapshot of the Cassasndra keyspace and send it to a backup container in Openstack Swift or SwiftStack.

The passwords are all "password1", and the ssh keys included should be considered "insecure" and not used anywhere outside this demo. 

The Vagrant/Virtualbox environment uses a range of defined IP addresses with Host-Only networking (eth1) and the default/included NAT address (eth0) for Internet access to install software packages. 

