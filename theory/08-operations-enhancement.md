# Operations Enhancement 이론 정리 (IAM, SSM, Auto Scaling, Instance Refresh)

## 1. IAM (Identity and Access Management)

### 개념

IAM은 AWS에서 **권한을 관리하는 서비스**이다.  
누가 어떤 리소스에 접근할 수 있는지를 정의한다.

### 핵심 역할

- 사용자 / 서비스의 권한 관리
- EC2가 AWS 서비스에 접근할 수 있도록 허용

### EC2에서의 IAM

EC2는 직접 권한을 가지지 않는다.  
→ 대신 **IAM Role을 통해 권한을 부여받는다**

구조:

    EC2 → IAM Role → AWS 서비스

---

## 2. SSM Session Manager

### 개념

SSM(Session Manager)는  
**SSH 없이 EC2에 접속할 수 있게 해주는 서비스**이다.

### 기존 방식 (SSH)

    내 PC → (22번 포트) → Bastion → EC2

문제점:

- 22번 포트 외부 개방 필요
- Key 관리 필요
- Bastion 서버 필요

---

### SSM 방식

    내 PC → AWS SSM → EC2

특징:

- SSH Key 불필요
- 22번 포트 불필요
- Bastion Host 불필요

---

### 어떻게 가능한가?

EC2 내부에 **SSM Agent**가 존재한다.  
이 Agent가 AWS SSM 서비스와 통신한다.

필요 조건:

1. IAM Role (AmazonSSMManagedInstanceCore)
2. 네트워크 (NAT 또는 VPC Endpoint)
3. SSM Agent

---

### 결과적으로 사라지는 것

기존 구성:

- Bastion Host ❌
- SSH Key ❌
- Port 22 ❌

👉 운영 접근 방식이 완전히 바뀜

---

## 3. Launch Template

### 개념

Launch Template은  
**EC2 생성 설정을 저장한 템플릿**이다.

포함 내용:

- AMI
- Instance Type
- Security Group
- IAM Role
- User Data

---

### 왜 필요한가?

기존 방식:

- EC2를 수동으로 생성
- 설정 반복 필요
- 일관성 없음

Launch Template 사용:

    동일한 설정으로 EC2 자동 생성 가능

---

## 4. Auto Scaling Group (ASG)

### 개념

ASG는  
**EC2를 자동으로 생성하고 유지하는 서비스**이다.

---

### 역할

- 원하는 개수 유지 (Desired capacity)
- 장애 발생 시 자동 복구
- 트래픽 증가 시 확장 가능

---

### 구조

    ASG → Launch Template → EC2 생성

---

## 5. Instance Refresh

### 개념

Instance Refresh는  
**ASG 내 EC2를 새로운 설정으로 교체하는 기능**이다.

---

## 🔥 핵심 질문 1: 왜 한 개씩만 교체되는가?

답:

👉 **서비스 중단을 막기 위해서**

---

### 동작 방식

예: EC2 2대 운영 중

1. 기존 인스턴스 1개 종료
2. 새 인스턴스 1개 생성
3. Health Check 통과
4. 나머지 1개 교체

---

### 흐름

    기존 EC2 2개
        ↓
    1개 종료 → 1개 생성
        ↓
    정상 확인
        ↓
    나머지 교체

---

### 결과

👉 항상 최소 1개는 살아있음  
👉 서비스 끊기지 않음 (무중단)

---

## 🔥 핵심 질문 2: 왜 Version 지정이 중요한가?

Launch Template은 버전 기반이다.

문제 상황:

- Version 2 생성
- 하지만 ASG는 Version 1 사용

결과:

👉 변경 내용 반영 안 됨

---

### 해결

- ASG → Launch Template version → latest
- Instance Refresh → 최신 버전 지정

---

## 6. User Data의 역할

### 개념

EC2가 생성될 때 자동으로 실행되는 스크립트

---

### 역할

- 서버 초기 설정 자동화
- 애플리케이션 자동 실행

---

### 중요성

Instance Refresh 시:

👉 새 인스턴스는 User Data 기반으로 자동 구성됨

---

## 7. 전체 구조 변화

### Before

    Bastion → SSH → EC2
    수동 EC2 생성
    수동 배포

---

### After

    SSM → EC2
    ASG → 자동 생성
    Instance Refresh → 자동 교체

---

## 8. 핵심 정리

### 운영 방식 변화

- SSH → SSM
- 수동 생성 → 자동 생성
- 수동 배포 → 무중단 배포

---

### 기술적 핵심

- IAM Role → 권한 관리
- SSM → 안전한 접근
- Launch Template → 설정 표준화
- ASG → 자동화
- Instance Refresh → 배포

---

## 📌 한 줄 정리

    "EC2를 직접 관리하는 구조에서,
     AWS가 자동으로 관리하는 구조로 전환된 단계"
