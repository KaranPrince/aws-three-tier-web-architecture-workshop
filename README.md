# 🌐 AWS 3-Tier Architecture – `mydemoapp.in`

![AWS](https://img.shields.io/badge/AWS-Cloud-orange?logo=amazon-aws)
![Architecture](https://img.shields.io/badge/3--Tier%20App-Web%20%7C%20App%20%7C%20DB-blue)
![IaC](https://img.shields.io/badge/Infrastructure-Manual%20%2B%20ASG-green)
![Security](https://img.shields.io/badge/Security-WAF%20%7C%20IAM%20%7C%20SG-red)

---

## 📌 Overview
This project demonstrates a **real-world AWS 3-Tier Web Application Architecture** with scalability, high availability, and security.  
It is built step by step using **AWS EC2, RDS, ALB, ASG, CloudFront, Route 53, WAF, CloudWatch, SNS, and Budgets**.  

👉 Deployed live using custom domain: **[mydemoapp.in](https://mydemoapp.in)**

---

## 🏗️ Architecture Diagram
![Architecture Diagram](https://github.com/KaranPrince/aws-three-tier-web-architecture-workshop/blob/main/application-code/web-tier/src/assets/3TierArch3D.png)

---

## ⚡ Key Features
- ✅ **3-Tier Separation** – Web, App, DB in separate subnets  
- ✅ **High Availability** – Load Balancing + Auto Scaling for Web & App tiers  
- ✅ **Private RDS (MySQL)** – secured inside DB subnet, no internet access  
- ✅ **CloudFront + Route 53** – CDN + custom domain with HTTPS  
- ✅ **AWS WAF** – Protection from SQLi, XSS, DDoS  
- ✅ **Monitoring & Alerts** – CloudWatch metrics + SNS notifications  
- ✅ **Cost Control** – AWS Budgets + Billing Alerts  
- ✅ **IAM Roles** – Enforced least privilege principle  

---

## 🛠️ AWS Services Used

| **Category**      | **Services** |
|--------------------|-------------|
| **Networking**     | VPC, Subnets, IGW, NAT Gateway, Route Tables, NACLs, Security Groups |
| **Compute**        | EC2 (Jump Server, Web, App), AMI, ASG, ALB |
| **Database**       | Amazon RDS (MySQL) |
| **Storage**        | Amazon S3 (Code storage, logs) |
| **Security**       | IAM Roles, AWS WAF |
| **Monitoring**     | CloudWatch, SNS |
| **Domain/CDN**     | CloudFront, Route 53 |
| **Billing**        | Budgets & Cost Alerts |

---

## 📝 Step-by-Step Implementation

### 🔹 Part 0: Setup
- Download code from GitHub & store in **S3 bucket**.
- Create **IAM EC2 Instance Role** with permissions for S3 + CloudWatch.
- Launch a **Jump Server (Bastion Host)** in public subnet for admin access.

---

### 🔹 Part 1: Networking & Security
- Create **VPC (10.0.0.0/16)**.  
- Setup **Public Subnets** (for Web + Jump) & **Private Subnets** (for App + DB).  
- Add **IGW** + **NAT Gateway (1 shared for App & DB)**.  
- Configure **Route Tables**: Public ↔ IGW, Private ↔ NAT.  
- Setup **NACLs** for each subnet tier.  
- Create **Security Groups**:  
  - Web SG → Allow 80/443 from internet.  
  - App SG → Allow 3000 only from Web SG.  
  - DB SG → Allow 3306 only from App SG.  

---

### 🔹 Part 2: Database Deployment (DB Tier)
- Create **DB Subnet Group** in private subnets.  
- Deploy **Amazon RDS MySQL** inside DB subnet.  
- Restrict access → only App SG can connect on port 3306.  

---

### 🔹 Part 3: App Tier Deployment
- Launch **App EC2** in private subnet.  
- Install **Node.js + dependencies**.  
- Configure `DbConfig.js` to connect with RDS.  
- Validate DB connection.  
- Create **AMI** of working App Server.  

---

### 🔹 Part 4: Internal Load Balancing & Auto Scaling
- Create **App Target Group**.  
- Setup **Internal ALB** for App Tier.  
- Create **Launch Template** with App AMI.  
- Deploy **Auto Scaling Group (ASG)** across multiple AZs.  

---

### 🔹 Part 5: Web Tier Deployment
- Launch **Web EC2** in public subnet.  
- Install **Nginx + React build tools**.  
- Deploy React build → `/usr/share/nginx/html/`.  
- Configure **Nginx reverse proxy** → `/api` routes to App ALB.  
- Validate full Web → App → DB flow.  
- Create **AMI** of Web Server.  

---

### 🔹 Part 6: External Load Balancing & Auto Scaling
- Create **Web Target Group**.  
- Setup **Internet-facing ALB** → routes traffic to Web Target Group.  
- Create **Launch Template** with Web AMI.  
- Deploy **Web ASG** across multiple AZs.  

---

### 🔹 Part 7: CloudFront Setup
- Create **CloudFront Distribution** with Web ALB as origin.  
- Enable **HTTPS (TLS)** and caching.  

---

### 🔹 Part 8: Route 53 Integration
- Register custom domain `mydemoapp.in` in GoDaddy.  
- Create **Hosted Zone in Route 53**.  
- Update GoDaddy nameservers → Route 53 NS.  
- Create **Alias Record** → points to CloudFront.  

---

### 🔹 Part 9: AWS WAF
- Create **Web ACL** with rules: SQLi, XSS, rate limiting.  
- Attach to **CloudFront Distribution**.  

---

### 🔹 Part 10: CloudWatch Setup
- Enable **EC2 & ALB metrics** in CloudWatch.  
- Create **Alarms** → CPU Utilization > 70%.  
- Send alerts via SNS.  

---

### 🔹 Part 11: SNS Alerts
- Create **SNS Topic** (ex: `MyApp-Alerts`).  
- Subscribe with your email.  
- Attach to **CloudWatch alarms**.  

---

### 🔹 Part 12: Budgets & Cost Alerts
- Create **AWS Budget** with monthly cost limit.  
- Configure alerts → send via SNS.  

---

## ✅ Validation Checklist
- [x] Web app loads via `https://mydemoapp.in`  
- [x] Web Tier → App Tier → DB communication works  
- [x] Auto Scaling launches new instances under load  
- [x] CloudFront delivers cached content globally  
- [x] WAF filters malicious traffic  
- [x] CloudWatch alarms send SNS notifications  
- [x] Budget alerts received  

---

## 📸 Screenshots 
- [VPC & Subnets Screenshot]  
- [Security Groups Screenshot]  
- [RDS Setup Screenshot]  
- [App Deployment Screenshot]  
- [Web Deployment Screenshot]  
- [ALB Screenshot]  
- [CloudFront Screenshot]  
- [Route 53 Screenshot]  
- [WAF Screenshot]  
- [CloudWatch & SNS Screenshot]  

---

## 🎯 Conclusion
This project implements a **fully functional AWS 3-Tier Architecture** with best practices in networking, scaling, security, monitoring, and cost control.

---
