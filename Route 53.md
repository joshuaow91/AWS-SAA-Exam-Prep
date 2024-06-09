**What is DNS?
- Domain name system which translates human friendly hostnames into machine ip addresses
- DNS is the back bone of the internet
- hierarchical naming structure 
- *Terminology
	- domain register
	- DNS records - A, AAAA, CNAME, NS
	- Zone File
	- Name Server - resolve dns queries
	- top level domain TLD
	- second level domain SLD

# Route 53 Overview
- highly available and scalable, managed and *Authoritative* DNS
	- authoritative = customer can update the DNS records
- Ability to check health of resources
- only AWS service which provides 100% availability SLA

## Records
- how you want to route traffic
- each record contains
	- domain/subdomain
	- record type
	- value
	- routing policy
	- TTL
- **MUST KNOW: Record Types - A / AAAA / CNAME / NS
	- A - maps a hostanme to ipv4
	- AAAA - maps a hostname to ipv6
	- CNAME - maps a hostname to another host name
		- The target is a domain name which must have A or AAAA record
		- **Can't create a CNAME for the top node of a DNS name space (Zone Apex)
	- NS - name servers for the hosted zone
		- control how traffic is routed to a domain
- **Hosted zones
	- a container for records that define how to route traffic to domains
	- Public hosted zone - route traffic on internet
	- Private hosted zones - how you route within one or more VPCs private domain names
- .50 cents per month per hosted zone
- **Alias records can be used for top node of a DNS name space
	- can't set TTL with alias record
	- targets
		- ELB
		- cloudfront dist.
		- API gateway
		- elastic beanstalk
		- S3 websites
		- VPC interface endpoints
		- Global accelerator
		- route 53 record in same hosted zone
		- **you cannot set an alias record for an EC2 DNS name**
## Routing Policy
- defines how route 53 routes to DNS queries
### Simple 
- routes traffic to single resource
- can specify multiple values in the same record
- if multiple values returned a random is chosen
- when alias is enabled only one resource as target
- cant be associated with health checks
### Weighted
- control % of requests to go to specific resource
- DNS records must have same name and type
- can be associated with health checks
- use case
	- load balancing between regions, testing new application versions
- **assign a weight of 0 to a record to stop sending traffic to a resource
- **if all records have a weight of 0, then all will be returned equally**

### Health Checks
- only for public resources
- automated dns failover
- monitor endpoints
- monitor other health checks
- health checks that monitor cloud watch
### Failover
- one primary and secondary only
- primary must be associated with health check
- fail over will be to a second record
### Latency Based
- redirect resource with lowest latency 
- measured based on how quick it is for users to connect between resource and aws region
- can be associated with health checks 
	- has failover capability
### Geolocation
- different from latency based
	- based on where user is located
- should create a default in case of no match
- use case
	- website localization
	- restrict content distribution
	- load balancing
- can be associated with health checks
### Geoproximity
- route based on geographic location
- shift traffic to resources based on defined bias
- specify with bias values
- 1 - 99 to expand
- -1 - -99 decrease = less traffic
- **shift traffic from one region to another by setting bias**
### IP Based
- routing based on client ip
- CIDR list provided for clients 
- user case: optimize performance, locations
- route end users from a particular ISP to a specific endpoint
### Multi Value
- route traffic to multiple resources
	- 53 returns multi value/resources
- can be associated with health checks
- not a sub for ELB
	- client side load balancing
- up to 8 healthy records are returned for each multi value query

**Domain Registrar vs. DNS Service
- buy domain anywhere
	- provides DNS service
	- you can use another DNS service to manage DNS records
- nameservers from registrar should be changed to use Route 53 public hosted zone name servers to use route 53 as DNS service provider



