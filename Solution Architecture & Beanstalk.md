##### 5 Pillars for a well architected application
- Costs
- Performance
- Reliability
- Security
- Operational Excellence
# Stateless Web App - WhatsTheTime.com
- allows ppl to know what time it is
- no DB needed
- start small and accept down time
- fully scale vert and horiz. no downtime
## Architecture
- User
	- what time is it
	- t2.micro instance returns time
- elastic ip
- scaling vert
	- m5.large ec2
		- same public ip because elastic ip
- scaling horiz.
	- adding m5 instances 
	- remove elastic ip
	- leverage route 53
		- A record of ttl 1hr
		- route 53 will keep ip in sync, no need to manage elastic ip
	- add load balancer
		- private ec2, 3 instance in one AZ
		- add ELB w/ health checks
		- security groups to restrict instance directly
		- change A record to alias record since its a ELB
	- auto-scaling group
		- add auto scaling group around instances
			- scale in and out based on demands
		- add multi AZ for high availability
			- ELB and ASG multi-az
	- change to reserved instance for cost savings

# Stateful Web App - MyClothes.com
- allows ppl to buy clothes online, ecommerce
- hundreds of users at same time
- maintain scalability
- sessions for user details and carts
- DB
## 3-tier Architecture
- route 53, muti az ELB
- ASG around multi az instances
- introduce stickness in ELB
	- if an instance gets terminated you lose shopping cart
- another approach
	- user cookies
	- send cart in web cookies
	- http requests get heavier
	- level of security risk
- another approach 
	- server session
	- send a session id in cookies
	- set up elasticache cluster
		- store / retrieve session data
	- alternative use dynamoDB
- store user data in an RDS instance
	- scale reads with RDS read replica
	- alt - lazy loading, looks in cache first 
# Stateful Web App - MyWordPress.com
- wordpress like website
- access and display picture uploads
- user data, blog content, stored in mysql
- scale globally

## Architecture
- route 53
- multi az ELB
- ASG multi AZ instances
- Aurora Mysql
- storing images
	- EBS volume
		- send image to LB, image makes it to EBS
		- scaling problem, each instance will have own EBS
		- wont have access to image on multi AZ/multi instances
	- EFS
		- creates ENI into each AZ
		- storage shared between all instances

# Instantiating applications quickly
- installing and deploying full stack app into our instances
- ec2
	- golden ami - install app, etc beforehand. launch ec2 instance from golden AMI
	- Bootstrap using user data - for dynamic config
	- hybrid - mix golden ami and user data
- RDS
	- restore from snapshot 
- EBS
	- restore from a snapshot
# Elastic Beanstalk
- a dev centric view of deploying an app on AWS
- uses all components - ec2, asg, elb, rds
- managed service
	- automatically handles capacity provisioning, load balancing, scaling, app health monitoring, instance config
	- dev is only respons. for code
- still full control over config
- all bundled in one single interface
- beanstalk free, underlying services paid
## Components
- application
- app version
- environment
	- collection of resources
	- tiers
	- multi envs
- process
	- create app
	- upload version
	- launch env
	- manage env

- **web server tier (website) and worker tier (tasks off a queue)
- **deployment modes - single instance, great for dev / high avail w/ ALB, ASG, Multi-AZ, RDS master and standy Multi AZ



