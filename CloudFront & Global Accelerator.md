# CloudFront
- Content Delivery Network (CDN) 
-  Improves read performance, content is cached at the edge 
- Improves users experience 
- 216 Point of Presence globally (edge locations)  
- DDoS protection (because worldwide), integration with Shield, AWS Web Application Firewall
### Cloud Front origins
- S3 bucket
	- For distributing files and caching them at the edge 
	- Enhanced security with CloudFront Origin Access Control (OAC) 
	- OAC is replacing Origin Access Identity (OAI) 
	- CloudFront can be used as an ingress (to upload files to S3) 
- Custom Origin (HTTP) 
	- Application Load Balancer 
	- EC2 instance 
	- S3 website (must first enable the bucket as a static S3 website) 
	- Any HTTP backend you want
### CloudFront vs S3 Cross Region Replication 
- CloudFront: 
	- Global Edge network 
	- Files are cached for a TTL (maybe a day) 
	- Great for static content that must be available everywhere 
- S3 Cross Region Replication: 
	- Must be setup for each region you want replication to happen 
	- Files are updated in near real-time 
	- Read only 
	- Great for dynamic content that needs to be available at low-latency in few regions
## CF w/ ALB as an origin
- ec2 must be public
- all public ip of edge locations
- ALB must be public
	- ec2 may be priv
- allow secruity group of LB
- all pub ip of edge locations
## CF geo restriction
- you can restrict who can access your dist.
- allow list - allow users to access only if theyre in one of the approved countries
- block list: Prevent your users from accessing your content if they're in one of the countries on a list of banned countries.
## CF - Price Classes
- You can reduce the number of edge locations for cost reduction 
- Three price classes: 
	1. Price Class All: all regions – best performance 
	2. Price Class 200: most regions, but excludes the most expensive regions 
	3. Price Class 100: only the least expensive regions
## CF Cache invalidation
- In case you update the back-end origin, CloudFront doesn’t know about it and will only get the refreshed content after the TTL has expired 
- However, you can force an entire or partial cache refresh (thus bypassing the TTL) by performing a CloudFront Invalidation 
- You can invalidate all files * or a special path /images/*
# Global Accelerator
- use case:
	- You have deployed an application and have global users who want to access it directly.
	- They go over the public internet, which can add a lot of latency due to many hops 
	- We wish to go as fast as possible through AWS network to minimize latency
- Unicast IP vs Anycast IP 
	- Unicast IP: one server holds one IP address 
	- Anycast IP: all servers hold the same IP address and the client is routed to the nearest one
- Leverage the AWS internal network to route to your application 
- 2 Anycast IP are created for your application 
- The Anycast IP send traffic directly to Edge Locations 
- The Edge locations send the traffic to your application
- Works with Elastic IP, EC2 instances, ALB, NLB, public or private 
- Consistent Performance 
	- ntelligent routing to lowest latency and fast regional failover 
	- No issue with client cache (because the IP doesn’t change) 
	- Internal AWS network 
- Health Checks 
	- Global Accelerator performs a health check of your applications 
	- Helps make your application global (failover less than 1 minute for unhealthy) 
	- Great for disaster recovery (thanks to the health checks) 
- Security 
	- only 2 external IP need to be whitelisted 
	- DDoS protection thanks to AWS Shield
### Global Accelerator vs Cloud Front
- They both use the **AWS global network and its edge locations** around the world 
- Both services integrate with AWS Shield for DDoS protection.
- Cloud Front
	- Improves performance for both cacheable content (such as images and videos)
	- Dynamic content (such as API acceleration and dynamic site delivery) 
	- Content is served at the edge
- Global Accelerator
	- Improves performance for a wide range of applications over TCP or UDP 
	- Proxying packets at the edge to applications running in one or more AWS Regions.
	- Good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP 
	- Good for HTTP use cases that require static IP addresses 
	- Good for HTTP use cases that required deterministic, fast regional failover