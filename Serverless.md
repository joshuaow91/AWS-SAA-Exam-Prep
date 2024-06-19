Serverless in AWS

• AWS Lambda  
• DynamoDB  
• AWS Cognito  
• AWS API Gateway • Amazon S3

• AWS SNS & SQS  
• AWS Kinesis Data Firehose • Aurora Serverless  
• Step Functions  
• Fargate

### lambda
• Virtual functions – no servers to manage! • Limited by time - short executions  
• Run on-demand  
• Scaling is automated!

• Easy Pricing:  
• Pay per request and compute time  
• Free tier of 1,000,000 AWS Lambda requests and 400,000 GBs of compute time

• Integrated with the whole AWS suite of services  
• Integrated with many programming languages  
• Easy monitoring through AWS CloudWatch  
• Easy to get more resources per functions (up to 10GB of RAM!) • Increasing RAM will also improve CPU and network!
### lambda limits
- limits to know per region
	- execution
		- memory allocation 128mb - 10gb (15 mins)
		- max ex time
		- env var (4kb)
		- disk cap - 512mb - 10gb
		- 1000 concurrent executions, increased with request
	- Deployment
		- deployment size for func 50mb compressed zip
		- uncompressed code and dep. 250mb
		- can use /tmp dir to load other files at startup
		- size of env 4 kb
### Lambda Snap Start
- improves functions performance up to 10x no extra cost for java 11 and above
- function is pre init, goes straight to invoke phase then shutdown
- snapshot cached for low latency for rapid start
### Lambda Edge and cloud functions
- attached to cloudfront distributions
- lambda@edge and cloud front functions
- deployed globally, no management of servers
- use case customized CDN
#### cloud front functions
- viewer req, origin req, origin response, viewer response with CF in between viewer and origin
- millions of req/second
- high perf high scale
- JS only
- use case
	- cache key, header man, url rewrite, req auth 
#### lambda @ edge
- node or python only
- scales 1000s of req/sec
- viewer/origin req/res
- use case
	- long ex time, network access, file system access or body access of http req
### Lambda in VPC
- by default lambda is launched outside of your own VPC
- it cannot access resources in your VPC
- must define VPC ID and attach security group in lambda function, lambda will create ENI
- RDS proxy
	- if lambda dir acc RDS may open too many connections under high load
	- improve scalability, improve availability, improve security
- **lambda function must be deployed in your VPC, because RDS proxy is never publicly accessible**
### Invoking Lambda from RDS & Aurora
- 