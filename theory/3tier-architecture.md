# 3-Tier Architecture

## 1. 개요

3-Tier Architecture는 시스템을 역할에 따라 계층으로 분리하여 구성하는 방식이다.
각 계층은 독립적인 역할을 수행하며, 확장성과 유지보수성을 높이는 구조를 제공한다.

---

## 2. 전체 구조

```text
Internet
 ↓
[Web Layer]
 ↓
[App Layer]
 ↓
[DB Layer]
```

---

## 3. Web Layer

### 역할

* 외부 요청을 수신하는 진입 지점
* 트래픽 분산 (Load Balancing)
* 요청을 내부 서버로 전달

### 구성 요소

* ALB (Application Load Balancer)

### 특징

* Public Subnet에 위치
* 외부와 직접 통신

---

## 4. App Layer

### 역할

* 클라이언트 요청을 처리하는 핵심 로직 수행
* API 처리 및 비즈니스 로직 실행
* 데이터 처리 및 응답 생성

### 구성 요소

* EC2 (Node.js / Express)

---

### 구조

```text
ALB → Target Group → App EC2 (Port 3000)
```

---

### 특징

* Private Subnet에 위치하여 외부 직접 접근 불가
* ALB를 통해서만 접근 가능
* 애플리케이션 포트(3000)를 사용하여 서비스 실행
* Health Check를 통해 서버 상태 관리

---

### Web Layer와의 차이

| 구분 | Web Layer  | App Layer |
| -- | ---------- | --------- |
| 역할 | 요청 수신 및 전달 | 요청 처리     |
| 위치 | Public     | Private   |
| 구성 | ALB        | Node.js   |
| 포트 | 80         | 3000      |

---

## 5. DB Layer

### 역할

* 애플리케이션 데이터 저장 및 관리
* 데이터 조회 및 트랜잭션 처리

### 구성 요소

* RDS (예정)

---

## 6. 트래픽 흐름

```text
1. 사용자 요청 발생
2. ALB가 요청 수신
3. Target Group을 통해 App EC2로 전달
4. App Layer에서 로직 처리
5. 결과 응답 반환
```

---

## 📌 핵심 정리

* Web Layer: 요청을 받는 계층
* App Layer: 요청을 처리하는 계층
* DB Layer: 데이터를 저장하는 계층

👉 각 계층은 역할이 명확히 분리되어야 한다

