# ACA-Capstone-Project-Highly-Available-PHP-Web-Application-on-AWS
This repository documents the solution architecture, implementation steps, and deployment outcomes for the AWS Academy Cloud Architecting (ACA) – Capstone Project. The project demonstrates the design and deployment of a secure, highly available, multi-tier application on AWS using best practices.
Below is a **clean, professional, British-English GitHub README** for your Capstone Project.
It is structured for recruiters, technical reviewers, and AWS portfolio presentation.

If you want a version with **badges, emojis, or a table of contents**, I can tailor it further.

---

## 📌 **Project Overview**

The scenario involves modernising an existing PHP application and MySQL database initially hosted on a single public EC2 instance. The goal is to migrate this monolithic, insecure setup into a **scalable, resilient, and secure AWS environment**.

This project showcases cloud architecture skills including:

* Secure Amazon RDS database deployment
* Highly available web hosting using ALB + Auto Scaling Group
* Secrets Manager for secure credential storage
* Private subnets for application and database servers
* Data migration into RDS from a SQL dump
* AWS Identity and Access Management (IAM) for service roles

---

# ✅ **Solution Architecture**

The solution implements a **three-tier architecture**:

```
                      ┌──────────────────────────┐
                      │   Internet Users         │
                      └──────────────┬───────────┘
                                     │
                           Public Subnets
                                     │
                      ┌──────────────────────────┐
                      │ Application Load Balancer│
                      └──────────────┬───────────┘
                                     │
                           Private Subnets (Web Tier)
                      ┌──────────────────────────┐
                      │ Auto Scaling Group (ASG) │
                      │  EC2 running PHP app     │
                      └──────────────┬───────────┘
                                     │
                           Private Subnets (DB Tier)
                      ┌──────────────────────────┐
                      │ Amazon RDS MySQL         │
                      │ ‘countries’ database     │
                      └──────────────────────────┘
```

### ✅ **Key Features**

* PHP application deployed across **multiple Availability Zones**
* Application Load Balancer (ALB) distributing traffic
* Auto Scaling Group (ASG) ensuring elasticity and fault tolerance
* Amazon RDS MySQL configured in private subnets
* Database credentials stored in **AWS Secrets Manager**
* Strict security group boundaries:

  * ALB → EC2 (HTTP only)
  * EC2 → RDS (MySQL only)
  * No public access to EC2 or RDS

---

# ✅ **Architecture Principles Applied**

### **Security**

* No public-facing EC2 instances
* Database inaccessible from the internet
* Secrets Manager eliminates hard-coded passwords
* Principle of least privilege enforced via SGs and IAM

### **Reliability**

* Multi-AZ load balancing
* Automatic recovery via ASG
* RDS manages backups and failover (if configured Multi-AZ)

### **Performance & Scalability**

* Web tier scales based on CPU utilisation
* ALB distributes load efficiently across Availability Zones

### **Cost Optimisation**

* Use of t2.micro instances
* Minimal optional features enabled
* Resources stopped/terminated when not in use

---

# ✅ **Implementation Steps**

### **1. Create Amazon RDS MySQL Database**

* Use DB Subnet Group provided by lab
* Store credentials in **AWS Secrets Manager**
* Ensure security group allows MySQL access only from EC2

### **2. Configure Application Load Balancer**

* Deploy in public subnets
* Create target group for EC2 instances
* Configure health checks

### **3. Launch Application Servers via Auto Scaling**

* Use **Example-LT** launch template (preloaded with PHP and SQL dump)
* Deploy ASG across private subnets in at least two AZs
* Attach ASG to ALB target group
* Use target tracking policy

### **4. Import SQL Data to RDS**

* Access SQL dump via EC2 from launch template
* Connect using RDS endpoint
* Import into the `countries` database

### **5. Test the Application**

* Access via ALB DNS name
* Verify:

  * Homepage loads
  * Query page retrieves MySQL data

---

# ✅ **Technologies Used**

* **Amazon EC2 (Auto Scaling + Launch Templates)**
* **Amazon RDS for MySQL**
* **Elastic Load Balancing – Application Load Balancer**
* **Amazon VPC (public/private subnets)**
* **AWS Secrets Manager**
* **Amazon Linux 2023**
* **IAM Roles & Policies**

---

# ✅ **Deliverables Included**

* ✅ Architecture Diagram
* ✅ Fully deployed and functional PHP application
* ✅ Data imported into RDS
* ✅ Written design summary (in this README)

---

# ✅ **Lessons Learned & Reflection**

This project demonstrates the transformation of a legacy monolithic application into a secure, highly available cloud-native architecture. It solidifies core cloud architecting skills in:

* Multi-tier design
* Secure networking
* Database migrations
* Automation and scaling
* Applying AWS Well-Architected Framework principles

It also reinforces cost-awareness in a restricted lab environment.

---

# 📜 **License**

This project is part of the AWS Academy Cloud Architecting curriculum.
Do not distribute proprietary lab materials.

---


