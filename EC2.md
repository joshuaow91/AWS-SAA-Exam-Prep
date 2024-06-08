# EC2 - elastic compute cloud

- Infrastructure as a service
##### Sizing and config
- CPU
- RAM
- Storage, network or hardware attached
- Type of network
- Firewall rules
- Bootstrap script

Ec2 user data script. Automates boot tasks, only runs at the instance of first start.
##### Types of ec2 instances
- T2.micro - low to mod
- T2.xlarge - mod
- C5d.4xlarge up to 10gbps
- R5.16xlarge 20gbps
- M5.8xlarge 10 gbps

If you stop an instance and start again, public ip will change
### Instance types basics

- General purpose - great for web servers / code repository - balancer between compute memory and networking
- Compute optimized - great for high level compute task, batch processing, media transcoding, machine learning, gaming servers 
- Memory optimized - fast performance for large datasets in memory - real time processing of big unstructured data
- Accelerated computing
- Storage optimized - great for storage intensive tasks that require high read and write, cache 
- Instance features
- Measuring instance performance

m5.2xlarge
m = instance class
5 = generation of instance
2xlarge = size within the instance class
##### ec2instances.info - website to compare instances

### Security groups
- only contain allow rules
- can reference ip addresses
- fundamentals of network security in aws
- SG are firewalls around ec2 instances
- regulate 
	- access to ports
	- ip ranges
	- inbound and outbound network
- can be attached to multiples instances
- locked down to a region
- if app has a timeout - security group issue
- connection refused - app error
- reference other sec groups
- ### classic ports to know
	- 22 = SSH - log into linux instance
	- 21 = FTP - upload files into a file share
	- 22 = SFTP - upload files using ssh
	- 80 = HTTP - access unsecured websites
	- 443 = HTTPS - access secured websites
	- 3389 = RDP Remote Desktop Protocol - log into a windows instance

### SSH
mac - SSH, EC2 instance connect
ssh - allows you control a remote machine or server all in command line

### EC2 purchasing options
on demand instances - short workload
	- billing per second linux windows
	- other OS billing per hour
	- no long term commitment
reserved - long workloads
	- 72% discount compared to on demand
	- instance type, region, tenancy, OS
	- regional or zonal
	- pay up front, no upfront, partial upfront
	- AWS savings plans is recommended to use instead on 1 or 3 year terms
savings plan - commit to specific amount of usage in dollars, long workloads
	- commit to certain type of usage e.g. 10/hour for 1 or 3 years
	- m5
spot instances - cheap short work loads, less reliable
	- up to 90% discounts
	- define max price, if price goes over you lose the instance
	- batch jobs, image processing, data analysis
	- not suited for critical jobs or dbs
dedicated - no other customers will share your hardware
	- fully dedicated server
	- address compliance requirements
	- most expensive option of aws
	- use case - BYOL licensing model, regulatory or compliance needs
-**dedicated instance, own instace, dedicated host, own server access to lower level hardware**
capacity reservations - reserve capacity in a specific AZ for any duration
	- no time commitment
	- short term uninterrupted workloads

### Spot Instances
- discount up to 90%
- define max spot price
- if current spot price goes over max price, choose to stop or terminate, 2 min grace period

**Pricing
- varies based on AZ

**terminate spot instance
- must be in open, active, disabled state to stop, does not terminate
- to terminate for good and not relaunch must cancel, then terminate
- persisent spot request will auto launch instances if not canceled

**spot fleets
- set of spot instances, on demand
- will try to meet target cap with price constraints
- when budget is reach it stops launching instances
- strategies
	- lowestPrice - spot fleet will launch instances from pool of lowest prices - short workloads
	- diversified - distributed across all pools - long workloads
	- capacityOptimized - optimal cap for number of instances
	- priceCapacityOptimized (recommended) - pools with highest cap avail. then select pool with lowest prices - best for most workloads
- spot fleet allows to auto req spot instances with lowest price

## Private and Public IP
- ipv4 and ipv6
- ipv4 - four numbers separated by dots
- 0-255 per space [0-255].[0-255]

# SAA Level ec2

### ec2 placement groups
- **cluster - grouped into low latency setup in single avail zone
	- same AZ
	- great networking 10gbps bandwidth
	- if AZ fails, all instances fail at same time
	- use case - big data jobs, app that need low latency
- **spread - spread across diff hardware max 7 instances per group per az - critical apps
	- all instances across different hardware - span across multiple AZ
	- reduced risk of simultaneous failure
	- limited to 7 instances per az per placement group
	- use case - app needs high avail
	- critical app, instance failure must be isolated
- **partition - spread across many partitions, scales to 100s of ec2 instances
	- spread across partitions in multiple AZ in same region
	- each par, can hgave many instances
	- each par represents a rack
	- up to 7 par per AZ
	- up tp 100 instances
	- do not share same physical rack, all isolated from failure
	- a failure can affect many ec2 but not other partitions
	- instances get access to par info as metadata
	- use case - cassandra, kafka, big data applications

### Elastic Network Interfaces ENI
- represent a Virtual Network Card
- each eni can have
	- primary private ipv4
	- one or more secondary ipv4
	- one elastic ipv4
	- one public ip
	- one or more sec groups
	- a mac address
- can create independent from instances and attach and move around
- bound to specific AZ
### EC2 Hibernate
- in memory ram state is reserved
- instance boot is much faster
- ram state is written to a file in root EBS volume
- EBS volume must be encrypted
- no more than 60 days hibernated
# EC2 Instance Storage

#### EBS Volumes (Elastic Block Store)
network drive you can attach to an instance while it runs
- persist data after instance is terminated
- multi attach feature for some EBS
- *bound to specific AZ
- 30gb free tier
- can be detached and attached to different instances, quickly
- uses network to communicate the instance, might be latency
- provisioned capacity

possible to have two EBS volumes attached to one instance

**delete on termination attribute (default) - controls ebs behavior when an instance is terminated - option to preserve root volume when instance is terminated

#### EBS Snapshots
- back up at any point in time for EBS volume
- recommended to detach from instance to do snapshot
- can copy across AZ or region
- helpful for transferring from az to another
- snapshot archive 
	- 75% cheaper
	- takes 24 - 72 hrs for restoring
- recycle bin for snapshots - set up rule to retain deleted snapshots
- retention can be set from 1 day to 1 year
- FSR fast snapshot restore - force full init to have no latency cost a lot $

#### AMI - Amazon Machine Image
Represents a customization of an EC2 instance
- add own software, config, os, monitoring
- built for a specific region
- can be copied across regions
- public ami - provided by aws
- aws marketplace ami - made by someone else - sold through marketplace

#### EC2 Instance Store
- if you need a high performance hardware server use and instance store
- better I/O performance
- if you stop or terminate your instance the store will be lost
- can't be used as a durable long term store for data
- use case - temp content, scratch data
- risk of data loss 
- back ups are your responsibility

#### EBS Volume Types
- 6 different types
	- gp2 / gp3 - general purpose SSD volume
	- io1 / io2 block express - highest performance for low laten and *miss crit
	- st 1 - low cost HDD vol for frequent access
	- sc 1 HDD - loest cost for less frequency
- *gp3
	- low latency 1gib - 16 tib
	- gp3 3k IOPS and throughput of 125mb
	- independly set IOPS

**multi attach feature
- attach the same EBS vol to multi EC2 instances in same AZ
- only avail for io1 and io2 family
- each instance has full read and write perms
- higher availibility
- apps must manage concurrent write ops
- up to 16 instances at a time
- must use a cluster aware file system

**EBS encryption
- data at rest encrupted
- all vols, snapshots, data in flight encrypted
- all encryp and decrypt handled tansparently
- minimal impact on latency

create ebs snapshot
encrypt using copy
create new ebs vol
attch encrypted vol

#### Amazon EFS - Elastic File System
- Managed network file system
- can be mounted on many instance in different AZ
- expensive
- pay per use
- highly available

**Storage classes
- storage tiers
	- standard
	- infrequent access
	- archived tier
	- lifcycle policies 
- avil and dura
	- standard 
	- one zone
- over 90% cost savings

**EFS vs EBS
- ebs
	- attached to one at a time, except multi attach
	- locked at AZ level
	- gp2 - i/o increases if disk size increases
	- gp3 - increase i/o independently
- root vols get term by default if instance term
- efs
	- mounting 100s of instances across AZ
	- only for linux
	- higher price point
	- storage tiers for cost savings
- instance store physically attached to ec2
