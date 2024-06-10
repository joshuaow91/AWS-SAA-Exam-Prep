## Encryption
- you can ecrypt objs
- SSE
	- SSE s3 managed keys enabled by default
	- SSE KMS - KMS key
	- SSE - C - manage own keys
- Client Side Encrypt
### SSE-S3
- using a key handled, managed and owned by AWS
- server side
- AES-256 - for head
- enabled by default
### SSE KMS
- Key management service
- user control over key
- audit key usage with cloud trail
- aws:kms head
- limitations
	- api calls into kms service
	- count towards quotas
### SSE-Customer
- managed outside AWS
- aws does not store key
- https must be used
- key passed in headers for every request
### Client Side Encryption
- clients must encrypt before sending
- client must decrypt as well
- client fully manages keys and cycle
### DSSE-KMS
- double encryption based on KMS - not on exam
## Encryption in flight SSL/TLS
- s3 endpoints - http, https(encrypted in flight)
- https is recommended
- SSE-C https is mandatory
### Force encryption in transit
- bucket policy - SecureTransport boolean
## Default Encryption
- force encrypt using a bucket policy. 
## S3 Cors
- cross origin resources sharing 
- configure cors headers, all for specific origin or *
- configured in s3 bucket
## S3 MFA Delete
- forces users to generate a code on a device before doing important s3 operations
- mfa is required
	- perm delete
	- suspend versioning
- only root account can dis and en MFA
- MFA is set up through aws CLI
## Access Logs
- log all access for audit purposes
- don't set logging bucket to be the monitored bucket - endless loop
## Pre-signed URLs
- generate urls - user inherits perms from user that gen the file
- exp 1 min - 720 min (12 hours)
- keep files private while sharing (temp access for down or upload)
## Glacier Vault & S3 Object Lock
- adopt a WORM - write once read many
- vault lock policy - lock for future edits
- helpful for compliance and data retention
- versioning must be enabled for object lock
	- WORM
	- block an obj ver for a specified amount of time
	- retention mode - compliance
		- same as GVL
	- retention mode - governance
		- most users can't override or delete
	- retention period must be set - can be extended
	- Legal hold - protects indefinitely 
## Access Points
- policy to grant read write acc to /{prefix}
- each point has it's own sec.
- simplifies sec management
- its own DNS name
- manage sec at scale
- VPC origin 
	- can define to only be accessible within a VPC
### Object Lambda access point
- use aws lambda function to change the obj before it's retrieved
- access point, then it invokes the lambda then s3 access then bucket 
- use case
	- redact PII
	- convert data from xml to json
	- resizing and water marking images