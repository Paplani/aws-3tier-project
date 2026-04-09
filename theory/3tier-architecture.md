# 3-Tier Architecture

## 1. 개요

3-Tier Architecture는 시스템을 역할에 따라 계층으로 분리하여 구성하는 방식이다.
각 계층은 독립적인 역할을 수행하며, 확장성과 유지보수성을 높이는 구조를 제공한다.

이 프로젝트에서는 AWS 환경에서 다음과 같은 구조로 구현하였다.

## 2. 전체 구조

    Internet
     ↓
    [Web Layer]
     ↓
    [App Layer]
     ↓
    [DB Layer]

## 3. Web Layer

### 역할

- 외부 요청을 수신하는 진입 지점
- 트래픽 분산 (Load Balancing)
- 요청을 내부 서버로 전달

### 구성 요소

- ALB (Application Load Balancer)

### 특징

- Public Subnet에 위치
- 외부와 직접 통신
- HTTP 요청을 받아 Target Group으로 전달

## 4. App Layer

### 역할

- 클라이언트 요청을 처리하는 핵심 로직 수행
- API 처리 및 비즈니스 로직 실행
- DB Layer와 통신하여 데이터 처리

### 구성 요소

- EC2 (Node.js / Express)

### 구조

    ALB → Target Group → App EC2 (Port 3000)

### 특징

- Private Subnet에 위치하여 외부 직접 접근 불가
- ALB를 통해서만 접근 가능
- Node.js 애플리케이션이 Port 3000에서 실행
- RDS(MySQL)와 내부 통신 수행
- Health Check를 통해 서버 상태 관리

### Web Layer와의 차이

| 구분 | Web Layer | App Layer |
|------|-----------|-----------|
| 역할 | 요청 수신 및 전달 | 요청 처리 |
| 위치 | Public | Private |
| 구성 | ALB | Node.js |
| 포트 | 80 | 3000 |

## 5. DB Layer

### 역할

- 애플리케이션 데이터 저장 및 관리
- 데이터 조회 및 트랜잭션 처리
- App Layer 요청에 따라 데이터 제공

### 구성 요소

- RDS (MySQL)

### 구조

    App EC2 → RDS (Port 3306)

### 특징

- Private Subnet에 위치 (외부 접근 불가)
- Public Access 비활성화
- App EC2에서만 접근 가능
- Security Group을 통해 접근 제어
- DB Endpoint(DNS)를 통해 연결

### 보안 설계

- RDS Security Group
  - Port: 3306
  - Source: App EC2 Security Group

→ 특정 계층(App Layer)만 DB 접근 가능하도록 제한

## 6. 전체 트래픽 흐름

    1. 사용자 요청 발생
    2. ALB가 요청 수신 (Web Layer)
    3. Target Group을 통해 App EC2로 전달
    4. App Layer에서 비즈니스 로직 처리
    5. 필요 시 RDS에 데이터 요청
    6. RDS가 결과 반환
    7. App Layer가 응답 생성
    8. ALB를 통해 사용자에게 응답 전달

## 7. 계층 분리 이유 (중요)

### 1) 보안

- DB는 외부에 노출되지 않음
- App Layer를 통해서만 접근 가능

### 2) 확장성

- Web / App / DB 각각 독립적으로 확장 가능
- Auto Scaling 적용 가능 (App Layer)

### 3) 유지보수성

- 각 계층 역할이 명확
- 문제 발생 시 원인 분리 가능

### 4) 구조적 안정성

- 트래픽 흐름이 명확함
- 장애 발생 시 영향 범위 제한 가능

## 📌 핵심 정리

- Web Layer: 요청을 받는 계층 (ALB)
- App Layer: 요청을 처리하는 계층 (Node.js)
- DB Layer: 데이터를 저장하는 계층 (RDS)

→ 핵심은 역할 분리와 계층 간 통신 구조이다.
