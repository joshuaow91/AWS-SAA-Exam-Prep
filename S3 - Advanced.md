# Lifecycle Rules
- transition objects between storage classes
- for infreq you can move to standard IA
- for archive objs move to glacier deep
## Transition Action
- config trans to class
## Expiration Actions
- access log files be set to delete after a duration of days
- used to delete old versions
- multi part uploads

- Rules can be created explicitly for different tags or prefixes

### S3 Analytics
- help decide when to trans to right storage class
## Requester Pays
- bucket owner pays for all s3 storage and data transfer
- you can enable a request to pay for any downloads of an obj from bucket
- helpful to share large datasets with other accounts
- requester must be auth in AWS
	- cannot be anon
## Event Notifications
- obj created
- obj removed
- obj removed
- replication
- object name filtering possible
	- "only get .jpg"
- use case
	- generate thumbnails of images uploaded to S#
- SNS
- SQS
- Lambda
- unlimited event creations
- typically deliver in seconds
- IAM perms necessary for events to work
	- SNS resource access policy
	- SQS RAP
	- Lambda RP
- Amazon Event Bridge
	- all events end up here no matter what
	- send events to over 18 different services as destinations
	- advanced filtering options
## Performance
- scales to high request rates 100-200ms latency
- no limits to prefixes
- high performance 3,500 put copy post delete or 5,00 get head reqs per sec per prefix
- 100mb uploads recommended to use mult part, 5gb required
- **Transfer Acceleration
	- increase trans speed by xfer to AWS edge location to s3 bucket in target location
	- compatible with multi part upload
- Byte-range fetches
	- parallelize gets by req spec. byte ranges
	- better resilience in case of failures
	- can be used to speed up downloads
	- can be used to retrieve partial amount of file
## Select & Glacier Select
- retrieve less data using SQL by performing server side filtering
- filter by rows and columns
- less network transfer, less cpu client cost
- 400% faster, 80% cheaper
- Simple filtering think Select
## Batch Operations
- perform bulk operation on existing s3 object in a single req
	- modify metadata and props at once
	- copy objs between buckets
	- encrypt or unencrypt
	- restore objs
	- invoke lambda
	- manages retries, tracks progress, sends completion not. generate reports
	- s3 inventory used to get obj list and select to filter objs
## Storage Lens
- understand, analyze and optimize storage across entire AWS org
- create your own dash or use default
- all metrics can be exported in csv
- summary metrics
	- gen insights
	- id fast growing or not used buckets or prefixes
- cost opt
	- insight to manage and opt. storage cost
- data protection
	- data protection features
	- id which not following best practices
- access management
	- ownership settings
- event metrics
	- event notifications
- performance
	- xfer acceleration
- activity
	- requestrs
- status codes
- free & paid
	- 28 usages free
	- data avail 14 days free
- Paid
	- cloud watch pub paid
	- data avail 15 month paid
	- advanced metrics, activity, cost opt, data pro, status code
	- prefix agg.
