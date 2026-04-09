# RDS & Database Connection (Node.js)

## 1. RDS 개념

RDS(Relational Database Service)는 AWS에서 제공하는 관리형 관계형 데이터베이스 서비스이다.

사용자는 DB 서버를 직접 설치하거나 관리할 필요 없이,
AWS가 데이터베이스 운영(백업, 패치, 장애 대응)을 자동으로 처리한다.

지원 엔진:

- MySQL
- PostgreSQL
- MariaDB
- Oracle
- SQL Server


## 2. RDS를 사용하는 이유

### 1) 운영 자동화

- DB 설치 불필요
- 패치 자동 적용
- 백업 자동 수행

👉 인프라 운영 부담 감소

---

### 2) 고가용성

- Multi-AZ 구성 가능
- 장애 발생 시 자동 Failover

👉 서비스 안정성 확보

---

### 3) 확장성

- 인스턴스 타입 변경 가능
- 스토리지 확장 가능

👉 트래픽 증가 대응 가능


## 3. RDS 네트워크 구조

RDS는 VPC 내부에 생성된다.

구성 요소:

- VPC
- Subnet
- DB Subnet Group
- Security Group

---

### DB Subnet Group

RDS가 배치될 Subnet들의 집합이다.

특징:

- 최소 2개 AZ 필요
- Private Subnet 권장

👉 고가용성과 네트워크 분리 구조


## 4. 왜 DB는 Private Subnet에 두는가

DB는 외부 사용자에게 직접 노출될 필요가 없다.

구조:

User → ALB → App → DB

### 이유

- 보안 강화
- 공격 표면 감소
- 계층 구조 유지

👉 DB는 내부 리소스로만 사용


## 5. Security Group 설계

RDS Security Group:

- Port: 3306 (MySQL)
- Source: App EC2 Security Group

---

### 왜 Security Group을 Source로 사용하는가

- 특정 계층만 접근 허용
- Auto Scaling 환경에서도 유지 가능
- IP 기반보다 명확한 제어

👉 계층 기반 접근 제어


## 6. Public Access 설정

RDS 생성 시 Public Access 옵션 존재

- Yes → 인터넷에서 접근 가능
- No → VPC 내부에서만 접근 가능

---

### 이번 설계

👉 Public Access = No

이유:

- DB는 외부 노출 불필요
- 보안 강화
- 3-Tier 구조 유지


## 7. RDS Endpoint 개념

RDS는 IP가 아닌 DNS Endpoint로 접근한다.

예시:

my-db.xxxxxx.ap-northeast-2.rds.amazonaws.com

---

### 특징

- IP는 변경될 수 있음
- Endpoint는 고정

👉 항상 Endpoint 사용


## 8. Node.js와 DB 연결 구조

Node.js 애플리케이션은 mysql2 라이브러리를 사용하여 RDS와 연결한다.

구조:

Node.js → mysql2 → RDS Endpoint → Database

---

### 연결 정보

- host: RDS Endpoint
- port: 3306
- user: DB 사용자
- password: 비밀번호
- database: 사용할 DB


## 9. 연결 과정

### 1단계: CLI 테스트

    mysql -h <endpoint> -u admin -p

👉 네트워크 및 보안 설정 확인

---

### 2단계: Node.js 연결

    const mysql = require("mysql2/promise");

👉 애플리케이션 연결 확인


## 10. 왜 CLI 테스트를 먼저 하는가

문제 발생 시 원인을 분리하기 위함이다.

- CLI 실패 → 네트워크 / 보안 문제
- CLI 성공 + 앱 실패 → 코드 문제

👉 디버깅 효율 증가


## 11. 이번 실습 핵심 정리

- DB는 외부에 노출하지 않는다
- App Layer만 DB에 접근한다
- Security Group은 계층 기준으로 설계한다
- DB는 Subnet Group 기반으로 배치된다
- Endpoint 기반으로 연결한다

👉 핵심은 “연결”이 아니라 “설계”이다
