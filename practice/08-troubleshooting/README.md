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

## 5. RDS Security Group 의 inbound rules 설정중 이해가 안됨

### 문제

RDS Security Group의 Inbound Rule 설정 시  
Source를 무엇으로 설정해야 하는지 명확하지 않았다.

### 원인

문제의 원인은 “DB 접근을 어떤 기준으로 제한해야 하는지”에 대한 이해 부족이었다.

초기에는 IP 기반으로 접근 제어를 생각하였다.

- 0.0.0.0/0 → 모든 IP 허용 (보안 위험)
- VPC CIDR → 내부 모든 리소스 허용 (과도한 접근 허용)

즉, “누가 접근해야 하는지”가 아니라  
“어디서 접근하는지(IP)” 기준으로 접근 제어를 생각한 것이 원인이었다.


### 해결

Source를 App EC2의 Security Group으로 설정하였다.

설정 내용:

- Port: 3306
- Source: app-ec2-sg

이렇게 설정하면 다음과 같은 효과가 있다.

- App Layer에 속한 EC2만 DB 접근 가능
- Bastion Host 및 다른 리소스 접근 차단
- Auto Scaling으로 인스턴스가 늘어나도 동일 SG 사용으로 자동 허용


### 배운 점

DB 접근 제어는 IP 기준이 아니라 “계층(역할)” 기준으로 설계해야 한다.

- DB는 App Layer만 접근해야 한다
- Security Group을 Source로 사용하면 계층 기반 제어 가능
- Auto Scaling 환경에서도 유연하게 대응 가능
- 최소 권한 원칙(Least Privilege)을 적용할 수 있다

👉 핵심:
“누가 접근하는가”를 기준으로 보안을 설계해야 한다.


### 6. Launch Template 변경이 반영되지 않는 문제

#### 문제
- Launch Template Version 2 생성
- Instance Refresh 실행
- 하지만 웹 응답이 변경되지 않음

#### 원인
- Auto Scaling Group이 Launch Template의 default version 사용
- Instance Refresh 시 최신 버전이 아닌 기존 버전 기준으로 실행됨

#### 해결 방법
- ASG → Launch Template version → latest로 변경
- Instance Refresh → desired configuration에서 최신 버전 지정

#### 결과
- Instance 교체 후 웹 응답이 정상적으로 변경됨


## 📌 최종 정리

* NAT Gateway → Outbound 전용
* Security Group → 리소스 단위 방화벽
* Inbound / Outbound → 트래픽 방향 기준
* Target Group → ALB의 전달 대상 목록
* RDS → 관리형 DB로 운영 부담 감소
* 전체 흐름 → Internet → ALB → App → RDS
