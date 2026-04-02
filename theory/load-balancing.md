# Load Balancing (ALB & Target Group)

## 1. 개요

Application Load Balancer(ALB)는 외부 요청을 받아
여러 서버(EC2)에 트래픽을 분산하는 역할을 한다.

---

## 2. ALB 역할

* 외부 요청 수신
* Target Group으로 요청 전달
* 트래픽 분산 (Load Balancing)
* Health Check 수행

---

## 3. Target Group

### 정의

* ALB가 요청을 전달할 서버 목록

---

### 구성

```text
Target Group
- EC2 #1
- EC2 #2
- EC2 #3
```

---

### 역할

1. 트래픽 전달

```text
ALB → Target Group → EC2
```

2. 서버 상태 관리 (Health Check)

---

## 4. Health Check

### 목적

* 서버 정상 여부 확인

---

### 동작

```text
ALB → EC2 (/ 요청)
```

* 성공 (200 OK) → healthy
* 실패 → unhealthy

---

### 특징

* unhealthy 서버는 트래픽 제외

---

## 5. 트래픽 흐름

```text
Internet
 ↓
ALB
 ↓
Target Group
 ↓
EC2
```

---

## 6. 포트 개념

* ALB → EC2: 80 포트
* nginx가 요청 처리

---

## 7. 핵심 개념

* ALB는 요청을 "받고 전달"만 함
* 실제 처리는 EC2가 수행
* Target Group은 서버 목록 + 상태 관리

---

## 📌 정리

* ALB = 트래픽 입구
* Target Group = 전달 대상 목록
* EC2 = 실제 서비스 처리
* Health Check = 서버 상태 확인

