# Docker Swarm to Kubernetes (EKS) Migration

## Overview
This repository documents a real-world **production migration** from a Docker Swarm–based EC2 deployment to **Kubernetes (Amazon EKS)**.

The migration focused on improving scalability, security, deployment reliability, and operational efficiency while ensuring **zero downtime**.

> 🔒 **Note:** This is a production case study.  
> Source code and full infrastructure manifests are not publicly available due to organizational policies.

---

## Architecture – Before Migration

The original platform was running on **AWS EC2 instances** with containerized workloads orchestrated using **Docker Swarm**.

### Key characteristics
- Applications deployed using **Docker Compose**
- **Docker Swarm** used for orchestration
- **Nginx Ingress** for traffic routing
- **SSL/TLS** managed via **Cert-Manager with Let’s Encrypt**
- Some services exposed via **Route53 → public EC2 IPs**
- **Bitbucket** as the source code repository
- **Jenkins** for CI/CD pipelines
- **Amazon RDS (MySQL)** as the database
- **Auto Scaling Groups (ASG)** for scaling EC2 instances
- All services running as containers across multiple EC2 instances

### Challenges
- Limited self-healing and orchestration capabilities
- Manual deployments and rollback complexity
- Higher operational overhead
- Difficult environment separation (dev / stage / prod)
- Slower release cycles

---

## Architecture – After Migration

The platform was migrated to **Amazon EKS** using **GitOps and automation-first principles**.

### Key improvements
- Provisioned **EKS clusters** for:
  - Development
  - Staging
  - Production
- Implemented **Argo CD** for GitOps-based continuous delivery
- Deployed **AWS Load Balancer Controller** with **Application Load Balancer (ALB)**
- Centralized observability using **Prometheus and Grafana**
- Converted Docker Compose configurations into:
  - `deployment.yaml`
  - `service.yaml`
- Managed environment-specific Kubernetes manifests using **Bitbucket branches**:
  - `dev`
  - `stage`
  - `prod`
- Implemented **AWS WAF** rules for application security
- Reused existing **VPC and RDS MySQL**, with required **security group updates**
- Implemented **CDN** for performance optimization
- Integrated **Amazon SES** for email services
- Configured **Bitbucket webhooks** to trigger Jenkins pipelines automatically on code push

---

## My Role & Responsibilities
- Designed and executed the migration strategy from Docker Swarm to Kubernetes (EKS)
- Converted Docker Compose files into Kubernetes manifests
- Set up multi-environment Kubernetes clusters (dev, stage, prod)
- Implemented GitOps workflows using Argo CD
- Integrated Jenkins CI pipelines with Bitbucket webhooks
- Configured ALB ingress and AWS WAF for secure traffic management
- Implemented monitoring and alerting using Prometheus and Grafana
- Coordinated production cutover with zero downtime
- Updated networking and security groups to support Kubernetes workloads

---

## Impact & Results
- 🚀 Migrated containerized workloads from Docker Swarm to Kubernetes (EKS)
- ⏱ Reduced deployment time from **~1 day to ~30 minutes**
- 🔁 Enabled declarative deployments with instant rollback using GitOps
- 📉 Reduced manual operational effort by **~60%**
- 🔐 Improved security using WAF and centralized ingress
- 📈 Improved scalability, reliability, and environment isolation
- ⚙ Enabled automated CI/CD triggers on every code change

---

## CI/CD & GitOps Workflow

``text
Code Push (Bitbucket)
   → Jenkins Pipeline Trigger (Webhook)
      → Build & Validation
         → Git Update (Environment Branch)
            → Argo CD Sync
               → Kubernetes Deployment''

---

## Role & Responsibilities
- Led the end-to-end migration of containerized workloads from Docker Swarm on EC2 to Kubernetes (Amazon EKS)
- Analyzed existing Docker Compose configurations and converted them into Kubernetes manifests (Deployments, Services, Ingress)
- Designed and implemented multi-environment Kubernetes clusters (dev, stage, prod)
- Introduced GitOps-based delivery using Argo CD for declarative deployments and rollback
- Integrated Jenkins CI pipelines with Git-based deployment workflows
- Implemented ALB-based ingress and AWS WAF for secure traffic routing
- Coordinated production cutover with zero downtime
- Optimized operational workflows, reducing manual intervention significantly

---

## Lessons Learned
- Kubernetes provides stronger reliability and scalability than Docker Swarm
- GitOps significantly improves deployment confidence and rollback safety
- Proper ingress and WAF configuration are critical for production security
- Environment isolation reduces deployment risk
- Observability is essential during large-scale migrations