# DB Layer 이후 학습 로드맵

## Phase 5-1: CRUD 구현

목표:
- App Layer에서 실제 데이터 처리 구현
- DB와 연결된 API 동작 확인

작업 내용:
- 테이블 생성 (users 등)
- Create (INSERT)
- Read (SELECT)
- Update (UPDATE)
- Delete (DELETE)

확인:
- curl 또는 브라우저로 API 테스트
- DB 데이터 반영 확인

---

## Phase 5-2: 환경 변수(.env) 적용

목표:
- DB 정보 및 민감 정보 분리
- 코드와 설정 분리

작업 내용:
- .env 파일 생성
- DB host / user / password / database 분리
- dotenv 적용

확인:
- 기존 코드에서 하드코딩 제거
- .env 기반으로 정상 실행 확인

---

## Phase 6: End-to-End Test

목표:
- 전체 흐름 검증 (ALB → App → DB)

작업 내용:
- CRUD API를 ALB DNS로 테스트
- 데이터 생성 → 조회 → 수정 → 삭제 흐름 검증

확인:
- 외부 요청이 DB까지 정상 반영되는지 확인

---
## 그 다음 과정

Core Build 재구축
IAM + SSM
Launch Template + Instance Refresh
CloudWatch

---

##  HTTPS 적용 (최종 복습)

작업 내용:
- ACM 인증서 발급
- ALB HTTPS 리스너 적용
- Route 53 연결

---

👉 핵심 흐름:
CRUD → .env → E2E → Monitoring → HTTPS
