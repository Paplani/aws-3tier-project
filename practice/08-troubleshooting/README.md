# Concept Troubleshooting Notes

## 1. NAT Gateway에 대한 오해

### 문제

* 모든 트래픽이 NAT Gateway를 거친다고 생각함

### 원인

* Private Subnet 구조와 NAT 역할에 대한 이해 부족

### 해결

* NAT Gateway는 Outbound 전용이라는 개념 확인
* 트래픽 흐름을 구분하여 이해

```text
사용자 요청:
Internet → ALB → EC2

EC2 outbound:
EC2 → NAT → Internet
```

### 배운점

* NAT는 "밖으로 나갈 때만 사용"
* 외부에서 들어오는 요청에는 사용되지 않음

---

## 2. Security Group에 대한 오해

### 문제

* Security Group이 ALB와 EC2 사이에서만 사용하는 것으로 이해함

### 원인

* Security Group을 특정 연결에만 적용되는 규칙으로 착각

### 해결

* Security Group은 리소스별로 적용되는 방화벽이라는 개념 이해

```text
ALB SG
EC2 SG
Bastion SG
```

### 배운점

* Security Group은 "리소스 단위 방화벽"
* 각 리소스마다 별도로 설정되어야 함

---

## 3. Inbound / Outbound 개념 혼동

### 문제

* Inbound와 Outbound의 역할이 헷갈림
* 왜 Outbound는 기본 허용인지 이해 부족

### 원인

* 트래픽 방향 기준이 아닌 기능 중심으로 생각함

### 해결

* 트래픽 방향 기준으로 개념 재정리

```text
Inbound:
외부 → 내부

Outbound:
내부 → 외부
```

* Outbound는 서버의 외부 통신을 위해 기본 허용됨

### 배운점

* Inbound는 제한이 핵심
* Outbound는 기본 허용이 일반적
* 서버는 외부와 통신해야 동작함

---

## 4. Target Group 개념 이해 부족

### 문제

* "ALB가 보낼 대상"이라는 의미를 이해하지 못함

### 원인

* ALB와 EC2가 직접 연결된다고 생각함

### 해결

* ALB → Target Group → EC2 구조로 이해

```text
ALB
 ↓
Target Group
 ↓
EC2
```

### 배운점

* Target Group은 "요청을 받을 서버 목록"
* ALB는 Target Group을 통해 트래픽을 전달
* Health Check를 통해 정상 서버만 선택됨

---

## 📌 최종 정리

* NAT Gateway → Outbound 전용
* Security Group → 리소스 단위 방화벽
* Inbound / Outbound → 트래픽 방향 기준
* Target Group → ALB의 전달 대상 목록

