# 3-Tier Architecture Overview

## 1. 프로젝트 개요

이 프로젝트는 AWS 기반의 3-Tier Architecture를 직접 설계하고 구축하면서,
보안, 가용성, 확장성, 운영성을 단계적으로 학습하고 문서화하는 것을 목표로 한다.

---

## 2. 3-Tier Architecture란?

3-Tier Architecture는 애플리케이션을 3개의 계층으로 분리하여 구성하는 방식이다.

* Web Layer: 사용자 요청을 받는 계층
* App Layer: 비즈니스 로직을 처리하는 계층
* DB Layer: 데이터를 저장하는 계층

이 구조는 역할 분리, 보안 강화, 확장성 확보를 위해 사용된다.

---

## 3. 전체 아키텍처 구조

```
User
  ↓
Application Load Balancer (ALB)
  ↓
Web Layer (EC2 / Auto Scaling)
  ↓
App Layer (Private EC2)
  ↓
DB Layer (RDS MySQL)
```

---

## 4. 레이어별 역할

### 4.1 Web Layer

* 사용자 요청을 가장 먼저 받는 계층
* ALB를 통해 트래픽을 분산받음
* 정적 콘텐츠 제공 또는 Reverse Proxy 역할 수행

---

### 4.2 App Layer

* 실제 비즈니스 로직을 처리하는 계층
* Web Layer로부터 요청을 전달받음
* DB와 통신하여 데이터 처리 수행

---

### 4.3 DB Layer

* 데이터 저장을 담당하는 계층
* 외부에서 직접 접근 불가
* App Layer만 접근 가능

---

## 5. 네트워크 구조

### 5.1 Public Subnet

* Internet Gateway와 연결
* 외부 인터넷에서 접근 가능

구성:

* ALB
* NAT Gateway

---

### 5.2 Private App Subnet

* 외부에서 직접 접근 불가
* 내부 서비스용 네트워크

구성:

* App EC2

---

### 5.3 Private DB Subnet

* 가장 내부 네트워크
* 외부 접근 완전 차단

구성:

* RDS

---

## 6. 요청 흐름

1. 사용자가 ALB로 요청을 보냄
2. ALB가 Web Layer로 트래픽 전달
3. Web Layer가 App Layer에 요청 전달
4. App Layer가 DB Layer와 통신
5. 결과가 사용자에게 반환됨

---

## 7. 설계 원칙

### 7.1 외부 노출 최소화

외부 접근은 ALB만 허용하고, 내부 리소스는 Private Subnet에 배치한다.

---

### 7.2 레이어 분리

Web / App / DB를 분리하여 구조를 단순화하고 관리성을 높인다.

---

### 7.3 최소 권한 원칙

Security Group을 통해 필요한 통신만 허용한다.

---

### 7.4 고가용성 고려

Multi-AZ 구조를 통해 장애 발생 시에도 서비스 유지가 가능하도록 설계한다.

---

### 7.5 관찰 가능성 확보 (Observability)

초기 단계부터 CloudWatch Alarm을 설정하여
리소스 상태를 지속적으로 모니터링할 수 있도록 한다.

