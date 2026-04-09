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

## Phase 7: CloudWatch (기본 모니터링)

목표:
- 인프라 상태 확인

작업 내용:
- EC2 CPU 확인
- ALB 요청/응답 확인
- RDS 상태 확인
- Auto Scaling 동작 확인

---

## Phase 8: CloudWatch (운영 관점)

목표:
- 장애 대응 및 모니터링 체계 구축

작업 내용:
- Dashboard 생성
- Alarm 설정 (CPU, Health Check 등)
- 로그 수집 구조 설계

---

## Phase 9: 보안 및 운영 개선

작업 내용:
- .env 보안 강화
- IAM Role 적용
- SSM Session Manager 적용

---

## Phase 10: HTTPS 적용 (최종 복습)

작업 내용:
- ACM 인증서 발급
- ALB HTTPS 리스너 적용
- Route 53 연결

---

👉 핵심 흐름:
CRUD → .env → E2E → Monitoring → HTTPS
