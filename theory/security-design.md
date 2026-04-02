# Security Design (Security Group)

## 1. 개요

Security Group은 AWS 리소스에 적용되는 가상 방화벽으로,
트래픽의 허용 여부를 제어한다.

---

## 2. Inbound / Outbound 개념

### Inbound

* 외부 → 내부로 들어오는 트래픽

```text
"누가 나에게 들어올 수 있는가"
```

---

### Outbound

* 내부 → 외부로 나가는 트래픽

```text
"내가 어디로 나갈 수 있는가"
```

---

## 3. 기본 정책

* Inbound: 기본 차단
* Outbound: 기본 허용

---

## 4. 구성된 Security Group

---

### 1) ALB Security Group

```text
Inbound:
HTTP 80 ← 0.0.0.0/0
```

* 외부 사용자 접근 허용

---

### 2) EC2 Security Group

```text
Inbound:
HTTP 80 ← ALB SG
SSH 22 ← Bastion SG
```

* ALB만 웹 요청 가능
* Bastion만 SSH 접속 가능

---

### 3) Bastion Security Group

```text
Inbound:
SSH 22 ← My IP
```

* 관리자만 접속 가능

---

## 5. 보안 흐름

```text
Internet → ALB → EC2
```

```text
Local → Bastion → EC2
```

---

## 6. 핵심 개념

* Security Group은 "트래픽 허용 여부"를 결정
* 리소스 간 접근은 Security Group으로 제한
* 최소 권한 원칙 적용

---

## 📌 정리

* Inbound: 들어오는 트래픽 제어
* Outbound: 나가는 트래픽 제어
* ALB, EC2, Bastion 각각 별도의 SG 사용
* 필요한 경로만 허용
