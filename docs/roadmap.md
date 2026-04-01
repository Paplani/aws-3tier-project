# AWS 3-Tier Project Roadmap

## 프로젝트 목표

이 프로젝트는 AWS 기반 3-Tier Architecture를 구축하면서  
네트워크 설계, 보안, 가용성, 확장성, 모니터링까지 포함한  
실무형 인프라 이해를 목표로 한다.

---

## Phase 0. Architecture Design

### 목표
- 전체 아키텍처 구조 설계
- 레이어별 역할 이해
- GitHub 구조 설계

### 결과물
- architecture-overview.md
- 프로젝트 README 초안

---

## Phase 1. Core Build + Observability Baseline

### 목표
- 3-Tier 핵심 인프라 구축
- CloudWatch Alarm 초기 설정

### 구성
- VPC
- Public Subnet (2개)
- Private App Subnet (2개)
- Private DB Subnet (2개)
- Internet Gateway
- NAT Gateway
- ALB
- EC2 (Web / App)
- RDS MySQL

---

### Observability (초기 설정)

- EC2 CPU Alarm
- Instance Status Check Alarm
- ALB Unhealthy Host Alarm
- RDS 기본 Alarm

---

## Phase 1-1. Security and Troubleshooting

### 목표
- Security Group 설계
- 접근 제어 구조 이해
- 장애 분석 및 해결

### 학습 내용
- SG 간 통신 설계
- ALB → Web → App → DB 흐름 검증
- unhealthy / 연결 실패 분석

---

## Phase 2. Operations Enhancement

### 목표
- 운영 및 관리 기능 고도화

### 구성
- IAM Role
- SSM Session Manager
- Launch Template Update
- Instance Refresh
- CloudWatch Dashboard
- HTTPS (ACM)
- Route 53

---

## Phase 3. Infrastructure as Code

### 목표
- Terraform 기반 인프라 코드화

### 방향
- 별도 레포로 진행 (aws-3tier-terraform)

---

## 전체 진행 흐름

1. Architecture Design
2. Core Build
3. Security / Troubleshooting
4. Operations Enhancement
5. Terraform
