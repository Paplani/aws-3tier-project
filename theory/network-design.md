# Network Design (VPC, Subnet, NAT, Route Table)

## 1. 개요

AWS VPC 환경에서 Public / Private 네트워크를 구성하고,
각 계층 간의 트래픽 흐름을 설계한다.

---

## 2. Public vs Private Subnet

### Public Subnet

* Internet Gateway(IGW)와 연결된 Subnet
* Public IP를 통해 외부에서 직접 접근 가능
* 사용 예:

  * ALB
  * Bastion Host

---

### Private Subnet

* IGW와 직접 연결되지 않은 Subnet
* 외부에서 직접 접근 불가
* 사용 예:

  * EC2 (App Server)
  * DB

---

## 3. Internet Gateway (IGW)

* VPC와 인터넷을 연결하는 통로
* Public Subnet에서 외부 통신 가능하게 함

```text
Internet ↔ IGW ↔ Public Subnet
```

---

## 4. NAT Gateway

### 역할

* Private Subnet의 EC2가 인터넷으로 나갈 수 있도록 지원

### 특징

* Outbound 전용
* 외부에서 직접 접근 불가

```text
Private EC2 → NAT → IGW → Internet
```

---

## 5. Elastic IP

* 고정된 Public IP
* NAT Gateway가 인터넷 통신 시 사용하는 공인 IP

```text
Internet 입장:
요청 출발지 = NAT의 Elastic IP
```

---

## 6. Route Table

### 역할

* 트래픽이 어디로 이동할지 결정

---

### Public Route Table

```text
0.0.0.0/0 → IGW
```

→ 인터넷으로 직접 연결

---

### Private App Route Table

```text
0.0.0.0/0 → NAT Gateway
```

→ NAT 통해 인터넷 접근

---

### Private DB Route Table

```text
외부 경로 없음
```

→ 완전 격리

---

## 7. 트래픽 흐름

### 1) 사용자 요청

```text
Internet → IGW → ALB → EC2
```

---

### 2) 관리자 SSH 접속

```text
Local → Bastion → Private EC2
```

---

### 3) EC2 Outbound

```text
EC2 → NAT → IGW → Internet
```

---

## 📌 정리

* Public Subnet: 외부 접근 가능
* Private Subnet: 외부 직접 접근 불가
* NAT Gateway: Private의 인터넷 사용 지원
* Route Table: 트래픽 경로 결정
