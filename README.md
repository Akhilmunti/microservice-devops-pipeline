# Cloud DevOps Pipeline for Microservices on AWS

This project demonstrates a complete, automated CI/CD pipeline for deploying a containerized microservice to a Kubernetes cluster on AWS. The entire cloud infrastructure is provisioned using Terraform, and the build-and-deploy workflow is orchestrated by GitHub Actions, with Helm for package management and configurations for robust monitoring.

This repository represents a holistic understanding of modern DevOps practices, from local development and containerization to full cloud automation and Infrastructure as Code (IaC).

---

## Key Achievements & Resume Points

* **Automated a CI/CD pipeline with GitHub Actions and Helm** for a containerized Python application, enabling zero-downtime, rolling deployments to a Kubernetes test environment on every commit.
* **Architected and provisioned a production-grade AWS environment** using Terraform, creating reusable modules for a VPC, EKS cluster, and IAM roles, fully codifying the infrastructure for rapid and repeatable deployments.
* **Integrated a multi-layered monitoring strategy** by writing configurations for the `kube-prometheus-stack` Helm chart for in-cluster metrics and creating AWS CloudWatch alarms via Terraform for infrastructure-level alerting.

---

## ⚙️ Tech Stack

| Category                  | Technologies                                                                 |
| ------------------------- | ---------------------------------------------------------------------------- |
| **Cloud Provider** | AWS (Amazon Web Services)                                                    |
| **Infrastructure as Code** | Terraform                                                                    |
| **Container Orchestration** | Kubernetes (AWS EKS)                                                         |
| **CI/CD** | GitHub Actions                                                               |
| **Containerization** | Docker                                                                       |
| **K8s Package Management**| Helm                                                                         |
| **Monitoring & Alerting** | Prometheus, Grafana (via Helm config), AWS CloudWatch (via Terraform)          |
| **Application** | Python, Flask, Gunicorn                                                      |

---

## 🏗️ Project Architecture & Workflow

This project follows a modern GitOps workflow, where Git is the single source of truth for both application code and infrastructure configuration.

## 🔩 Infrastructure Details (`/infrastructure`)

The Terraform code is fully modular to ensure reusability and maintainability.

* **VPC Module (`/modules/vpc`):** Provisions a production-ready Virtual Private Cloud with public and private subnets across multiple availability zones. It includes an Internet Gateway for public traffic and a NAT Gateway to allow secure, outbound internet access for resources in the private subnets.

* **EKS Module (`/modules/eks`):** Provisions a managed AWS EKS cluster. This includes creating the necessary IAM roles and policies for the control plane and worker nodes, defining an auto-scaling node group for the EC2 instances, and setting up an associated ECR container registry for our Docker images.

* **Production Environment (`/prod`):** This is the root configuration that calls the VPC and EKS modules, wiring them together by passing the outputs of the VPC (like subnet IDs) as inputs to the EKS module.

## 📊 Monitoring Strategy

A two-pronged approach to monitoring was configured to provide full-stack observability.

1.  **In-Cluster Monitoring (Prometheus & Grafana):** The configuration file in `/monitoring/prometheus-grafana-values.yaml` defines the settings for deploying the `kube-prometheus-stack`, the industry standard for collecting detailed metrics from Kubernetes pods and services. This provides deep insights into application performance.

2.  **Cloud Infrastructure Alerting (AWS CloudWatch):** The Terraform code in the EKS module automatically provisions a CloudWatch Alarm for high CPU utilization on the worker nodes. If the infrastructure is under strain, this alarm triggers a notification via an SNS topic, allowing operations teams to respond proactively.