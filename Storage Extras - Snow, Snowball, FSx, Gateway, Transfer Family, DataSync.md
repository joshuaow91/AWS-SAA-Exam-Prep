# AWS Snow
- used to collect and process data at the edge and migrate data into and out of AWS
- Data Migration
	- snowcone
	- snowball edge
	- snowmobile
- edge computing
	- snow cone
	- snow mobile
- if it takes more than a week to transfer data, use snow devices
- physical transfer
### snowball edge - move TBs or PBs
- edge storage
- compute
### snowcone
- light - 4.5 lbs

**snow ball cannot directly import to glacier
you must use s3e first, snsowball > import to s3 > s3 lifecycle policy > glacier

### FSx
- allows third party file systems on AWS
- fully managed
- lustre, windows file server, openZFS, netApp ONTAP
- **HPC - keyword to look for to know you need FSx for lustre
	- seamless integration with s3, can write the output of the computations back to s3 
- file sys deploy
	- scratch - temp storage
		- high burst
		- short term process opt costs
	- persistent file system
		- long term
		- replicated data within same az
		- replace failed files in mins
		- long term processing sensitive data
- net app
	- storage shrinks or grows
	- point in time instantaneous cloning
- OpenZFS
	- NFS protocol
	- move workloads runnning on zfs to aws
	- point in time instantaneous

## Storage Gateway
- hybrid cloud
	- part on premises
	- part on cloud
- bridge between on premises and cloud data
- use case
	- disaster recovery
	- back up and restore
	- tiered storage
	- on premises cache and low latency file access
- types of gateway
	- s3
		- standard, s-ia, zone one ia, int tiereing
		- you can have lifecycle policy to transition to glacier
		- NFS and SMB protocol sed to access
	- fsx
		- local cache for frequently accessed data
		- SMB, NTFS, Active Dir
	- volume
		- block storage iSCSI protocol
		- cached vols, stored vols
		- EBS
	- tape
		- VTL virtual tape lib backed by s3 and glacier
## AWS Transfer Family
- fully managed service for file transfers into and out of amazon s3 or EFS using the FTP protocol
- FTP, FTPS, SFTP
## DataSync
- move large amount of data to and from
- on premises / other cloud to AWS - agent req
- AWS to AWS - no agent
- can synch s3, efs, fsx
- replication task can be scheduled, hourly daily weekly
- file perms and metadata are preserved
- one agent can use 10gbps, can set up bandwidth limit
# All Storage Options - Comparison
- S3: Object Storage 
- S3 Glacier: Object Archival 
- EBS volumes: Network storage for one EC2 instance at a time 
- Instance Storage: Physical storage for your EC2 instance (high IOPS) 
- EFS: Network File System for Linux instances, POSIX filesystem 
- FSx for Windows: Network File System for Windows servers 
- FSx for Lustre: High Performance Computing Linux file system 
- FSx for NetApp ONTAP: High OS Compatibility 
- FSx for OpenZFS: Managed ZFS file system 
- Storage Gateway: S3 & FSx File Gateway, Volume Gateway (cache & stored), Tape Gateway
- Transfer Family: FTP, FTPS, SFTP interface on top of Amazon S3 or Amazon EFS 
- DataSync: Schedule data sync from on-premises to AWS, or AWS to AWS 
- Snowcone / Snowball / Snowmobile: to move large amount of data to the cloud, physically
- Database: for specific workloads, usually with indexing and querying
