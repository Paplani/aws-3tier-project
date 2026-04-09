# RDS (Relational Database Service)

## 1. 개념

RDS는 AWS에서 제공하는 관리형 관계형 데이터베이스 서비스이다.

사용자는 직접 DB 서버를 설치하거나 운영할 필요 없이,
AWS가 데이터베이스 인프라를 관리해준다.

대표적으로 지원하는 DB 엔진:

- MySQL
- PostgreSQL
- MariaDB
- Oracle
- SQL Server


## 2. RDS를 사용하는 이유

### 1) 서버 관리 불필요

기존 방식:
- EC2에 DB 설치
- 패치 / 백업 / 장애 대응 직접 수행

RDS:
- AWS가 자동 관리

👉 인프라 운영 부담 감소


### 2) 자동 백업 및 복구

- 자동 백업 기능 제공
- 특정 시점으로 복구 가능 (Point-in-time recovery)

👉 데이터 안정성 확보


### 3) 고가용성 지원

- Multi-AZ 구성 가능
- 장애 발생 시 자동 Failover

👉 서비스 안정성 증가


## 3. RDS와 EC2 DB의 차이

| 항목 | EC2 DB | RDS |
|------|--------|-----|
| 설치 | 직접 설치 | 자동 |
| 백업 | 직접 설정 | 자동 |
| 패치 | 직접 수행 | AWS 관리 |
| 장애 대응 | 수동 | 자동 |
| 운영 난이도 | 높음 | 낮음 |

👉 RDS는 “운영 자동화된 DB”


## 4. RDS 네트워크 구조

RDS는 반드시 VPC 내부에 생성된다.

구성 요소:

- VPC
- Subnet (Private 권장)
- DB Subnet Group
- Security Group

### DB Subnet Group

- RDS가 배치될 Subnet 집합
- 최소 2개 AZ 필요

👉 고가용성 기반 구조


## 5. 왜 RDS를 Private Subnet에 두는가

DB는 외부 사용자에게 직접 노출될 필요가 없다.

접근 구조:

User → ALB → App → DB

즉, DB는 App Layer를 통해서만 접근해야 한다.

### 이유

- 보안 강화
- 공격 표면 감소
- 구조 명확화

👉 DB는 항상 내부 리소스


## 6. Security Group 설계

RDS Security Group:

- Port: 3306 (MySQL)
- Source: App EC2 Security Group

### 왜 SG를 Source로 사용하는가

- 특정 서버 그룹만 허용 가능
- Auto Scaling 환경에서도 유지 가능
- CIDR보다 명확한 접근 제어

👉 계층 기반 보안 설계


## 7. Public Access = No 이유

RDS 설정 중 Public Access 옵션 존재

- Yes: 인터넷에서 직접 접근 가능
- No: VPC 내부에서만 접근 가능

### 이번 설계에서는

👉 Public Access = No

이유:

- DB는 외부 노출 불필요
- 보안 강화
- 3-Tier 구조 유지


## 8. RDS Endpoint 개념

RDS는 IP가 아닌 Endpoint(DNS)로 접근한다.

예시:

my-db.xxxxxx.ap-northeast-2.rds.amazonaws.com

### 특징

- 내부적으로 IP가 변경될 수 있음
- Endpoint는 고정

👉 항상 Endpoint 사용해야 함


## 9. Node.js와 RDS 연결 구조

App EC2 내부에서:

- mysql2 라이브러리 사용
- DB Endpoint로 연결

구조:

Node.js → mysql2 → RDS Endpoint → DB

### 핵심

- host: RDS endpoint
- port: 3306
- user/password: 인증 정보
- database: 사용할 DB


## 10. 연결 테스트 흐름 (중요)

### 1단계: CLI 테스트

    mysql -h <endpoint> -u admin -p

👉 성공 시 네트워크 정상


### 2단계: Node.js 연결

👉 코드 문제 확인


### 이유

문제 발생 시 원인 분리 가능

- CLI 실패 → 인프라 문제
- CLI 성공 + 앱 실패 → 코드 문제


## 11. 이번 실습에서 배운 핵심

- DB는 외부에 노출하지 않는다
- App Layer만 DB에 접근한다
- Security Group은 계층 기준으로 설계한다
- DB는 Subnet Group 기반으로 배치된다
- Endpoint 기반으로 연결한다

👉 핵심은 “연결”이 아니라 “설계”이다
