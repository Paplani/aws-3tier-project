# AWS 3-Tier Architecture Project

## 1. Project Overview (프로젝트 개요)

This project focuses on building a 3-Tier Architecture on AWS to understand how real-world cloud infrastructure is designed and operated.

이 프로젝트는 AWS 기반의 3-Tier Architecture를 직접 구축하면서
네트워크 구조, 보안, 확장성, 가용성, 운영 방식을 이해하는 것을 목표로 한다.

---

## 2. Architecture (아키텍처)

```
User
  ↓
Application Load Balancer (ALB)
  ↓
Web Layer (Auto Scaling)
  ↓
App Layer (Private EC2)
  ↓
Database Layer (RDS)
```

This architecture separates responsibilities into different layers to improve security, scalability, and maintainability.

각 레이어를 분리하여 보안성과 확장성을 향상시키는 구조이다.

---

## 3. Tech Stack (기술 스택)

### AWS

* VPC
* EC2
* Application Load Balancer (ALB)
* Auto Scaling Group
* RDS (MySQL)
* CloudWatch
* IAM
* Route 53
* ACM

### OS / Web

* Linux (Ubuntu)
* nginx

---

## 4. Project Structure (프로젝트 구조)

```
aws-3tier-project/
├── README.md
├── docs/
├── theory/
├── practice/
└── weekly-logs/
```

---

## 5. Documentation

### Architecture

* [Architecture Overview](./docs/architecture-overview.md)

### Roadmap

* [Project Roadmap](./docs/roadmap.md)

---

## 6. Practice

* [Phase 0 - Architecture Design](./practice/01-architecture-design)

(Next steps will be added progressively)

---

## 7. Key Learning Goals (학습 목표)

* Understand why 3-Tier Architecture is used
* Design secure network structures using Public and Private subnets
* Build scalable infrastructure using Load Balancer and Auto Scaling
* Monitor system behavior using CloudWatch
* Analyze and troubleshoot real-world issues

---

## 8. Progress (진행 상황)

* Phase 0: Architecture Design (In Progress)
* Phase 1: Core Infrastructure Build (Planned)
* Phase 2: Operations Enhancement (Planned)
* Phase 3: Infrastructure as Code (Planned)

---

## 9. Notes

This repository is built as a hands-on learning project and portfolio.

이 프로젝트는 실습 기반 학습 및 포트폴리오를 목적으로 작성되었다.
