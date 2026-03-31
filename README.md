Highly Available 3-Tier Web Application on AWS
1. Objective
The objective of this project is to design and deploy a highly available, scalable, and fault-tolerant 3-tier web application on AWS. The system should handle traffic spikes efficiently and provide secure access over HTTPS.

2. Business Scenario
A fast-growing e-commerce startup requires a web application infrastructure that:
•	Handles high traffic during sales events (e.g., Black Friday)
•	Ensures high availability across multiple Availability Zones
•	Supports auto scaling based on demand
•	Provides secure HTTPS access
•	Maintains session persistence for shopping cart functionality

3. AWS Services Used
•	Amazon EC2
•	Application Load Balancer
•	Auto Scaling Groups
•	Amazon EBS
•	Amazon S3
•	AWS Certificate Manager
•	AWS KMS
•	Amazon VPC
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/be575cb7-d661-4217-921d-5d5fe0525665" />


4. Architecture Overview
Internet
↓
Application Load Balancer (HTTPS:443)
↓
Target Group
├── AZ-1a: EC2 Instance (Auto Scaling Group)
├── AZ-1b: EC2 Instance (Auto Scaling Group)
↓
EBS gp3 (Encrypted Storage)
↓
S3 Bucket (Static Assets)
↓
Bastion Host (Public Subnet for SSH Access)

5. Infrastructure Setup
5.1 Create VPC and Subnets
•	Create a VPC with CIDR (e.g., 10.0.0.0/16)
•	Create:
o	2 Public Subnets (for ALB & Bastion Host)
o	2 Private Subnets (for EC2 App Servers)
•	Configure:
o	Internet Gateway (for public subnets)
o	NAT Gateway (for private subnet internet access)

 

5.2 Launch EC2 Instances (Golden AMI)
•	Create a Golden AMI with:
o	Apache/Nginx installed
o	Application code pre-configured
•	Use User Data Script to start services automatically
 
5.3 Configure Storage (EBS)
•	Attach 20 GB gp3 EBS volume to each EC2 instance
•	Enable encryption using AWS KMS
•	Mount volume to application directory

5.4 Setup S3 Bucket
•	Create an S3 bucket for static assets (images, CSS, JS)
•	Enable:
o	Versioning (optional)
o	Public or controlled access
 

5.5 Bastion Host Setup
•	Launch Bastion Host in public subnet
•	Assign Elastic IP
•	Allow SSH access (port 22)

6. Load Balancer Configuration
6.1 Create Application Load Balancer
•	Scheme: Internet-facing
•	Listener: HTTPS (Port 443)
•	Attach ACM SSL Certificate
   

________________________________________
6.2 Configure Target Group
•	Register EC2 instances
•	Health Check:
o	Path: /
o	Protocol: HTTP
•	   
________________________________________
6.3 Enable Cross-Zone Load Balancing
•	Ensure traffic is evenly distributed across AZs
Screenshot:
(Insert Cross-zone LB setting screenshot here)
 
6.4 Configure Sticky Sessions
•	Enable session stickiness
•	Duration: 1 hour
•	Used for shopping cart persistence
 

7. Auto Scaling Configuration
•	Create Launch Template using Golden AMI
•	Configure Auto Scaling Group:
o	Minimum: 2 instances
o	Maximum: 4 instances
o	Desired: 2 instances
•	Enable scaling policy based on CPU usage
 

8. Security Configuration
•	Security Groups:
o	ALB: Allow HTTPS (443)
o	EC2: Allow HTTP (80) from ALB only
o	Bastion: Allow SSH (22) from your IP
•	Enable encryption:
o	EBS via KMS
o	HTTPS via ACM

9. Validation & Testing
9.1 Test Application Access
•	Access application using ALB DNS over HTTPS

 
9.2 Test High Availability
•	Stop one instance → Application should still work

9.3 Test Auto Scaling
•	Simulate load → New instances should launch automatically

10. Key Concepts Covered
•	EC2 Instance Types & AMI
•	User Data Automation
•	Application Load Balancer
•	Cross-Zone Load Balancing
•	Sticky Sessions
•	Auto Scaling Groups
•	Secure Architecture (HTTPS, KMS)

11. Conclusion
This project demonstrates how to build a production-ready, scalable, and highly available AWS architecture. The system ensures:
•	Fault tolerance across Availability Zones
•	Secure communication via HTTPS
•	Dynamic scaling based on demand
•	Persistent user sessions for better user experience

