#!/bin/sh

#pssh-based backup script for cassandra - DEMO

export DATE=$(date +%Y%m%d)
#export DATE=20160517
export KEYSPACE=mykeyspace

# snapshot directory
export SNAPDIR="/var/lib/cassandra/data/mykeyspace/users-9446386017dd11e692133b2b8b05b7d2/snapshots/"

# user credentials
export CUSER=aboring
export SWIFTBIN=/usr/local/bin/swift
export SUSER=cbkup	
export SPASS=password1
export SHOST=192.168.56.102
export SNAPNAME='mykeyspace-$(hostname)'

#echo $HOSTNAME
#echo $DATE
#echo $KEYSPACE
#echo SNAPDIR

# create bkup container, organized by date.
# this is run on the pssh node. 
# use swift(1), unless you want to run API calls
# using curl(1) directly.

swift -A http://$SHOST/auth/v1.0 -U $SUSER -K password1 post cbkup-$DATE

# the keyspace schemas need to be dumped, but doesn't need to run in parallel like the nodetool commands
# if the cqlsh utility is installed on this mgmt node, you can run something like this:.
# cqlsh -e "DESC KEYSPACE mykeyspace" > mykeyspace.cql && swift [args] upload container mykeyspace.cql

# connect to cassandra nodes, and run nodetool snapshot on the keyspace. 
# use -h to read from a host file, rather than specifying list of nodes
# ref: https://docs.datastax.com/en/cassandra/2.0/cassandra/operations/ops_backup_takes_snapshot_t.html

pssh -i -H "172.28.128.7 172.28.128.8" -l $CUSER "nodetool snapshot mykeyspace -t $SNAPNAME"


# upload to SwiftStack from Cassadnra nodes.
# if swift(1) CLI does not exist on Cassandra nodes,
# you'll need to use use curl(1) and manage pseudo-directories
# and large object manifests manually.

pssh -i -H "172.28.128.7 172.28.128.8" -l $CUSER "swift -A http://$SHOST/auth/v1.0 -U $SUSER -K $SPASS upload cbkup-$DATE $SNAPDIR/$SNAPNAME && nodetool clearsnapshot"
