# Phase 0 - Architecture Design

## 1. 실습 목적

이번 단계에서는 AWS 리소스를 생성하기 전에
3-Tier Architecture의 전체 구조를 먼저 설계하고 이해하는 것을 목표로 한다.

단순 구축이 아니라, 이후 단계에서의 보안 / 확장성 / 가용성까지 고려한
기초 설계 단계를 수행한다.

---

## 2. 실습 목표

* 3-Tier Architecture 개념 이해
* Web / App / DB Layer 역할 구분
* 전체 요청 흐름 이해
* 네트워크 구조 설계
* 프로젝트 전체 방향 설정

---

## 3. 목표 아키텍처

```
User
  ↓
Application Load Balancer (ALB)
  ↓
Web Layer
  ↓
App Layer
  ↓
DB Layer
```

---

## 4. 사용할 AWS 리소스

### 4.1 네트워크

* VPC
* Subnet (Public / Private)
* Internet Gateway
* NAT Gateway
* Route Table

### 4.2 컴퓨팅

* EC2 (Web / App)
* Auto Scaling Group
* Application Load Balancer

### 4.3 데이터베이스

* RDS (MySQL)

### 4.4 모니터링

* CloudWatch Metrics
* CloudWatch Alarm

---

## 5. 네트워크 구성 계획

* Public Subnet (2개)
* Private App Subnet (2개)
* Private DB Subnet (2개)

Multi-AZ 구조를 기반으로 설계하여
가용성과 확장성을 고려한다.

---

## 6. 설계 핵심 포인트

### 6.1 보안

* App / DB는 Private Subnet에 배치
* 외부 접근은 ALB만 허용

### 6.2 확장성

* Web Layer는 Auto Scaling 적용 가능하도록 설계

### 6.3 가용성

* 서로 다른 AZ에 리소스를 분산 배치

### 6.4 관찰 가능성 (Observability)

* Phase 1에서 CloudWatch Alarm을 함께 설정하여
  시스템 상태를 실시간으로 확인 가능하게 한다

---

## 7. 다음 단계

다음 단계에서는 VPC 및 Subnet을 생성하고
3-Tier Architecture의 기반 네트워크를 구축한다.

이 과정에서 네트워크 분리 구조와 라우팅 흐름을 직접 확인한다.
